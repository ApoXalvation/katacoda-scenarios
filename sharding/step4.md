We have solid basement for MongoDB:<br>
 - We had bootstrap a Kubernetes cluster<br>
 - We had deploy NFS server<br>
 - We had int Helm package manager and...<br>
 - ...We used it to deploy nfs-client-provisioner<br>

Now it is time for Mongo, but before that You should take a look at how many things We have running now to achive this basement<br>
`kubectl get all --all-namespaces`{{execute HOST`}}<br>
What we do now is create a separate namespace for MongoDB to have things organized<br>
## Task

We will have a few yaml templates in a moment so it is a good idea to create new working directory:<br>
`mkdir /root/mongodb; cd /root/mongodb`{{execute HOST1}}
Now We want to create a new namespace for our MongoDB deplayment:
`echo "
apiVersion: v1
kind: Namespace
metadata:
  name: mongo
" > namespace.yaml`{{execute HOST1}}<br>
From now We will use _k_ except for kubectl to make things faster:<br>
`k apply -f namespace.yaml`{{execute HOST1}}<br>
Now We can check if our new namespace for mongo only was created and what is inside:<br>
`k get namepsaces; k get all -n mongo`{{execute HOST1}}<br>
