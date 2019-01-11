
We will need nfs server for persistent volume claims for our shards, so lets configure one on host2

## Task
Update and install nfs-kernel-server on host2:<br>
`apt-get install nfs-kernel-server`{{execute HOST2}}<br>
and nfs client on host1:<br>
`apt-get install nfs-common`{{execute HOST1}}<br>
then prepare shared sirectory (host2):<br>
`mkdir /root/nfs-share`{{execute HOST2}}<br>
define what we want to share and who may connect(host2):<br>
`echo "/root/nfs-share [[HOST1_IP]]/16(rw,no_root_squash)" >> /etc/exports &&
service nfs-kernel-server restart`{{execute HOST2}}<br>
## Tests
Our NFS server is up'n'ready, lets test it:<br>
Prepare mount point on host1:<br>
`mkdir /root/host2 &&
mount -t nfs -o proto=tcp,port=2049 [[HOST2_IP]]:/root/nfs-share /root/host2`{{execute HOST1}}<br>
then You may check if files are accessible on both hosts:<br>
`touch /root/nfs-share/test`{{execute HOST2}}<br>
`ls /root/host2/`{{execute HOST1}}<br>
