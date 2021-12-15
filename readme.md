# Ansible Smarthome Playbook

## About

This playbook deploys OpenHAB, Deconz and NGINX in Docker containers and does some default configs on host.

## Features

- Basic SSH Hardening
- Basic packages installation
- Installation and setup of Docker
- Deploying NGINX in a container
- Deploying OpenHAB in a container
- Deploying deConz in a container
- Config pushing for OpenHAB
- Config pushing for deConz

## Variables
Place your variable files in vars/

general.yml

    user: 
    ssh_key: 

openhab.yml

    openhab_host:

deconz.yml

    deconz:
    deconz_controller: /dev/ttyUSB0 #Or Whatever Your Path is

## Tags
- common
    - Applies basic SSH Hardening
    - Adds SSH key for user
    - Installs basic Packages
- docker
  - Adds docker repo
  - Installs docker via apt
  - Adds user to docker group
- nginx
  - Creates docker network
  - Creates docker volumes
  - Deploys nginx container
- deconz
  - Creates user for deconz container
  - Creates directory for deconz container
  - Deploys deconz container
- openhab
  - Creates user for openhab container
  - Creates directories for openhab container
  - Deploys openhab container
- openhab_config
  - Pushes openhabs config and addon.cfg file to host
- deconz_config (Usually only for migration)
  - Pushes deconz configs to host
- initial
  - Does all of the above, except for deconz_config and openhab_config

## Usage
### Fresh Install
1. Add variables to vars/
2. Install depedencies with 

        ansible-galaxy install -r requirements.yml

3. Run the Playbook with 

        ansible-playbook setup.yml -t "initial" -i "your_host" -k --ask-become

    At this point you would be done if you want to migrate data follow
4.         ansible-playbook setup.yml -t "openhab_config,deconz_config" -i "your_host" --ask-become


