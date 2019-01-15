A shard contains a subset of sharded data for a sharded cluster.<br>
Together, the cluster’s shards hold the entire data set for the cluster.<br>
## Description
Open shard1.yaml:<br>
`cat shard1.yaml`{{execute HOST1}}<br>
This file and second one `shard2.yaml` have  common body with a previous one - configsvr.yaml.<br>
We create service for both shards, then the StatefulSet with three replicas, then launch mongod, but this time with the `--shardsvr` parameter, and listen on port 27018. In the end We create and attach persistent volume claims to each pod.<br>
Let's try to put each shard on differ node: shard1 = master<br>
`      nodeSelector:
        kubernetes.io/hostname : master`<br>
and shard2 = node01:<br>
`      nodeSelector:
        kubernetes.io/hostname : node01`<br>
<br>
## Task
To deploy MongoDB shards run:<br>
`k apply -f shard1.yaml`{{execute HOST1}}<br>
`k apply -f shard2.yaml`{{execute HOST1}}<br>
To watch the process use:<br>
`watch -n1 'kubectl get pods -n mongo -o wide; kubectl get pvc -n mongo'`{{execute HOST1}}<br>
Type <kbd>ctr</kbd>+<kbd>c</kbd> to stop watching.
