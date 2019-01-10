Once the Master has initialised, additional nodes can join the cluster as long as they have the correct token. The tokens can be managed via `kubeadm token`, for example<br>
`kubeadm token list`{{execute HOST1}}

## Task

On the second node, run the command to join the cluster providing the IP address of the Master node. <br>
`kubeadm join --discovery-token-unsafe-skip-ca-verification --token=102952.1a7dd4cc8d1f4cc5 [[HOST_IP]]:6443`{{execute HOST2}} <br>
This is the same command provided after the Master has been initialised.<br>
<br>
The `--discovery-token-unsafe-skip-ca-verification` tag is used to bypass the Discovery Token verification. As this token is generated dynamically, we couldn't include it within the steps. When in production, use the token provided by `kubeadm init`.
