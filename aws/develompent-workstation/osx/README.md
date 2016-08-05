# Development Enviroment for AWS usung Vagrant

## 1. VirtualBox

### 1.1 Install Virtualbox
[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

## 2.  [Vagrant](https://www.vagrantup.com/)

### 2.1 Instal Vagrant 
[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)


### 2.2 Instanl Vagrant ENV Plugin
[https://github.com/gosuri/vagrant-env](https://github.com/gosuri/vagrant-env)

```bash
$ vagrant plugin install vagrant-env
```

### 2.3 Create [ubuntu/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64) vagrant box

#### Create a source directory on your home directory

```bash
mkdir ~/source
```
___
#### Configure [ubuntu/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64) vagrant box
```bash
$ mkdir -p ~/vagrant/ubuntu_14_04_LTS
$ cd ~/vagrant/ubuntu_14_04_LTS
$ vagrant init ubuntu/trusty64
$ atom Vagrantfile
```
___
#### Change the of `Vagrantfile` with the content of the linked file below:

[https://github.com/acarrasquillo/cloud/blob/master/aws/develompent-workstation/osx/Vagrantfile](https://github.com/acarrasquillo/cloud/blob/master/aws/develompent-workstation/osx/Vagrantfile)
___
#### Add some variables to `.env` file for the box setup
```bash
$ echo HOME=$HOME > .env
$ echo AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXDKQ >> .env
$ echo AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXACg/+Kz >> .env
```
___
#### Bring up the box with virtualbox
```bash
$ vagrant up --provider virtualbox
```
___
#### This may take some time go get some cereal
```bash
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'ubuntu/trusty64' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'ubuntu/trusty64'
    default: URL: https://atlas.hashicorp.com/ubuntu/trusty64
==> default: Adding box 'ubuntu/trusty64' (v20160801.0.0) for provider: virtualbox
    default: Downloading: https://atlas.hashicorp.com/ubuntu/boxes/trusty64/versions/20160801.0.0/providers/virtualbox.box
    default: Progress: 15% (Rate: 662k/s, Estimated time remaining: 0:09:54)
.
.
.
```
___
#### SSH into the box and test the internet connection
```
$ vagrant ssh
vagrant@vagrant-ubuntu-trusty-64:~$ ping google.com
PING google.com (216.58.194.206) 56(84) bytes of data.
64 bytes from sfo03s01-in-f14.1e100.net (216.58.194.206): icmp_seq=1 ttl=63 time=16.2 ms
64 bytes from sfo03s01-in-f14.1e100.net (216.58.194.206): icmp_seq=2 ttl=63 time=19.0 ms
64 bytes from sfo03s01-in-f14.1e100.net (216.58.194.206): icmp_seq=3 ttl=63 time=23.6 ms
^C
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 16.206/19.649/23.694/3.086 ms
```
___
#### WUJU you're set to use the box for development.


