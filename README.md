Goals next session:
* Setup Traefik
* Get dashboard working (both kubernetes and traefik)
* Get domain
* Route domain with Traefik
* Set up Let's encrypt with traefik
* Deploy a website

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
5. Set up context: `kubectl config set-context <context-navn> --user=<username> --cluser=<cluster-name>`
6. Use context: `kubectl config use-context <context-name>`


## Setup dashboard
* [Guide](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

## Install Traefik


## Pod priority
```
apiVersion: scheduling.k8s.io/v1beta1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000
globalDefault: false
description: "This priority class should be used for XYZ service pods only."
```

## Drain pod (maintenance)
1. Run `kubectl drain <node>` to start shutting down pods and reschedule on other nodes
2. Node is now unchedulable (see `kubectl get nodes` to confirm)
3. Do maintenance with node
4. Run `kubectl uncondon <node>`


### Resources
* [Traefik docs](https://docs.traefik.io/user-guide/kubernetes/)
* [ACME cert](https://blog.n1analytics.com/free-automated-tls-certificates-on-k8s/#install-cert-manager)

## References
* [How to install Kubernetes on Ubuntu 18.04 Bionic Beaver Linux](https://linuxconfig.org/how-to-install-kubernetes-on-ubuntu-18-04-bionic-beaver-linux)
* [Accessing Clusters (kubernetes.io)](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/)
* [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
[Creating a Custom Cluster from Scratch (kubernentes.io)](https://kubernetes.io/docs/setup/scratch/#try-examples)

* [Ansible tutorial ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-1-11-cluster-using-kubeadm-on-ubuntu-18-04)
