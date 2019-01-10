In this scenario, We are going to deplay a Sharded MongoDB with three replicas (two for shards, and one for config database), and two mongos.<br>
To add little more real live conditions, We are going to use two nodes with kubernetes cluster on it.<br>
 Next We will configure persistent volume claims using nfs-client-provisioner - at least You need to change one line to adapt this statefulSet template to cloud enverinoment.<br>
Also We will try to achive some minimal pod affinity on our two nodes.

## Task
For this scenario We use scipt which deplay for us k8s cluster, this save some time:<br>
`/opt/launch-kubeadm.sh`{{execute HOST1}}<br>
__If You are wondering what does this script do, please take a look <a href src="https://katacoda.com/courses/kubernetes/getting-started-with-kubeadm">this</a> tutorial.__<br>
You can now join second host to cluster by running the following command as root:
`kubeadm join 172.17.0.123:6443 --token 96771a.f608976060d16396 --discovery-token-unsafe-skip-ca-verification`{{execute HOST2}}<br>
After about one minute all nodes should be ready, You can check status of them using this command:<br>
`kubectl get nodes`{{execute HOST1}}
