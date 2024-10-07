# k3s-setup
Setup and configuration of the k3s Kubernetes cluster

## Setting up the k3s cluster on Ubuntu/Debian
It is recommended to disable ufw (Uncomplicated firewall) using:
``` 
ufw disable
```
To install the k3s cluster use the command:
```
curl -sfL https://get.k3s.io | sh -
```
This installs a single-node server, which is a fully-functional Kubernetes cluster, including all the datastore, control-plane, kubelet, and container runtime components necessary to host workload pods. <br/><br/>
Tools like kubectl, crictl, ctr, k3s-killall.sh, and k3s-uninstall.sh are automatically installed.
### Adding additional worker nodes
To add additional worker nodes on another machine, you first need to get the token from the server node:
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
Use this token, and the ip adress of the server node to add the new worker node.
On the new machine, use this command, together with the token and the ip of the server node:
```
curl -sfL https://get.k3s.io | K3S_URL=https://yourServerIP:6443 K3S_TOKEN=NodeToken sh -
```
Where you have to replace `yourServerIP` and `Nodetoken`, with the ip of the server where the master and control-plane is running and the token from the previous command
## Utilizing Horizontal Pod Autoscaling
To be able to use the autoscaling, the [metric server](https://github.com/kubernetes-sigs/metrics-server#readme) must be installed. This can be done with the following command:
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## Network rules
Additionally, network rules need to be configured for server and worker nodes
for more information on ingress/engress rules refer to [k3s docs](https://docs.k3s.io/installation/requirements?os=debian#networking)

