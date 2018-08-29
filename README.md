# Kubernetes

## Setup Ubuntu 18.04

### Set up new user
1. `adduser <username>`
2. Copy over `.ssh` folder & chown to username
3. Add to sudoers group: `usermod -aG sudo <username>`

### Set up master
1. Set up local kubernetes user
  * Disable login without SSH key
2. Install docker: `apt install docker.io`
3. Enable docker: `systemctl enable docker`
4. Setup kubernetes **apt** repository
  * `curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add`
  * `sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"`
5. Install **kubeadm**: `sudo apt install kubeadm`
6. Turn of swapping (all nodes): `sudo swapoff -a`
7. Initiate kubernetes-master: `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`
8. Take note of `kubeadm join` command eg.:
9. Make `$HOME/.kube` folder
10. Copy default configuration from `/etc/kubernetes/admin.conf` to `$HOME/.kube/config`
11. Make kubernetes user owner with `chown`
12. Install **flannel** overlay network: `kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`
13. Check to see if everything is running: `kubectl get pods --all-namespaces`

### Set up node
1. Steps 1..6 from set up master
2. `kubeadm join`

### Connecting to cluster
1.

## References
[How to install Kubernetes on Ubuntu 18.04 Bionic Beaver Linux](https://linuxconfig.org/how-to-install-kubernetes-on-ubuntu-18-04-bionic-beaver-linux)
