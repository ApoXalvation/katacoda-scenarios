A shard contains a subset of sharded data for a sharded cluster.<br>
Together, the cluster’s shards hold the entire data set for the cluster.<br>
## Description
Open shard1.yaml:<br>
`cat shard1.yaml`{{execute HOST1}}<br>
This file and second one `shard2.yaml` has very common body with previous one - configsvr.yaml.<br>
We create service for both shards, then the StatefulSet with three replicas, then launch mongod, but this time with the `--shardsvr` parameter, and listen on port 27018. On the end We create and attach persistend volume claims to each pod.
<br>
## Task
To deplay MongoDB shards run:<br>
`k apply -f shard1.yaml`{{execute HOST1}}<br>
`k apply -f shard2.yaml`{{execute HOST1}}<br>
To watch the process use:
`watch -n1 'kubectl get all -n mongo; kubectl get pvc -n mongo'`{{execute HOST1}}
Type <kbd>ctr</kbd>+<kbd>c</kbd> to stop watching.
