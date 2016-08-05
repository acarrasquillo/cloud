# Development Enviroment for AWS usung Vagrant

## 1. VirtualBox

### 1.1 Install Virtualbox
[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

## 2.  [Vagrant](https://www.vagrantup.com/)

### 2.1 Install Vagrant 
[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)


### 2.2 Instanll Vagrant ENV Plugin
[https://github.com/gosuri/vagrant-env](https://github.com/gosuri/vagrant-env)

```bash
$ vagrant plugin install vagrant-env
```

### 2.3 Create [ubuntu/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64) vagrant box

#### Create a source directory on your home directory

```bash
mkdir ~/source
```

#### Configure [ubuntu/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64) vagrant box
```bash
$ mkdir -p ~/vagrant/ubuntu_14_04_LTS
$ cd ~/vagrant/ubuntu_14_04_LTS
$ vagrant init ubuntu/trusty64
$ atom Vagrantfile
```

#### Add some variables to `.env` file for the box setup
```bash
$ echo HOME=$HOME > .env
$ echo AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXDKQ >> .env
$ echo AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXACg/+Kz >> .env
```

#### Change the of `Vagrantfile` with the content below:
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.env.enable # enable the plugin
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "#{ENV['HOME']}/source/", "/home/vagrant/source/"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  # Display the VirtualBox GUI when booting the machine
    # vb.gui = true
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
   apt-get update
   apt-get install -y awscli
   echo "export AWS_ACCESS_KEY_ID=#{ENV['AWS_ACCESS_KEY_ID']}" | sudo tee /etc/profile.d/aws-cred-env.sh
   echo "export AWS_SECRET_ACCESS_KEY=#{ENV['AWS_SECRET_ACCESS_KEY']}" | sudo tee -a /etc/profile.d/aws-cred-env.sh
  SHELL
end
```

#### Bring up the box with virtualbox
```bash
$ vagrant up --provider virtualbox
```
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

#### WUJU you're set to use the box for development.


