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
For variables copy and paste the default.config.yml, rename it to config.yml and edit the variables to your liking. 

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
1. Clone this repo
2. Create a directory in ```inventory/host_vars``` named after your host e.g. smarthome.local
3. Copy the file ```examples/example_vars.yml``` to the before created folder
4. Rename the copied and pasted file from ```example_vars.yml``` to ```vars.yml```
5. Edit the ```vars.yml``` file to your liking - see variables
6. Edit the ```inventory/hosts.yml``` file and make sure line 5 matches the name of the directory you created before and replace the IP with the one of your smarthome host
7. In the root of this project run ```ansible-galaxy install -r requirements.yml```
8. Run the playbook with ```ansible-playbook setup.yml -i inventory/hosts.yml -k --ask-become``` in the projects root directory