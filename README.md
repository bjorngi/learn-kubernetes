# Kubernetes

## Setup Ubuntu 18.04
### Set up new user
1. `adduser <username>`
2. Copy over `.ssh` folder & chown to username
3. Add to sudoers group: `usermod -aG sudo <username>`

### Set up machines
1. Set up local kubernetes user
  * Disable login without SSH key
2. Disable ssh for root user
2. Install docker: `apt install docker.io`
3. Enable docker: `systemctl enable docker`
4. Setup kubernetes **apt** repository
  * `curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add`
  * `sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"`
5. Install **kubeadm**: `sudo apt install kubeadm`
6. Turn of swapping (all nodes): `sudo swapoff -a`

### Set up master
1. Initiate kubernetes-master: `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`
2. Take note of `kubeadm join` command eg.:
3. Make `$HOME/.kube` folder
4. Copy default configuration from `/etc/kubernetes/admin.conf` to `$HOME/.kube/config`
5. Make kubernetes user owner with `chown`
6. Install **flannel** overlay network: `kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`
7. Check to see if everything is running: `kubectl get pods --all-namespaces`

### Set up node
1. `kubeadm join`

## Connecting to cluster
1. [Create token](https://github.com/kubernetes/dashboard/wiki/Creating-sample-user)
2. Install `kubectl` locally
3. Set cluster: `kubectl config set-cluster <cluster-name> --server=<server-addr>:<port(default:6443)> --insecure-skip-tls-verify=true`
4. Set user: `kubectl config set-credentials <username> --token=<your-token>`
5. Set up context: `kubectl config set-context <context-navn> --user=<username> --cluster=<cluster-name>`
6. Use context: `kubectl config use-context <context-name>`

## References
[How to install Kubernetes on Ubuntu 18.04 Bionic Beaver Linux](https://linuxconfig.org/how-to-install-kubernetes-on-ubuntu-18-04-bionic-beaver-linux)
[Accessing Clusters (kubernetes.io)](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/)
[kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
[Creating a Custom Cluster from Scratch (kubernentes.io)](https://kubernetes.io/docs/setup/scratch/#try-examples)
