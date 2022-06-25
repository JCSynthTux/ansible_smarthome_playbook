# Ansible Smarthome Playbook

## About

This playbook deploys OpenHAB, Deconz and NGINX in Docker containers and does some default configs on host. 
Tested and used for production in Ubuntu 20.04.

## Features

- User setup
- Basic SSH Hardening
- Basic packages installation
- Installation and setup of Docker
- Deploying NGINX in a container
- Deploying OpenHAB in a container
- Deploying deConz in a container
- Config pushing for OpenHAB
- Config pushing for deConz


## Roles 

- [User management role by singleplatform-eng](https://github.com/singleplatform-eng/ansible-users)
- [Security role by Jeff Geerling](https://github.com/geerlingguy/ansible-role-security)
- [Package role by GROG](https://github.com/GROG/ansible-role-package)
- [NGINX role by me](https://github.com/JCSynthTux/ansible-role-docker-nginx)
- [Docker role by Jeff Geerling](https://github.com/geerlingguy/ansible-role-docker)
- [Docker ARM role by Jeff Geerling](https://github.com/geerlingguy/ansible-role-docker_arm)

## Variables
An example setup for vars can be seen in ```examples/examples_vars.yml```.

For vars of other roles please check the docs of the role.

### Variables for migration
These vars are only needed for migrating your data from an old install to one managed by this playbook. Therefore **you should not add those vars to your ```vars.yml```**.
```
openhab_config: /path/to/openhab_config
```
Set ```openhab_config``` to the path where your openhab config is located. The directory should contain the folders in which items, things and mappings are configured.
```
openhab_addons_config: /path/to/your/addons.cfg
```
```openhab_addons_config``` should point to your addons.cfg file.
```
deconz_config: /path/to/deconz_config
```
```deconz_config``` should contain all the contents of ```/opt/deCONZ```, which is the inside mount point of the deconz docker container. So in case you come from an existing container install you could basically use your whole volume. 

## Tags
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

### Optional Migration
#### OpenHAB
For Migrating OpenHAB Config run ```ansible-playbook setup.yml -i inventory/hosts.yml --tags "openhab_config" --extra-vars='openhab_config=/path/to/config/dir,openhab_addons_config=/path/to/addons.cfg```

You can either omit ```openhab_config``` or ```openhab_addons_config``` and only the one left will be migrated.

#### Deconz
For Migrating Deconz Config run ```ansible-playbook setup.yml -i inventory/hosts.yml --tags "deconz_config" --extra-vars='deconz_config=/path/to/deconz_config```
