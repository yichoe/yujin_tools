balcony-docker
==============

Ensure that we have yujinrobot/balcony docker container running.

Requirements
------------

 - Ubuntu Trusty
 - hostnames already setup ( via /etc/hosts or DNS or other )
 - clock synchronized ( via NTP or other )

Role Variables
--------------

 - balcony_dockerhub_user: The username on dockerhub used to retrieve balcony image
 - balcony_dockerhub_password: the password
 - balcony_dockerhub_email: the email address 

Dependencies
------------

 - marvinpinto.docker
 - tersmitten.supervisor
 
Example Playbook
----------------

    - hosts: 
       - gocart-server
      pre_tasks:
        - name: 'Ensure that we can connect to this host'
          ping:
      vars_prompt:
       - name: "docker_hub_login"
         prompt: "Docker Hub Username : "
         private: no
         tags: docker
       - name: "docker_hub_password"
         prompt: "Docker Hub password : "
         private: yes
         tags: docker
       - name: "docker_hub_email"
         prompt: "Docker Hub email : "
         private: no
         tags: docker
      roles:
       
       - role: balcony-docker
         tags: balcony-docker
         balcony_dockerhub_user: "{{ docker_hub_login }}"
         balcony_dockerhub_password: "{{ docker_hub_password }}"
         balcony_dockerhub_email: "{{ docker_hub_email }}"
         become: yes

License
-------

BSD

Author Information
------------------

AlexV: alexandre.vincent@yujinrobot.com
