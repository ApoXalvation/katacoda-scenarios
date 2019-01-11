This charts installs custom storage class into a Kubernetes cluster using the Helm package manager. It also installs a NFS client provisioner into the cluster which dynamically creates persistent volumes from single NFS share.<br><br>
If You want to know more about <a href="https://helm.sh/">Helm - package manager</a> or nfs-client-provisioner <a href="https://github.com/helm/charts/tree/master/stable/nfs-client-provisioner">this</a> should be the good place to start
## Task

Run below commands for init helm:<br>
`helm init`{{execute HOST1}}<br>
extend its permissions, so Helm can deploy for us whole nfs provisioner:<br>
`kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default`{{execute HOST1}}<br>
then install and configure stable nfs-client-provisioner package:<br>
`helm install --set nfs.server=[[HOST2_IP]] --set nfs.path=/ --name nfs-provisioner stable/nfs-client-provisioner`{{execute HOST1}}<br>
check if all is ready:
`k get nodes`{{execute HOST1}}
`k get pods`{{execute HOST1}}
