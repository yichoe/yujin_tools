Yujin Ansible Konfiguration
===========================

This folder follows the [best practices from ansible](http://docs.ansible.com/ansible/playbooks_best_practices.html)

Note that by default ansible assumes:

- hostnames are consistent all over the network (DNS properly setup)
- hosts are accessible via SSH. For convenience you should have a proper set of SSH keys properly setup on all machines for the relevant users.
To do basic checks on your environment, you can refer to [ansible getting started docs](http://docs.ansible.com/ansible/intro_getting_started.html)

If some assumption here appears to be wrong for your infrastructure, refer to [ansible inventory docs](http://docs.ansible.com/ansible/intro_inventory.html#list-of-behavioral-inventory-parameters)

Namsung GoCartYujin deployment
------------------------------

To install ansible on Ubuntu Trusty, follow http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu

Since:

- there is no DNS setup, only hostnames defined in /etc/hosts on all hosts
- there is no user policy except "yujin is the known user that has sudo privileges"

It makes sense to centralise all deployment to be executed from the concert server.
So on the concert server install ansible : 

    sudo apt-get install ansible

Make sure your hostnames are setup properly and setup your network inventory in /etc/ansible/hosts.
Currently we assume we launch ansible from a concert machine (until we can configure concert hostname in software):

    gocart201
    gocart202
    gocart203
    gocart204

    [local]
    127.0.0.1
    
    [concert]
    127.0.0.1 groot_stream=devel
    
    [gocarts]
    gocart201
    gocart202
    gocart203
    gocart204
    
BEWARE : You should NOT use this configuration on your own machine.
Because of the lack of DNS, the necessary local setup to execute remotely is quite different.

Make sure the yujin user can access all robots, and from there you should be able to deploy:

 - the concert software locally by using the playbooks in this repository
 - the robot software remotely by using the playbooks in this repository

Getting ready
-------------

 * First, connect to the concert server, and check namsung site is properly setup for ansible:

`ansible gocarts -m ping`

`ansible gocarts -a "/bin/echo hello"`

 * Checkout this repository

`git clone --recursive https://github.com/yujinrobot/yujin_tools.git`
 
 * Change to the playbooks directory, so you can access the files easily, ie.:

`cd yujin_tools/ansible/namsung`

 * Install ansible roles requirements from galaxy and github:

`ansible-galaxy install -r requirements.yml -p roles`

 * To list all tasks for a playbook:

`ansible-playbook concert.yml --list-tasks --ask-vault-pass`

Running Playbooks
-----------------

* Run `clocksync.yml` to ensure clock synchronization software is setup and running on both robots and concert

    `ansible-playbook clocksync.yml --ask-vault-pass -K -b`
     or only on concert:
    `ansible-playbook clocksync.yml --ask-vault-pass -K -b --limit concert`

* Run `concert_dockers.yml` to ensure the concert dockers are setup and running on concert (local) server
    
    `ansible-playbook concert_dockers.yml --ask-vault-pass -K -b`

* Run `ros_concert.yml` to ensure the ros_concert software is setup and running on concert (local) server

    `ansible-playbook ros_concert.yml --ask-vault-pass -K -b`
    
* Run `syncthing_gopher.yml` to ensure syncthing is setup and running on both robots and concert

    `ansible-playbook syncthing_gopher.yml --ask-vault-pass -K -b`
     or only on a specific robot:
    `ansible-playbook syncthing_gopher.yml --ask-vault-pass -K -b --limit gocart203`
    
* Run `concert.yml` to ensure concert server is setup and running (master playbook). 
* TODO : `ros_gopher.yml` to ensure ros_gopher software is setup and running on robots
* TODO : `gocart.yml` to ensure gocart robot is setup and running (master playbook).

 
  
