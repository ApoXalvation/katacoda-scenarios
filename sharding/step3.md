Install'n'configure nfs-client-provisioner using helm.
If You want to know more about <a href="https://helm.sh/">Helm - package manager</a> or nfs-client-provisioner <a href="https://github.com/helm/charts/tree/master/stable/nfs-client-provisioner">this</a> should be the good place to start
## Task

Run below commands for init helm and extend its permissions, so Helm can deploy for us whole nfs provisioner:
`helm init &&
kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default &&
helm install --set nfs.server=[[HOST2_IP]] --set nfs.path=/root/nfs-share stable/nfs-client-provisioner`{{execute HOST1}}
This charts installs custom storage class into a Kubernetes cluster using the Helm package manager. It also installs a NFS client provisioner into the cluster which dynamically creates persistent volumes from single NFS share.
