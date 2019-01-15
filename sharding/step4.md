We have a solid basement for MongoDB:<br>
 - Kubernetes cluster<br>
 - NFS server<br>
 - nfs-client-provisioner<br>

Now it is time for Mongo, but before that, You should take a look at how many things We have running now.<br>
`kubectl get all --all-namespaces`{{execute HOST1}}<br>
Create a separate namespace for MongoDB to have things organised <br>
## Task

We will have a few yaml templates in a moment, so it is a good idea to create new working directory:<br>
`mkdir /root/mongodb; cd /root/mongodb`{{execute HOST1}}<br>
Now We want to create a new namespace for our MongoDB deployment:<br>
`echo "
apiVersion: v1
kind: Namespace
metadata:
  name: mongo
" > namespace.yaml`{{execute HOST1}}<br>
From now We will use _k_ except for kubectl to make things faster:<br>
`k apply -f namespace.yaml`{{execute HOST1}}<br>
Now We can check if our new namespace for mongo only was created and what is inside:<br>
`k get ns`{{execute HOST1}}<br>
Create new volumen `secret` and generate kay for MongoDB:
`openssl rand -base64 741 > mongodb-keyfile`{{execute HOST1}}<br>
`kubectl create secret generic mongokeyfile --from-file=./mongodb-keyfile`{execute HOST1}}<br>{
`k get all -n mongo`{{execute HOST1}}<br>
