## Setup ServerPilot with Vagrant

This repository provides a sample Vagrantfile to create a Ubuntu 14.04 64-bit virtual machine. After following the installation instructions, you'll have a server managed by ServerPilot and accessible via your browser at `local.io`.
Inspired from [manjoker/Vagrantfile](https://github.com/manjoker/Vagrantfile)

https://serverpilot.io

## Information
You will be able to setup as many sub-domain as you want. You will have phpMyAdmin running on your server and you will also be able to share your virtual machine with https://vagrantcloud.com

## Getting Started
1. Install dependencies
  * [Virtualbox](https://www.virtualbox.org/) 4.0 or greater.
  * [Vagrant](http://downloads.vagrantup.com/) 1.3.1 or greater.
2. Copy the [Vagrantfile](https://github.com/Xety/ServerPilot/blob/master/Vagrantfile)
3. Execute the following commands to setup the virtual machine :
```bash
$ vagrant up
$ vagrant ssh
```

## Connecting to ServerPilot
  * [Log in](https://manage.serverpilot.io/#login) to ServerPilot
  * Go to the **Servers** page and click **+ Connect Server**
  * Click `Install ServerPilot manually.` at the bottom
    * `Name` => `local.io`
    * `SFTP Password` => `Choose a password`
  * Copy the **whole** command (and let the page open in your browser):
![fdf4c72543af0a52e47b3474b150e93b](https://cloud.githubusercontent.com/assets/8210023/11672449/e0f1da26-9e10-11e5-84f0-2b3229e75c48.png)

  * Paste the command in the VM terminal wait until to get this message :
![c5d6522793686dbc0ab42d75e5be7df1](https://cloud.githubusercontent.com/assets/8210023/11672125/f12bf9ce-9e0d-11e5-8a8c-8f18ee4c799e.png)

  * Watch **ServerPilot** install
![55a2c4882b4e624ef61a49d5de59c32a](https://cloud.githubusercontent.com/assets/8210023/11672081/8cdad1d4-9e0d-11e5-8640-e1a7d3b744a7.png)
  * **Note** : The part `Testing server configuration` **may take several minutes** (more than 20 minutes for me)

## Create your Applications in ServerPilot
  * Go to the **Apps** page and click **+ Create App**
    * `Name` => `website`
    * `Domain` => `local.io`
    * `Server` => `local.io`

## Edit the `hosts` file
  * Edit the following file as `administrator` / `root`
    * Ubuntu/Mac OS X : `/etc/hosts`
    * Windows : `C:\WINDOWS\system32\drivers\etc\hosts`
  * Add the follow code in this file :
    * `192.168.56.101 local.io`
    * If you plan to add sub-domains, you can add them now in the `hosts` file :
```
192.168.56.101 local.io
192.168.56.101 subdomain1.local.io
192.168.56.101 subdomain2.local.io
```

## Edit the `Vagranfile` file
  * Now, you must edit the `Vagranfile` and **uncomment** this line :
    ```
    config.vm.synced_folder '/var/www/Sites/website', '/srv/users/serverpilot/apps/website/public', owner: "serverpilot", group: "serverpilot"
    ```
  * Adapt it to your system. The first part, is the path on **your** system. The second part, is the path on the VM system.
    * **Note** : Don't remove the `owner` and `group` keys. I have got some issues with the permissions, and this configuration seems to resolve all permissions bugs. If you want to add new `synced_folder`, you must add the `owner` and `group` keys with the `serverpilot` value.
  * Execute the following command to reload the VM with the modified Vagranfile :
```bash
vagrant@local:~$ exit
$ vagrant reload
```

## Enjoy
  * Go to [local.io](http://local.io) in a new tab and you should see the ServerPilot splash page **or** your application.
![6f7a24856a34bdc310657e9d92d6655c](https://cloud.githubusercontent.com/assets/8210023/11673405/d519a794-9e18-11e5-940b-a7104484b9aa.png)


# More Configuration
The following configuration are not required, but can be usefull if you plan to install phpMyAdmin or setup a sub-domain.

### Install `phpMyAdmin`
  * Create a new application on ServerPilot :
    * `Name` => `pma`
    * `Domain` => `pma.local.io`
    * `Server` => `local.io`
  * Download the latest version of `phpMyAdmin` :
    * http://www.phpmyadmin.net/home_page/downloads.php
  * Open the archive that you downloaded
  * Place the archive where you want on your system. For the tutorial, i have placed it in `/var/www/Sites/pma`.
  * Rename `config.sample.inc.php` to `config.inc.php`.
  * Now, open `config.inc.php` and set a random string of characters for the value of ``$cfg['blowfish_secret']`` near the top of the file. Exemple :
```
$cfg['blowfish_secret'] = 'asdof7q230984(*^3q4'
```

  * Add `192.168.56.101 pma.local.io` to your `hosts` file.
  * Uncomment and adapt (regarding to the path) the following line in the Vagranfile :
```
config.vm.synced_folder '/var/www/Sites/pma', '/srv/users/serverpilot/apps/pma/public', owner: "serverpilot", group: "serverpilot"
```
  * Execute the following command to reload the VM with the modified Vagranfile :
```bash
$ vagrant reload
```
  * Create a database on ServerPilot :
![280552ce196fca4efcc75bb31060c1c2](https://cloud.githubusercontent.com/assets/8210023/11673299/f894f1fc-9e17-11e5-82eb-a7fe91dabd72.png)

  * Go to [pma.local.io](http://pma.local.io) and login with the same `username` and `password` that you used to create the database.

### Create a `sub-domain`
  * If you followed the phpMyAdmin installation, you already know how to setup a sub-domain. For the tutorial, I will setup a sub-domain named `xety.local.io`
  * Create a new application on ServerPilot :
    * `Name` => `xety`
    * `Domain` => `xety.local.io`
    * `Server` => `local.io`
  * Add `192.168.56.101 xety.local.io` to your `hosts` file.
  * Uncomment and adapt (regarding to the path) the following line in the Vagranfile :
  ```
  config.vm.synced_folder '/var/www/Sites/xety', '/srv/users/serverpilot/apps/xety/public', owner: "serverpilot", group: "serverpilot"
  ```
  * Execute the following command to reload the VM with the modified Vagranfile :
  ```bash
  $ vagrant reload
  ```
  * * Go to [xety.local.io](http://xety.local.io) and enjoy ! Yes, it's very simple to do.

### Connect to the `SFTP`
Yes, you can connect to the server using the SFTP method !
  * Use your prefered software. I will use `FileZilla`.
  * Login in FileZilla with the fallowing credentials :
    * `Host` => `192.168.56.101`
    * `User` => `serverpilot`
    * `Password` => `The SFTP password that you provided at the beginning of this tutorial`
    * `Port` => `22`
  * You can also login with the `vagrant` user :
    * `Host` => `192.168.56.101`
    * `User` => `vagrant`
    * `Password` => `vagrant`
    * `Port` => `22`
  * But i recommend to you to use the `serverpilot` user and not the `vagrant`, else you will probably have some permissions issues (i.e : https://serverpilot.io/community/articles/how-to-fix-file-permissions.html)

### Share your VM
  * It's easy with 1 command, more detail there : https://vagrantcloud.com/help/vagrant/shares/create
  * Possible issue :
    * `Why the share doesn't show the good application ?` => Because ServerPilot define the default application by alphabeticallyorder. More information : https://serverpilot.io/community/articles/how-to-set-the-default-app.html
