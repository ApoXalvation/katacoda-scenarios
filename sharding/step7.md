MongoDB mongos instances route queries and writes operations to shards in a sharded cluster.<br>
mongos provide the only interface to a sharded cluster from the perspective of applications.<br>
Applications never connect or communicate directly with the shards.<br>
<br>
_<a href="https://docs.mongodb.com/manual/core/sharded-cluster-query-router/">click</a> - for more info about mongos_
## Description
Open mongos.yaml:<br>
`cat mongos.yaml`{{execute HOST1}}<br>
This config is less complicated than any previous, it does not need persistent volume because all data is cached into ram.<br>
We create service for mongos, then the StatefulSet with two replicas, then launch mongos, with addresses of MongoDB config servers replicaset, and listen on port 27017. And in this case, pods with mongos instances should be propagated on each node, same as configsvr.<br>
## Task
To deplay MongoS run:<br>
`k apply -f mongos.yaml`{{execute HOST1}}<br>
To watch the process use:<br>
`watch -n1 'kubectl get pods -n mongo -o wide; kubectl get pvc -n mongo'`{{execute HOST1}}<br>
Type <kbd>ctr</kbd>+<kbd>c</kbd> to stop watching.
