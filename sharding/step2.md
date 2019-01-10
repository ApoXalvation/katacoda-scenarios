
We will need nfs server for persistent volume claims for our shards, so lets configure one on HOST2

## Task
Update and install nfs-kernel-server on host2:<br>
`apt-get update && apt-get install nfs-kernel-server`{{execute HOST2}}<br>
and nfs client on host1:<br>
`apt-get update && apt-get install nfs-common`{{execute HOST1}}<br>
then prepare shared sirectory (host2):<br>
`mkdir /root/nfs-share`{{execute HOST2}}<br>
define what we want to share and who may connect(host2):<br>
`echo "/root/nfs-share [[HOST1_IP]]/32(rw,fsid=0,insecure,no_subtree_check,async)" >> /etc/exports`{{execute HOST2}}<br>
`echo "portmap: [[HOST1_IP]]/32" >> /etc/hosts.allow`{{execute HOST2}}<br>
`echo "portmap:ALL" >> /etc/hosts.deny`{{execute HOST2}}<br>
`service nfs-kernel-server restart`{{execute HOST2}}<br>
Our NFS server is up'n'ready, lets test it:<br>
Prepare mount point on acs1:<br>
`mkdir /root/node1`{{execute HOST1}}<br>
`mount -t nfs -o proto=tcp,port=2049 [[HOST2_IP]]:/ /root/node1`{{execute HOST1}}<br>
then try if We can see files:<br>
`touch /root/nfs-share/test`{{execute HOST2}}<br>
`ls /root/node1/`{{execute HOST1}}<br>
