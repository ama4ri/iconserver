# Besticon service Deployment

## Description

You can use this project to deploy favicon-service with docker-compose.
Project provides iconserver with nginx as proxy with ssl and load-balancer if we want more than besticon contaner.
It means that project has two services in docker-compose file:
  - besticon contaner
  - nginx container
And our project helps you to prepare docker compose file for launch.
The deployment has been tested with RedHat and Ubuntu.
Ansible helps us to achive this goal.

Additionally we have small ansible role which helps us to create self-sign SSL certificate.

!!!!! Caution !!!!!
If you use ansible >= 2.17:
  And if remote RedHat family server has platform-python == 3.6.8:
    Ansible yum/dnf module won't work!!!
!!!!!!!!!!!!!!!!!!!
## Preparation to deploy

Your system must have python3 and pip3 packages => 3.8!!!

Project has two based group of variables:

### Envirnment variables for Besticon docker container
It support all env wich you can find in [official github] (https://github.com/mat/besticon) project for besticon.
And of couse, you can find there all requared information about how besticon works.
In this project we have .env file in project root directory which contains all these variables
File .env helps to pass variables to Besticon docker container.

### Variables for ansible playbook file with examples

  - ansible_python_interpreter: /usr/bin/python3.11
    Description: you can choose python version on remote server to execute ansible tasks
  
  - ansible_ssh_user=root
    Description: user for remote server

  - ansible_ssh_pass=tr1c0l0rOtt
    Description: password from remote server user

  - state: present or absent
    Description: variable to install or delete project from remote server

  - ssl_dir: /opt/iconserver/ssl
    Description: dir to store created ssl key and certifivate

  - server_name: iconserver.tech
    Description: domain name which will be used to create ssl certificate and it uses as nginx server_name

  - key_size: 4096
    Description: private key size to ssl certifgicate

  - key_type: RSA
    Description: private key type. Avaliable options: DSA, ECC, Ed25519, Ed448, X25519, X448

  - country_name: CR
    Description: country code for ssl certificate creation

  - email_address: admin@iconserver.tech
    Description: email address for ssl certificate creation

  - organization_name: iconserver
    Description: organization name for ssl certificate creation

  - packages_list: [ chrony, docker-ce, docker-ce-cli, docker-compose-plugin ]
    Description: packages list which will be installed to host server

  - systemd_services: [ chronyd, docker ]
    Description: list of required serives which will be start on host server

  - nginx_dir: /opt/iconserver/nginx/
    Description: dir on host server for nginx config

  - project_dir: /opt/iconserver
    Description: main dir for project

  - compose_version: 1.29.0
    Description: version of docker compose to install on host server

  - besticon_tag: v3.20.0
    Description: version of Besticon server docker image

  - replicas: 1
    Description: replica count of Besticon server

  - nginx_tag: latest
    Description: version of nginx docker image   

## Deploy and run docker-compose project
After we set all variables or leave default values we can run ansible playbook to deliver our project to a server and launch it.

```
ansible-playbook -i hosts.ini deploy.yml 
```

## Service usage

To find out if service work or not you can execute the following command via terminal:

```
curl -k 'https://{ you server ip or server_name(from cofnig)server_name(from cofnig)}'
```