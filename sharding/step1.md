Kubeadm has been installed on the nodes. Packages are available for Ubuntu 16.04+, CentOS 7 or HypriotOS v1.0.1+.<br>
<br>
The first stage of initialising the cluster is to launch the master node. The master is responsible for running the control plane components, etcd and the API server. Clients will communicate to the API to schedule workloads and manage the state of the cluster.

## Task

The command below will initialise the cluster with a known token to simplify the following steps.

`kubeadm init --token=102952.1a7dd4cc8d1f4cc5 --kubernetes-version $(kubeadm version -o short)`{{execute HOST1}}

In production, it's recommend to exclude the token causing kubeadm to generate one on your behalf.<br>
<br>
To manage the Kubernetes cluster, the client configuration and certificates are required. This configuration is created when kubeadm initialises the cluster. The command copies the configuration to the users home directory and sets the environment variable for use with the CLI.

`sudo cp /etc/kubernetes/admin.conf $HOME/
sudo chown $(id -u):$(id -g) $HOME/admin.conf
export KUBECONFIG=$HOME/admin.conf`{{execute HOST1}}
