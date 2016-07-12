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

 - yujin_dockerhub_user: The username on dockerhub used to retrieve balcony image
 - yujin_dockerhub_password: the password
 - yujin_dockerhub_email: the email address 
 - yujin_docker_image: the docker image
 - yujin_docker_user: the user to add to docker group

Dependencies
------------

 - marvinpinto.docker
 
Example Playbook
----------------

    - hosts: 
       - concert
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
       
       - role: yujinrobot.yujin-docker
         tags: yujinrobot.yujin-docker
         yujin_dockerhub_user: "{{ docker_hub_login }}"
         yujin_dockerhub_password: "{{ docker_hub_password }}"
         yujin_dockerhub_email: "{{ docker_hub_email }}"
         yujin_docker_image:
           name: hello-world
           tag: latest
           force: yes

License
-------

BSD

Author Information
------------------

AlexV: alexandre.vincent@yujinrobot.com
