In this scenario, We are going to deplay a Sharded MongoDB with three replicas (two for shards, and one for config database), and two mongos.<br>
To add little more real live conditions, We are going to use two nodes with kubernetes cluster on it.<br>
 Next We will configure persistent volume claims using nfs-client-provisioner - at least You need to change one line to adapt this statefulSet template to cloud enverinoment.<br>
Also We will try to achive some minimal pod affinity on our two nodes.

## Task
For this scenario We use scipt which deplay for us k8s cluster, this save some time:<br>
`/opt/launch-kubeadm.sh`{{execute HOST1}}<br>
If You are wondering what does this script do, please take a look <a href src="https://katacoda.com/courses/kubernetes/getting-started-with-kubeadm">this</a> tutorial.

