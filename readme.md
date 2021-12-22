# Ansible Smarthome Playbook

## About

This playbook deploys OpenHAB, Deconz and NGINX in Docker containers and does some default configs on host. 
Tested and used for production in Ubuntu 20.04.

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

    deconz_host:
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

## Usage
### Fresh Install
1. Copy default.config.yml
2. Rename copy to config.yml
3. Edit config.yml with your settings
4. Install depedencies with 

        ansible-galaxy install -r requirements.yml

5. Run the Playbook with 

        ansible-playbook setup.yml --skip-tags "openhab_config,deconz_config" -i "your_host" -k --ask-become

At this point you would be done if you want to migrate data follow

### Migrate or update openhab configs
1. Copy your openhab conf folder and addons.config to 

        roles/openhab/files/openhab

2.  Run this

        ansible-playbook setup.yml -t "openhab_config,deconz_config" -i "your_host" --ask-become
