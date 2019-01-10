In this scenario, We are going to deploy a Sharded MongoDB with two shards.<br>
To add little more real-life conditions, We are going to use two nodes with kubernetes cluster on it.<br>
Next, We will configure persistent volume claims for each shard and config database using nfs-client-provisioner - in the end, You will need to change one line to adopt this statefulSet template to a cloud environment.<br>
Also, We will try to achieve some minimal pod affinity for our two nodes.<br>

## Task
For this scenario We are using a shell script which deploys for us k8s cluster, this saves some time:<br>
`/opt/launch-kubeadm.sh` - this command is now running on HOST1, please wait with next steps until it ends executing<br>
_If You are wondering what does this script do, please take a look at <a href src="https://katacoda.com/courses/kubernetes/getting-started-with-kubeadm">this</a> tutorial._<br><br>
You can now join the second host to cluster by running the following command as root:
`kubeadm join [[HOST1_IP]]:6443 --token 96771a.f608976060d16396 --discovery-token-unsafe-skip-ca-verification`{{execute HOST2}}<br><br>
After about a one minute both nodes should be _Ready_, You can check the statuses by this command on master node:<br>
`kubectl get nodes`{{execute HOST1}}
Both nodes should have status _Ready_
