---
title: Docker Compose
description: Standard used for our docker-compose files
published: true
date: 2023-05-05T17:35:48.358Z
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
  
## Services

We use 3 names for our services :
- db : for a database
- frontend : for a web interface or another frontend
- backend : for anykind of backend (exemple: the ldap service)