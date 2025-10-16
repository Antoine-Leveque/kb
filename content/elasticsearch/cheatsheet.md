+++
title = 'Cheatsheet'
date = 2025-07-25T14:43:00+02:00
draft = true
+++

# Elasticsearch Cheatsheet

## Table of contents
- [Elasticsearch Cheatsheet](#elasticsearch-cheatsheet)
  - [Useful links](#useful-links)
  - [Useful informations](#useful-informations)
  - [Useful commands](#useful-commands)
    -[Kubernetes commands](#kubernetes-commands)
    - [Cluster related commands](#cluster-related-commands)
    - [Index and shard related commands](#index-and-shard-related-commands)
    - [Tasks related command](#tasks-related-commands)
    - [Snapshots related commands](#snapshots-related-commands)
    - [Miscellaneous commands](#miscellaneous-commands)

- - -

## Useful informations

Elasticsearch default transport port (between ES nodes): `9300`  
Elasticsearch default http port (api): `9200`  
Elasticsearch operator namespace: `elastic-system`  
Elasticsearch ingress URL format: `elastic.namespace.kubecluster.datacenter.kube.qwant.ninja`  
Kibana ingress URL format: `kibana.namespace.kubecluster.datacenter.kube.qwant.ninja`  

- - -

## Useful commands

### Kubernetes Commands

List all Elasticsearch clusters from a kubernetes kibana:
```bash
kubectl get elasticsearch -A
```
```bash
NAMESPACE             NAME              HEALTH   NODES   VERSION   PHASE   AGE
int-es-apm            es-apm            green    9       8.15.0    Ready   181d
int-es-eval           es-eval           green    6       8.15.0    Ready   203d
int-es-ia-gen         es-ia-gen         green    5       8.15.0    Ready   193d
int-es-modules        es-modules        green    6       8.15.0    Ready   224d
int-es-news           es-news           green    5       8.15.0    Ready   208d
int-es-newspartners   es-newspartners   green    7       7.17.16   Ready   228d
```

List all kibanas from a kubernetes context:
```bash
kubectl get kibana -A
```

### Cluster related commands

Display elasticsearch cluster nodes:
```bash
curl https://elasticsearch.endpoint:port/_cat/nodes?pretty
```

Display list of indices on an elasticsearch cluster:
```bash
curl https://elasticsearch.endpoint:port/_cat/indices?pretty
```

Display cluster health:
```bash
curl https://elasticsearch.endpoint:port/_cluster/health?pretty
```
### Index and shard related commands

Disable all shard allocation but primaries:
```bash
curl -XPUT https://elasticsearch.endpoint:port/_cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.enable": "primaries"
  }
}
```

Enable all shard allocation:
```bash
curl -XPUT https://elasticsearch.endpoint:port/_cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.enable": "all"
  }
}
```

Reindex an index:
```bash
curl -X POST "https:/elasticsearch.endpoint:port/_reindex" -H 'Content-Type: application/json' -d'
{
  "source": {
    "index": "my-index-000001"
  },
  "dest": {
    "index": "my-new-index-000001"
  }
}
'
```

Close an index:
```bash
curl -XPOST https://elasticsearch.endpoint:port/indexname/_close
```

Open an index:
```bash
curl -XPOST https://elasticsearch.endpoint:port/index_name/_open
```

Describe shard allocation problems:
```bash
curl -X GET "https://elasticsearch.endpoint:port/_cluster/allocation/explain?pretty" -H 'Content-Type: application/json' -d'
{
  "index": "my-index-000001",
  "shard": 0,
  "primary": true
}
'
```

Retry failed shard allocation:
```bash
curl -X POST https://https://elasticsearch.endpoint:port//_cluster/reroute?retry_failed=true?pretty
```

### Tasks related commands

Show running tasks:

```bash
curl -XGET https://elasticsearch.endpoint:port/_tasks
```

Display specific task details:
```bash
curl -XGET https://elasticsearch.endpoint:port/_tasks/taskid
```


### Snapshots related commands
Set snapshot repository settings:
```bash
curl -XPUT https://elasticsearch.endpoint:port/_snapshot/s3_repository
{
  "type": "s3",
  "settings": {
    "bucket": "<bucket-name>",
    "client": "<client-name>"
  }
}
```

Running a snapshot for specific index:
```bash
curl -XPUT 'https:/elasticsearch.endpoint:port/_snapshot/<repo>/snapshot_name?wait_for_completion=true' \
 -d '{"indices":"XXXXX,XXXXX",
 "ignore_unavailable":true,
"include_global_state":false,
"metadata":{"taken_by":"x.xxx",
"taken_because":"because I can"}}'
```

Running a snapshot of all indices:
```bash
curl -XPUT 'https:/elasticsearch.endpoint:port/_snapshot/<repo>/snapshot_name'
```

Get snapshot status/restore progress:
```bash
curl -XGET 'https:/elasticsearch.endpoint:port/_snapshot/<repo>/snapshot_name?pretty'`
```
or
```bash
curl -XGET 'https:/elasticsearch.endpoint:port/_snapshot/<repo>/snapshot_name/_status?pretty'
```

Stop snapshot operation (Deleting the corresponding snapshot will stop all operations):
```bash
curl -XDELETE 'https://elasticsearch.endpoint:port/_snapshot/<repo>/snapshot_name?pretty'
```

### Miscellaneous commands

Exclude data nodes for allocation:
```bash
curl -X PUT "https://elasticsearch.endpoint:port/_cluster/settings" -H 'Content-Type: application/json' -d'
{
  "persistent" : {
    "cluster.routing.allocation.exclude._name" : "data-node01,data-node02"
  }
}
'
```
To remove exclude configuration:
```bash
curl -X PUT "https://elasticsearch.endpoint:port/_cluster/settings" -H 'Content-Type: application/json' -d'
{
  "persistent" : {
    "cluster.routing.allocation.exclude._name" : null
  }
}
'
```
To view exclude configuration:
```bash
curl -X GET "https://elasticsearch.endpoint:port/_cluster/settings" -H 'Content-Type: application/json'
```

