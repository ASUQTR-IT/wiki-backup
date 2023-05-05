---
title: Docker Compose
description: Standard used for our docker-compose files
published: true
date: 2023-05-05T17:41:04.575Z
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

It will give us something like this :

```yml
	version: "3.9"
  
  services: 
```

  
### Services

We use 3 names for our services :
- db : for a database
- frontend : for a web interface or another frontend
- backend : for anykind of backend (exemple: the ldap service)


### The service attributes

We follow a specific order to maintain uniformity in our docker-compose files, this allows us to kinkly find something whatever the service