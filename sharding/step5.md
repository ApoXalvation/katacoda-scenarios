Config servers store the metadata for a sharded cluster. The metadata reflects state and organization for all data and components within the sharded cluster. The metadata includes the list of chunks on every shard and the ranges that define the chunks.

## Task

Deploy three replicas of MongoDB config server using below command, in next step We analize this template.<br>

`echo '
apiVersion: v1
kind: Service
metadata:
  name: configsvr
  namespace: mongo
  labels:
    name: mongodb-configsvr
spec:
  ports:
  - port: 27019
    targetPort: 27019
  clusterIP: None
  selector:
    role: mongodb-configsvr
---
apiVersion: apps/v1beta2
kind: StatefulSet
metadata:
  name: configsvr
  namespace: mongo
spec:
  selector:
    matchLabels:
      role: mongodb-configsvr
  serviceName: configsvr
  replicas: 3
  template:
    metadata:
      labels:
        role: mongodb-configsvr
        tier: configsvr
    spec:
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
              mountPath: /mongo-disk
  volumeClaimTemplates:
  - metadata:
      name: data
      annotations:
        volume.beta.kubernetes.io/storage-class: "managed-nfs-storage"
    spec:
      accessModes: [ "ReadWriteMany" ]
      resources:
        requests:
          storage: 1Gi
' > configsvr.yaml;
k apply -f configsvr.yaml`{{execute HOST1}}<br>
