Now it is time to create replicasets for configsvr, shard1 and shard2, and in the end attach shards to cluster of MongoDB
## Task
Initiate configsvr replicaset 
`
k exec -n mongo configsvr-0 -c configsvr-container -- mongo --port 27019 --eval "rs.initiate( { _id: \"configsvr\", configsvr: true, members: [ { _id : 0, host : \"configsvr-0.configsvr:27019\" }, { _id : 1, host : \"configsvr-1.configsvr:27019\" }, { _id : 2, host : \"configsvr-2.configsvr:27019\" } ] } )"
`{{execute HOST1}}

Initiate shard1 replicaset
`
k exec -n mongo shard1-0 -c shard1-container -- mongo --port 27018 --eval "rs.initiate( { _id: \"shard1\", members: [ { _id : 0, host : \"shard1-0.shard1:27018\" }, { _id : 1, host : \"shard1-1.shard1:27018\" }, { _id : 2, host : \"shard1-2.shard1:27018\" } ] } )"
`{{execute HOST1}}

Initiate shard2 replicaset
`
k exec -n mongo shard2-0 -c shard2-container -- mongo --port 27018 --eval "rs.initiate( { _id: \"shard2\", members: [ { _id : 0, host : \"shard2-0.shard2:27018\" }, { _id : 1, host : \"shard2-1.shard2:27018\" }, { _id : 2, host : \"shard2-2.shard2:27018\" } ] } )"
`{{execute HOST1}}

Add shards to a Cluster
`
k exec -n mongo mongos-0 -- mongo --port 27017 --eval "sh.addShard(\"shard1/shard1-0.shard1:27018\");"
`{{execute HOST1}}
`
k exec -n mongo mongos-0 -- mongo --port 27017 --eval "sh.addShard(\"shard2/shard2-0.shard2:27018\");"
`{{execute HOST1}}

## CHECK
Check replicasets
`
k exec -n mongo configsvr-0 -c configsvr-container -- mongo --port 27019 --eval "rs.status();"
`{{execute HOST1}}
`
k exec -n mongo shard1-0 -c configsvr-container -- mongo --port 27018 --eval "rs.status();"
`{{execute HOST1}}
`
k exec -n mongo shard2-0 -c configsvr-container -- mongo --port 27018 --eval "rs.status();"
`{{execute HOST1}}
Check sharded MongoDB cluster
`
k exec -n mongo mongos-0 -- mongo --port 27017 --eval "sh.status();"
`{{execute HOST1}}
