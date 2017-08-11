# ansible-tower

Development environment for Ansible using Vagrant and Virtualbox.
Include VM definitions for Ansible Tower and clients running CentOS 6 & 7 and Ubuntu 14.

```
 ____________________________________
<  The only good bug is a dead bug.  >
 ------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)        )\/\
                ||----w |
                ||     ||
```
## Requirements

This development environment requires the following on the host machine (i.e. your MacBook or PC):

  - Virtualbox (VMware Fusion or VMware Workstation are supported with plugin)
  - Vagrant
  - Vagrant Plugins
  - Git
  - Optional:  Create VM definitions in *Vagrantfile*.

#### Clone repo
Clone this repository to local host and change to directory.

`$ git clone https://github.com/rcompos/ansible-tower`

#### Install Vagrant

Install Vagrant by downloading from https://www.vagrantup.com/downloads.html

#### Install Vagrant Plugins

Install Vagrant plugins using native plugin manager.  
*vagrant-hostmanager* manages VM operating system hosts file (*/etc/hosts*).
VMware Fusion and Desktop are supported with a paid plugin.

`$ vagrant plugin install vagrant-hostmanager`

#### Create VM Definitions (optional)

Virtual machine definitions are stored in the file *Vagrantfile*.  Customize as needed.
A large number of VM definitions which adds some notable lag to Vagrant start-up times.
Start-up lag can be remedied by pruning unwanted definitions.

## Setup
#### Spin up new VM's and configure

Change to *ansible-tower* directory and run the following as your user (root not required).

Get status:

`$ vagrant status`

Start set of VM's:

`$ vagrant up tower dev0`

Update hosts files:

`$ vagrant hostmanager`

Re-run provision scripts to update Ansible hosts:
*All ip addresses are listed.*

`$ vagrant provision tower`

Bounce servers to disable SELinux (unfortunate for now):

`$ vagrant reload dev0`

Login to Ansible Tower server and change to root. 
*The login message provides the url, username and password for Ansible Tower web service.*

`$ vagrant ssh tower`

On server *tower* change to root and run ansible ping module to connect to all nodes.
*Execute as root on server 'tower'.*  Type yes and press return three times:

```
tower$ sudo su -
tower$ ansible all -m ping   #  * type yes then enter twice *
```

## Develop

The ansible repo available from tower server at */vagrant/config*.
This is a shared directory to directory where Vagrantfile resides on host.

#### LAMP simple on CentOS 7 
Vagrant up servers `webserver0` and `dbserver0`.  *Execute as root on server 'tower'*:

See readme https://github.com/rcompos/ansible-tower/tree/master/demo/lamp_simple_rhel7-demo

```
tower$  cd /vagrant/demo/lamp_simple_rhel7-demo
tower$  ansible-playbook -i hosts site.yml
URL:  http://<webserver0_ip>/index.php
```

#### Tomcat on CentOS 7
Vagrant up server `tomcat0`.  *Execute as root on server 'tower'*:

See readme https://github.com/rcompos/ansible-tower/tree/master/demo/tomcat-standalone-demo

```
tower$  cd /vagrant/demo/tomcat-standalone-demo
tower$  ansible-playbook -i hosts site.yml
URL:  http://<tomcat0_ip>:8080
```

#### MongoDB on CentOS 6 cluster
Vagrant up servers `mongo1`, `mongo2`, `mongo3` and `mongo4`.  *Execute as root on server 'tower'*:

See readme https://github.com/rcompos/ansible-tower/tree/master/demo/mongodb-demo

```
tower$  cd /vagrant/demo/mongodb-demo
tower$  ansible-playbook -i hosts site.yml
URL:  http://<mongo1_up>:????
```

#### Wordpress Nginx on CentOS 7
Vagrant up `wordpress0`.  *Execute as root on server 'tower'*:

See readme https://github.com/rcompos/ansible-tower/tree/master/demo/wordpress-nginx_rhel7-demo

```
tower$  cd /vagrant/demo/wordpress-nginx_rhel7-demo
tower$  ansible-playbook -i hosts site.yml
URL:  http://<wordpress0_ip>
```

## Destruction
#### Stop VM's

Stop VM's:

`$ vagrant halt`

#### Destroy VM's to abandon or rebuild:

`$ vagrant destroy -f tower dbserver webserver`

## Helpful Commands

Start all VM's:

`$ vagrant up`

Start single VM:

`$ vagrant up tower`

Start multiple VM's:

`$ vagrant up dbserver webserver`

Stop single VM:

`$ vagrant halt dbserver`

Stop multiple VM's:

`$ vagrant halt dbserver webserver`

Get global status:

`$ vagrant global-status`
