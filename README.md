# kubernetes
It is an open source platform used to deploy, manage and maintain a group of containers. it is an Ubuntu based platform.
It is used most commonly together with Docker for better control of containerized applications.

![laravel-logo](/mini-project/img/laravel-page)

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

The code block that we want to alter in the file should look similar to what we have below

```php
<?php

/*Route::get('/', function () {
    return view('welcome');
});*/
```

When you are done editing the file it should now look like this 👇🏾

```php
<?php

Route::get('/', function () {
    return view('welcome');
});
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

```php
APP_NAME="your app name" (call it anything you wish)
APP_URL=your machine's IP addr eg. (192.168.10.22)
DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=enter the name of your database here
DB_USERNAME=root
DB_PASSWORD=enter your mysql root password here
```
After updating your .env file, save the changes and exit


### 8. Install Composer

Composer is a dependency manager for PHP used for managing dependencies and libraries required for PHP applications. To install `composer` run the following command: 

```php
curl -sS https://getcomposer.org/installer | php
```

You should get the following output
```php
All settings correct for using Composer
Downloading...
Composer (version 2.4.3) successfully installed to: /root/composer.phar
Use it: php composer.phar 
```

Next, move the downloaded binary to the system path and make it executable by everyone
```php
mv composer.phar /usr/local/bin/composer
chmod +x /usr/local/bin/composer
```

Next, run the following command and enter `yes` to any prompt that appears 
```php
composer
```

You should see the following output 

![laravel-logo](/mini-project/img/composer.png)


### 9. Install Composer Dependencies

<span>Whenever you clone a new Laravel project you must now install all of the project dependencies. This is what actually installs Laravel itself, among other necessary packages to get started. When we run composer, it checks the `composer.json` file which is submitted to the github repo and lists all of the composer (PHP) packages that your repo requires. Because these packages are constantly changing, the source code is generally not submitted to github, but instead we let composer handle these updates.</span>

So to install all this source code run composer with the following command and enter `yes` to any prompt that appears

```php
composer install
```

Generate the artisan key with the following command 
> make sure you are in the `/var/www/altschool/laravel` directory before executing any command that starts with `php artisan`
```php
php artisan key:generate
```

Also run the following php artisan commands
```
php artisan config:cache
php artisan migrate:fresh
php artisan migrate --seed
```


### 10. Configure Apache to Host Laravel 8

Next, you'll need to create an Apache virtual host configuration file to host your Laravel application.
```php
nano /e
