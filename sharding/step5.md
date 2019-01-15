Config servers store the metadata for a sharded cluster.<br>
The metadata reflects state and organisation for all data and components within the sharded cluster.<br>
The metadata includes the list of chunks on every shard and the ranges that define the chunks.<br>

## Task

Copy Kubernetes configuration files to our working directory:<br>
`cp /tmp/* ./`{{execute HOST1}}<br>

## Description
Open configsvr.yaml:<br>
`cat configsvr.yaml`{{execute HOST1}}<br>
Let me describe the content of the configuration.<br>
The very first part:<br>
`
‎ apiVersion: v1
‎ kind: Service
‎ metadata:
‎   name: configsvr
‎   namespace: mongo
‎   labels:
‎     name: configsvr
‎ spec:
‎   ports:
‎   - port: 27019
‎     targetPort: 27019
‎   clusterIP: None
‎   selector:
‎     role: configsvr
`
<br>
We use stable Kubernetes api v1 to create Service which provides us port 27019 to our Kubernetes environment, more about pros od this a while.<br>
`apiVersion: apps/v1beta2
kind: StatefulSet
metadata:
  name: configsvr
  namespace: mongo`<br>
Next, We use beta api version because our StatefulSet will use nfs persistent volume claims which are not present in stable api yet.<br>
`
‎ spec:
‎   selector:
‎     matchLabels:
‎       role: configsvr
‎   serviceName: configsvr
`<br>
And because We append this StatefulSet to service configsvr, We expose port 27019 inside our Kubernetes, so other containers can connect to mongo config server.<br>
`
‎   replicas: 3
`<br>
We launch three replicas because We need to create MongoDB replicaset to achieve high availability and because mongo does not allow to create stand-alone nodes for shards nor config server since MongoDB 3.6.<br>
`
‎     spec:
‎       affinity:
‎         podAntiAffinity:
‎           preferredDuringSchedulingIgnoredDuringExecution:
‎           - weight: 100
‎             podAffinityTerm:
‎               labelSelector:
‎                 matchExpressions:
‎                 - key: role
‎                   operator: In
‎                   values:
‎                   - configsvr
‎               topologyKey: kubernetes.io/hostname
`<br>
It is time for affinity part, in this case, pods with configsvr instances should be propagated on each node - for that, We check if label role with value configsvr exists on the node, and if it is, try to take another node. If all nodes have this label set then, because it is the "preferred" policy, put it anywhere.<br>
`
‎       terminationGracePeriodSeconds: 10
‎       containers:
‎         - name: configsvr-container
‎           image: mongo
‎           command:
‎             - "mongod"
‎             - "--port"
‎             - "27019"
‎             - "--replSet"
‎             - "configsvr"
‎             - "--bind_ip"
‎             - "0.0.0.0"
‎             - "--configsvr"
‎             - "--dbpath"
‎             - "/mongo-disk"
‎           ports:
‎             - containerPort: 27019
‎           volumeMounts:
‎             - name: data
‎               mountPath: /mongo-disk
`<br>
We are going to use mongo image to deploy our containers and execute inside them:<br>
`mongod --port 27019 --replSet configsvr --bind_ip 0.0.0.0 --configsvr --dbpath /mongo-disk`
We expose on container port 27019, and then We would like to mount volume named data to as /mongo-disk to achieve that We need to request our nfs provisioner to create those volumes (one for each replica).
`
‎   volumeClaimTemplates:
‎   - metadata:
‎       name: data
‎       annotations:
‎         volume.beta.kubernetes.io/storage-class: "managed-nfs-storage"
‎     spec:
‎       accessModes: [ "ReadWriteMany" ]
‎       resources:
‎         requests:
‎           storage: 1Gi
`<br>
The volume.beta.kubernetes.io/storage-class that part where We set our nfs provider nfs-client.<br>
You can check available storage classes using
`k get sc`{{execute HOST1}}.
We set access mode as read and write and request storage 1Gi.<br>
<br>
## Task
Let's deploy the MongoDB config servers.<br>
`k apply -f configsvr.yaml`{{execute HOST1}}<br>
You may watch the process of creating pods and persistent volume claims for them, using below command:
`watch -n1 'kubectl get pods -n mongo -o wide; kubectl get pvc -n mongo'`{{execute HOST1}}<br>
Type <kbd>ctr</kbd>+<kbd>c</kbd> to stop watching.
