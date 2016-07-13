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

On the machine doing the deployment, install ansible : 

    sudo apt-get install ansible

Make sure your hostnames are setup properly in /etc/hosts.
Setup your network inventory in /etc/ansible/hosts like:

    # for direct access
    gocart201
    gocart202
    gocart203
    gocart204

    # for local access
    [local]
    127.0.0.1
    
    # mandatory to define concert machines
    [concert]
    192.168.3.3
    
    # mandatory to define gocart machines
    [gocarts]
    gocart201
    gocart202
    gocart203
    gocart204

[concert] and [gocarts] are two mandatory inventory groups that are used in the deployment scripts here.
They define if a machine is to be deployed like a concert server or like a gocart robot.

Make sure your local user can connect to the yujin user on the remote machine.
Make sure the yujin user has passwordless sudo access to root.
From there you should be able to deploy:

 - clock synchronization configuration using clocksync.yml
 - syncthing using syncthing_gopher.yml
 - the concert web portal via concert_portal.yml
 - the concert ROS software using groot_concert.yml
 - the balcony docker using balcony_docker.yml
 
TODO : Robot software

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

 * To list all hosts for a playbook run:

`ansible-playbook concert.yml --list-hosts --ask-vault-pass`

 * To limit a playbook run to a subset of hosts:

`ansible-playbook concert.yml --list-hosts --ask-vault-pass --limit gocart-server-devel`


Running Playbooks
-----------------

* Run `clocksync.yml` to ensure clock synchronization software is setup and running on both robots and concert

    `ansible-playbook clocksync.yml --ask-vault-pass -K`
     or only on concert:
    `ansible-playbook clocksync.yml --ask-vault-pass -K --limit concert`

* Run `syncthing_gopher.yml` to ensure syncthing is setup and running on both robots and concert

    `ansible-playbook syncthing_gopher.yml --ask-vault-pass -K`
     or only on a specific robot:
    `ansible-playbook syncthing_gopher.yml --ask-vault-pass -K --limit gocart203`

* Run `groot_concert.yml` to ensure the ros_concert software is setup and running on concert (local) server

    `ansible-playbook groot_concert.yml --ask-vault-pass -K`
     To make sure we re installing groot --stable ( this will override local config )
     `ansible-playbook groot_concert.yml --ask-vault-pass -K -e yujin_stream="stable"`

* Run `balcony_docker.yml` to ensure the balcony docker is setup and running on concert (local) server
    
    `ansible-playbook balcony_docker.yml --ask-vault-pass -K`
    To make sure we re installing the stable version ( this will override local config )
     `ansible-playbook balcony_docker.yml --ask-vault-pass -K -e yujin_stream="stable"`

* Run `concert_portal.yml` to ensure the concert_portal software is setup and running on concert (local) server

    `ansible-playbook concert_portal.yml --ask-vault-pass -K`
     To make sure we re installing groot --stable ( this will override local config )
     `ansible-playbook concert_portal.yml --ask-vault-pass -K -e yujin_stream="stable"`
    
* Run `concert.yml` to ensure full concert server is setup and running (master playbook).


 
  
