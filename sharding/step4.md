The cluster has now been initialised. The Master node will manage the cluster, while our one worker node will run our container workloads.

## Task

The Kubernetes CLI, known as kubectl, can now use the configuration to access the cluster. For example, the command below will return the two nodes in our cluster.<br>
`kubectl get nodes`{{execute HOST1}}<br>
<br>
**At this point, the Nodes will not be ready. **<br>
<br>
This is because the Container Network Interface has not been deployed. This will be fixed within the next step.
