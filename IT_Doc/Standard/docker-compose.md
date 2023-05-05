---
title: Docker Compose
description: Standard used for our docker-compose files
published: true
date: 2023-05-05T18:02:18.851Z
tags: it, server
editor: markdown
dateCreated: 2023-05-05T17:35:48.358Z
---

# Docker compose files

Our docker compose files follow a few standards.

## Seperate files

We seperate our docker-compose files. We will not mixe multiple services in a single file. For exemple: Our gitea git is in the services directory on our server in its own gitea directory. 

- ./services
	- ./gitea
  		- docker-compose.yml
      - ./db-data
	- ldap
  
## File structure

We order our files in the following order :

1. version : the docker-compose version used by the file
2. services: the services in the present file
3. volumes: the volumes we need to specify globally (typically those not associated to a directory)
4. networks: The networks specifications

  
### Services

We use 3 names for our services :
- db : for a database
- frontend : for a web interface or another frontend
- backend : for anykind of backend (exemple: the ldap service)


#### The service attributes

We follow a specific order to maintain uniformity in our docker-compose files, this allows us to kinkly find something whatever the service is.

The service attributes order is as follows:
- image/build: one of the 2
- container_name: The container name, very important to allow us to properly monitor the containers
- restart: typicaly set to -> unless-stopped
- depends_on: indicates if the service depends on another service, exemple waiting for the db to finish
- healthcheck: specifies how the service should check for container heath
- volumes: The folder/files to associate outside:inside the container (start with ./ so we can copy the directories to backups)
- command: cmd commands to pass the container
- environment: the env files to pass to the container
- ports: The ports to open between the server:container. We will always specify the home on the serverside ```127.0.0.1:port:containerport```
- networks : specifies if the services uses a specific network


### File structure exemple

It will give us something like this :

```yml
	version: "3.9"
  
  services:
  	  frontend:
        image: gitea/gitea:1.19.1
        container_name: gitea
        restart: unless-stopped
        depends_on:
          db: # Will specify in the db service the healthcheck
        		condition: service_healthy
        volumes:
          - ./gitea-data:/data
          - /etc/timezone:/etc/timezone:ro
          - /etc/localtime:/etc/localtime:ro
        environment:
          - USER_UID=1000
          - USER_GID=1000
          - GITEA__database__DB_TYPE=postgres
          - GITEA__database__HOST=database:5432
          - GITEA__database__NAME=gitea
          - GITEA__database__USER=gitea
          - GITEA__database__PASSWD=someRandomPassword
        ports:
          - "127.0.0.1:3000:3000"
        networks:
          - ldap_network
    	
```