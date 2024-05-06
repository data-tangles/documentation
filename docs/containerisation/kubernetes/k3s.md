# k3s Install

k3s is a "batteries-included" distribution of Kubernetes packaged into a single binary. This makes it really lightweight for low power devices such as Raspberry Pi's
and also makes it easy for getting familiar with Kubernetes as a whole.

For in depth documentation, visit the [k3s homepage](https://docs.k3s.io/).

k3s can be installed by running a single command on your Linux distro of choice. I generally add some slight modifications to the command in order to prep for creating a cluster.

## Initialise cluster

On the first/master node run the following and replace `<example token>` with a token of your choosing

``` bash
curl -sfL https://get.k3s.io | K3S_TOKEN=<example token> sh -s - server --cluster-init
```
The above will fetch the install binary and bootstrap a cluster using the provided token

## Add additional nodes

For any additional nodes you want to add to the cluster you can run the following replacing `<example token>` with the one specified in the bootstrap command and the `<master node fqdn/ip>` with the master node fqdn/ip:

``` bash
curl -sfL https://get.k3s.io | K3S_TOKEN=<example token> sh -s - server --server https://<master node fqdn/ip>:6443
```

## Verify node status

This should successfully join the node to the cluster which you can confirm by running the following on one of the nodes:

``` bash
kubectl get nodes
```

## Copy kubeconfig file

While you are logged into one of the nodes it would be a good idea to export the kubeconfig file so that you can connect using your local machine. You can get the kubeconfig file details by running the following on one of the nodes:

``` bash
cat /etc/rancher/k3s/k3s.yaml
```

Copy this output into a new kubeconfig file in your user profile path.

``` title="Windows"
C:\Users\username\.kube\config
```

``` title="Linux"
$HOME/.kube/config
```

You should now be able to connect to the cluster from your local machine
