+++
title = 'Resize_pvc'
date = 2025-10-20T17:44:37+02:00
draft = false
+++

# Resize PVC dans kubernetes

Dans le cas où un PVC est plein, et que celui-ci n'est pas géré par un operator (CNPG, Victoria Metrics...), il est nécessaire d'appliquer ce mode opératoire pour augmenter la taille du PVC. En effet, la taille est un élément immuable dans le Statefulset.

## Prérequis
- Accès au cluster kubernetes concerné (kubectl, k9s, lens...) et PIM Azure

## Mode opératoire

### Désactiver la synchonisation automatique

Pour pouvoir manipuler les objets kubernetes à notre guise pendant l'opération, il faut désactiver la synchronisation automatique d'ArgoCD.

#### ApplicationSet

Dans l'applicationSet, passer les valeurs de `selfHeal` et `prune` à `false`.

```yaml
syncPolicy:
  managedNamespaceMetadata:
    labels:
  syncOptions:
    - RespectIgnoreDifferences=true
    - CreateNamespace=true
  automated:
    selfHeal: false
    prune: false
```

Une fois cette modification faite, pousser et merge cette modification pour que le paramètre soit pris en compte.  

#### WebUI

Depuis l'interface web d'ArgoCD, dans l'application, une fois les modifications prises en compte, cliquer sur `Details` puis sur `DISABLE AUTO-SYNC` dans la partie "Sync Policy".  

### Edition du PVC

Maintenant que la synchro auto est désactivée, on peut éditer les objets kubernetes.  

Exemple avec kubectl : 

Lister les PVC dans le namespace :
```bash
kubectl --context $context --namespace $namespace get pvc
```

```bash
kubectl --context $context --namespace $namespace edit pvc $nom_du_pvc
```
Le manifest "runtime" du PVC va s'ouvrir dans l'éditeur par défaut.  
Changer la valeur souhaitée. Exemple : 

```yaml
spec:
  resources:
    requests:
      storage: 1Gi
```

Ou en une ligne : `kubectl --context $CONTEXT -n $NAMESPACE patch pvc $PVC_NAME -p '{"spec": {"resources": {"requests": {"storage":"$NEW_STO"}}}}'`


### Suppression du StatefulSet 

Pour que le changement de valeur soit pris en compte, il faut supprimer le StatefulSet duquel le PVC dépend sans supprimer les pods et PVC associés.

```bash
kubectl --context $context --namespace $namespace delete sts $nom_du_sts --cascade=orphan
```

**Attention** Il est très important d'indiquer l'argument `--cascade=orphan`. Auquel cas, les pods et PVC liés au STS seront **supprimés**.

### Modification des valeurs sur gitlab

Ajustez les valeurs correspondantes à la capacité de votre PVC. 

### Réactivation de la synchronisation

Remettre les valeurs de `selfHeal` et `prune` à `true` dans l'applicationSet.

Pousser les modifications et synchroniser l'application dans ArgoCD.  

Les valeurs de l'objet dans kubernetes et dans gitlab correspondent.


