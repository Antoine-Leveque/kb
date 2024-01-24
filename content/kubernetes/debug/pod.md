+++
title = 'Pod'
date = 2024-01-24T15:45:50+01:00
draft = false
+++

### Running one time pod for debuging purpose

```bash
kubectl --context {CONTEXT} -n ${NAMESPACE} run -i --tty --rm ${POD_NAME} --image=${IMAGE}  --labels "label=1","label=2" --restart=Never -- sh
```
