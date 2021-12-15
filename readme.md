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

