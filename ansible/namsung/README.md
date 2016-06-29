Ansible "namsung" configuration
==================================

This folder follows the [best practices from ansible](http://docs.ansible.com/ansible/playbooks_best_practices.html)

Note that by default ansible assumes:

- hostnames are consistent all over the network (DNS properly setup)
- hosts are accessibles via SSH, using SSH keys. 

If some assumption here appears to be wrong, refer to [ansible docs](http://docs.ansible.com/ansible/intro_inventory.html#list-of-behavioral-inventory-parameters)


Using the namsung site configuration
------------------------------------

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
