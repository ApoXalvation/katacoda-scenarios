


## Task

`apt-get update && apt-get install nfs-kernel-server`{{execute HOST2}}
`apt-get update && apt-get install nfs-common`{{execute HOST1}}
`mkdir /root/nfs-share`{{execute HOST2}}
`echo "/root/nfs-share [[HOST1_IP]]/32(rw,fsid=0,insecure,no_subtree_check,async)" >> /etc/exports`{{execute HOST2}}
`echo "portmap: [[HOST1_IP]]/32" >> /etc/hosts.allow`{{execute HOST2}}
`echo "portmap:ALL" >> /etc/hosts.deny`{{execute HOST2}}
`service nfs-kernel-server restart`{{execute HOST2}}
`mount -t nfs -o proto=tcp,port=2049 [[HOST2_IP]]:/ /root/node1`{{execute HOST1}}
`touch /root/nfs-share/test`{{execute HOST2}}
`ls /root/node1/`{{execute HOST1}}
