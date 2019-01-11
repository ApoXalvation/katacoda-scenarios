Config servers store the metadata for a sharded cluster.<br>
The metadata reflects state and organization for all data and components within the sharded cluster.<br>
The metadata includes the list of chunks on every shard and the ranges that define the chunks.<br>

## Task

Copy kubernetes configuration files to our working direcotry:<br>
`cp /tmp/* ./`{{execute HOST1}}<br>

## Description
Open configsvr.yaml:<br>
`less configsvr.yaml`{{execute HOST1}}<br>
Let me describe the content of configuration.<br>
The very first part:<br>
`apiVersion: v1
kind: Service
metadata:
  name: configsvr
  namespace: mongo
  labels:
    name: configsvr
spec:
  ports:
  - port: 27019
    targetPort: 27019
  clusterIP: None
  selector:
    role: configsvr`<br>
We use stable kubernetes *api v1* to create *Service* which provides us port 27019 to our kubernetes enverinoment, more about pros od this a while.<br>
`apiVersion: apps/v1beta2
kind: StatefulSet
metadata:
  name: configsvr
  namespace: mongo`<br>
Next We use *beta api* version becouse our *StatefulSet* will use nfs persisten volume claims which are not present in stable api yet.<br>
`spec:
  selector:
    matchLabels:
      role: configsvr
  serviceName: configsvr`<br>
And becouse We append this *StatefulSet* to *service configsvr* We expose port 27019 inside our kubernetes, so other containers can connect to mongo config server.<br>
`  replicas: 3`<br>
We launch *three replicas* becouse We need to create mongodb replicaset to achive high avability and becouse mongo do not allow create stand alone nodes for shards nor config server since mongo 3.6.<br>
`    spec:
      terminationGracePeriodSeconds: 10
      containers:
        - name: configsvr-container
          image: mongo
          command:
            - "mongod"
            - "--port"
            - "27019"
            - "--replSet"
            - "configsvr"
            - "--bind_ip"
            - "0.0.0.0"
            - "--configsvr"
            - "--dbpath"
            - "/mongo-disk"
          ports:
            - containerPort: 27019
          volumeMounts:
            - name: data
              mountPath: /mongo-disk`<br>
We are going to use mongo image to deploy our containers and execute execute inside them:<br>
`mongod --port 27019 --replSet configsvr --bind_ip 0.0.0.0 --configsvr --dbpath /mongo-disk`
We expose on *container port 27019* and then We would like to mount volume named *data* to as */mongo-disk* to achive that We need to request our nfs provisioner to create thats volumes (one for each replica).<br>
`  volumeClaimTemplates:
  - metadata:
      name: data
      annotations:
        volume.beta.kubernetes.io/storage-class: "managed-nfs-storage"
    spec:
      accessModes: [ "ReadWriteMany" ]
      resources:
        requests:
          storage: 1Gi`<br>
The *volume.beta.kubernetes.io/storage-class* is that part becouse of which We had to use beta api, and then We can set it to our nfs provider *managed-nfs-storage*. We set *access mode  as read and write* and *request storage 1Gi*.<br>
<br>
