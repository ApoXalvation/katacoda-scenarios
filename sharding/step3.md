This chart installs custom storage class into a Kubernetes cluster using the Helm, package manager. It also installs the NFS client provisioner into the cluster which dynamically creates persistent volumes from single NFS share.<br><br>
_If You want to know more about Helm - package manager or nfs-client-provisioner <a href="https://helm.sh/">this</a> should be the best place to start_

## Task

Run below commands for init helm:<br>
`helm init`{{execute HOST1}}<br>
extend its permissions so that Helm can deploy for us whole nfs provisioner:<br>
`kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default`{{execute HOST1}}<br>
then install and configure stable nfs-client-provisioner package:<br>
`helm install --set nfs.server=[[HOST2_IP]] --set nfs.path=/root/nfs-share --name nfs-provisioner stable/nfs-client-provisioner`{{execute HOST1}}<br>
If You received `Error: could not find a ready tiller pod` it is because helm init does not finish yet, try again in few seconds.<br><br>
Check if all is ready:<br>
`kubectl get nodes`{{execute HOST1}}<br>
`kubectl get pods`{{execute HOST1}}<br>
