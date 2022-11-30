# kubernetes
It is an open source platform used to deploy, manage and maintain a group of containers. it is an Ubuntu based platform.
It is used most commonly together with Docker for better control of containerized applications.

# Prerequisites
- I assume you know how to deploy a virtual machine 🖥
- basic knowledge of the linux file system | ownership | permissions 🐧
- basic knowledge of git 🚦
- stable internet connection (very important)☁️
- How to use linux editor `vim` or at least `nano` 📝
- a cup of coffee ☕️

<span> If this mini-article doesn't work for you there are video guides below that can help deploy this project here [Helpful Videos](#Helpful-Videos)</span>

### SETUP

### step 1

We will be installing add updating the apt repository before installing any package

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

### step 4. 

To add the repository in certain location,
we shall be changing permission of the file with
```
sudo chmod 777 /etc/apt/sources.list.d/
```

> Note: we need to cd into /etc/apt/sources.list.d/
> then do nano kubernetes.list to enter the following url, and save the file in the location   

```
deb https://apt.kubernetes.io/ kubernetes-xenial main
```

<span>Confirm to see that the above url is saved in that location by doing</span>
```
cat /etc/apt/sources.list.d/kubernetes.list
```

> Note: you should get the following output 👇🏾 
> deb https://apt.kubernetes.io/ kubernetes-xenial main 

Now let's check for any update available with 

```
sudo apt-get update
```

### step 5
Let's install kubernetes components with

```
sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```

> Note that this is gonna take few minutes.
> Next we have to initialize the master node using the swapoff command to disable swaping on other devices with the following  

```
sudo swapoff -a
```

### step 6

Next is to start the initiallization
```
sudo kubeadm init
```

To start using the cluster, we need to run the following 

```
mkdir -p $HOME/.kube
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```


Rename the cloned git repo to whatever you wish to call your project, for my use case I will name it `laravel`
```php
mv laravel-realworld-example-app laravel
```

Switch to your projects directory
```php
cd laravel 
```


Run the following command to edit the `web.php` file in the routes directory
```
nano /var/www/altschool/laravel/routes/web.php
```



### 7. Create and edit the `.env` file

`.env` files are not generally committed to source control for security reasons.

```php
cp .env.example .env
```

> This will create a copy of the `.env.example` file in your project and name the copy simply `.env`

Next, edit the `.env` file and define your database:
```
nano .env
```

`Note`: Configure your `.env` file just as it is in the output below, only make changes to the `DB_DATABASE` and `DB_PASSWORD` lines

