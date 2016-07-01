Ansible "namsung" configuration
==================================

This folder follows the [best practices from ansible](http://docs.ansible.com/ansible/playbooks_best_practices.html)

Note that by default ansible assumes:

- hostnames are consistent all over the network (DNS properly setup)
- hosts are accessible via SSH. For convenience you should have a proper set of SSH keys properly setup on all machines.
To do basic checks on your environment, you can refer to [ansible getting started docs](http://docs.ansible.com/ansible/intro_getting_started.html)

If some assumption here appears to be wrong for your infrastructure, refer to [ansible inventory docs](http://docs.ansible.com/ansible/intro_inventory.html#list-of-behavioral-inventory-parameters)


Using the namsung site configuration
------------------------------------

Check ansible documentation to understand how to use hte command line and all the arguments.
In any case, here is a quick cheatsheet : 

 * First you should probably check namsung site is properly setup for ansible:

`ansible namsung -i devel -m ping`

`ansible namsung -i devel -a "/bin/echo hello"`

 * Change to the current playbook directory, so you can access the files easily, ie.:

`cd ansible/namsung`

 * To install ansible roles requirements from galaxy and github:

`ansible-galaxy install -r requirements.yml -p roles`

 * To list all tasks for a playbook:

`ansible-playbook -i devel concert.yml --list-tasks`

 * To install ntp on concert server:

`ansible-playbook -i devel concert.yml --tags ntp`

 * To install syncthing-gopher on gocart203 (using keys from vault file and without ssh key setup on robot):
 
`ansible-playbook -v -i devel gocart.yml -K --tags syncthing-gopher --ask-vault-pass --ask-pass` 
 
  
