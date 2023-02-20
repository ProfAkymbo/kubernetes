# kubernetes setup using kubeadm
It is an open source platform used to deploy, manage and maintain a group of containers. it is an Ubuntu based platform.
It is used most commonly together with Docker for better control of containerized applications.

# Prerequisites
- I assume you know how to deploy a virtual machine 🖥 Ubuntu preferably.
- basic knowledge of the linux file system | ownership | permissions 🐧
- basic knowledge of git 🚦
- stable internet connection (very important)☁️
- How to use linux editor `vim` or at least `nano` 📝
- a cup of coffee ☕️

### SETUP

### step 1

We will be installing and updating the apt repository before installing any package

```
sudo apt-get update
```

### step 2

Now that our installer is up to date, next is to use following command to install packages using https

```php
 sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

### step 3

Next is to install Docker with the following commands 

```
 sudo mkdir -m 0755 -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Now let's set up the repo with the following command
```php
 echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Run this command to update the package:
 
```
 sudo apt-get update
 ```
Containerd is an important component for kubernetes to function, run
'''
sudo apt install containerd.io
'''
Open the /etc/containerd/conf.toml file, and comment out disabled_plugins

'''
nano /etc/containerd/conf.toml
'''
As it's in the output below:

![image](containerd-status.PNG)

### step 4

After installing Docker with the above step,
Let's restart containerd service

```
sudo systemctl restart containerd.service
```

### step 5
Next is to install kubernetes using the following commands.
'''
 sudo apt-get install -y apt-transport-https ca-certificates curl
'''

Download the Google Cloud public signing key:

```
sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg| sudo apt-key add
```

### step 6 

Let's add the Kubernetes APT repository with:
'''
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
'''

### step 7
Let's install kubernetes components with

```
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
sudo apt-mark hold kubelet kubeadm kubectl
```
The hold flag would prevent updates to the kubernetes components installed.


Next is to do 
```
nano /etc/sysctl.conf
```

> to uncomment the line below  

![image](ipv4-forwarding.PNG)

### step 8

Followed by this command to confirm
```
sysctl -p
```
Also run this command which works intelligently to add any dependent modules automatically.
```
sudo modprobe br_netfilter
```
All the above are run on all the 3 nodes ( one master and 2 workers ).

### step 9

Next is to Initialize the master node using the following command: NOTE- YOU NEED TO REPLACE YOUR MASTER NODE IP ADDRESS BELOW
```
 kubeadm init --pod-network-cidr 10.244.0.0/16 --apiserver-advertise-address=120.31.12.56
```

To start using the cluster, Exit root user, we need to run the following 

```
mkdir -p $HOME/.kube
```
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### step 9
we can now deploy pods using the following command:

```
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/ master/Documentation/kube-flannel.yml
```
```
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/ master/Documentation/k8s-manifests/kube-flannel-rbac.yml
```

### step 10
To see all pods deployed, use the following command:
```
sudo kubectl get pods –all-namespaces
```

# Note that i encountered the error below 
![image](kubeadm-init-error.PNG)
and i debugged it with [This](https://forum.linuxfoundation.org/discussion/862825/kubeadm-init-error-cri-v1-runtime-api-is-not-implemented)
