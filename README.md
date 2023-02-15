# kubernetes simple steps setup
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
sudo apt-get install -y apt-transport-https
```

### step 3

Next is to install Docker with the following command

```
sudo apt install docker.io
```

Now you can start and enable docker with the following command
```php
sudo systemctl start docker
sudo systemctl enable docker
```
check that it’s running with this command:
```
 sudo systemctl status docker
 ```
you should get an output like this:

![An image](markdownsheet.jpg)

### step 4

After installing Docker with the above step,
we will be using the curl commands for url installation syntax

```
sudo apt-get install curl
```

### step 5
Next is to download the keys to kubernetes urls using the following command.
```
sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg| sudo apt-key add
```

### step 6 

let's run the command below to add repository in certain location and change permission of the file
```
sudo chmod 777 /etc/apt/sources.list.d/
```

> do   

```
nano /etc/apt/sources.list.d/kubernetes.list 
```
and enter the following content:
```
deb https://apt.kubernetes.io/ kubernetes-xenial main
```

<span>Confirm to see that the above url is saved in that location by doing</span>
```
cat /etc/apt/sources.list.d/kubernetes.list
```

> Note: you should get the following output 👇🏾   
  'deb https://apt.kubernetes.io/ kubernetes-xenial main' 

Now let's check for any update available with 

```
sudo apt-get update
```

### step 7
Let's install kubernetes components with

```
sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```

> Note that this is gonna take few minutes.
> Next we have to initialize the master node using the swapoff command to disable swaping on other devices with the following  

```
sudo swapoff -a
```

### step 8

Next is to Initialize the master node using the following command:
```
sudo kubeadm init
```

To start using the cluster, we need to run the following 

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


