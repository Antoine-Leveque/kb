+++
title = 'Storage'
date = 2024-01-24T09:27:43+01:00
draft = false
+++

# PVC to POD

```bash
kubectl -n ${NAMESPACE} get pods -o=json | jq -c '.items[] | {name: .metadata.name, namespace: .metadata.namespace, claimName: .spec |  select( has ("volumes") ).volumes[] | select( has ("persistentVolumeClaim") ).persistentVolumeClaim.claimName }'
```
