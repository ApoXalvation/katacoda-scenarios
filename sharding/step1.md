In this scenario, We are going to deplay a Sharded MongoDB with three replicas (two for shards, and one for config database), and two mongos.<br>
To add little more real live conditions, We are going to use two nodes with kubernetes cluster on it.<br>
 Next We will configure persistent volume claims using nfs-client-provisioner - at least You need to change one line to adapt this statefulSet template to cloud enverinoment.<br>
Also We will try to achive some minimal pod affinity on our two nodes.

## Task
For this scenario We use scipt which deplay for us k8s cluster, this save some time:<br>
`/opt/launch-kubeadm.sh` - this command is running on HOST1 now, please wait with next steps until it ends executing <br>
_If You are wondering what does this script do, please take a look <a href src="https://katacoda.com/courses/kubernetes/getting-started-with-kubeadm">this</a> tutorial._<br><br>
You can now join second host to cluster by running the following command as root:
`kubeadm join [[HOST1_IP]]:6443 --token 96771a.f608976060d16396 --discovery-token-unsafe-skip-ca-verification`{{execute HOST2}}<br><br>
After about one minute all nodes should be ready, You can check status of them using this command:<br>
`kubectl get nodes`{{execute HOST1}}
Both nodes should has status _Ready_
