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

If you only need the SSH hardening part and basic package install check [Initial Setup role by me](https://github.com/JCSynthTux/ansible-role-debian-initial-setup).

For the docker installation and setup check the [Docker role by Jeff Geerling](https://github.com/geerlingguy/ansible-role-docker)

## Variables
This section will only describe variables needed for roles included in this playbook.

For variables of roles maintained outside of this please see the repo of said roles.
- [Initial Setup role by me](https://github.com/JCSynthTux/ansible-role-debian-initial-setup)
- [NGINX role by me](https://github.com/geerlingguy/ansible-role-docker) *Leave ```nginx_network``` on default*
- [Docker role by Jeff Geerling](https://github.com/geerlingguy/ansible-role-docker)

```
openhab_install: true
deconz_install: true
mqtt_install: true
```
Set to ```true``` to install component. ```false``` or omitting will despawn the container, but data will still be there.

```
timezone: Europe/Berlin 
openhab_host: openhab.smarthome.local
smarthome_users:
  - foo
  - bar
  - foobar
```
Set ```openhab_host``` to domain on which OpenHAB should be reachable. Add linux users to ```smarthome_users```, to add them to the ```openhab``` linux group. Those users will be able to edit openhab config later.

```
deconz_controller: /Path/To/Deconz/Controller
deconz_host: deconz.smarthome.local
```
Set ```deconz_controller``` to the path where your controller is located. See the ```--device``` command line option [here](https://github.com/deconz-community/deconz-docker#command-line-options) for information about the location. Set ```deconz_host``` to domain on which the Deconz WebUI should be reachable.

### Variables for migration
These vars are only needed for migrating your data from an old install to one managed by this playbook. Therefore **you should remove those vars from your ```vars.yml``` after migrating**.
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
You can also migrate configs from OpenHAB, Deconz and MQTT with this playbook. After running the ```Fresh Install``` section you would do the following:
1. Set the vars metioned in the **Variables for migration** section
2. ansible-playbook setup.yml -i inventory/hosts.yml --tags "deconz_config,openhab_config"