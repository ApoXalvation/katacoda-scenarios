Install'n'configure nfs-client-provisioner using helm.
If You want to know more about helm and nfs-client-provisioner <a href="https://github.com/helm/charts/tree/master/stable/nfs-client-provisioner">this</a> should be the good place to start
## Task

Run below command:
`helm install --set nfs.server=[[HOST2_IP]] --set nfs.path=/root/nfs-share stable/nfs-client-provisioner`{{execute HOST1}}
This charts installs custom storage class into a Kubernetes cluster using the Helm package manager. It also installs a NFS client provisioner into the cluster which dynamically creates persistent volumes from single NFS share.
