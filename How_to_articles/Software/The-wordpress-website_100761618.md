1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : The wordpress website

Created by Luc Allaire, last modified on Feb 27, 2023

When you wish to either transfer to Wordpress website to an other docker or an other server (still in a docker pls)

* [Transferring the wordpress website :](#Thewordpresswebsite-Transferringthewordpresswebsite:)
* [Resetting the wordpress website :](#Thewordpresswebsite-Resettingthewordpresswebsite:)
* [Docker-compose file exemple :](#Thewordpresswebsite-Docker-composefileexemple:)

  * [docker-compose.yml](#Thewordpresswebsite-docker-compose.yml)
  * [Breakdown of the docker-compose file :](#Thewordpresswebsite-Breakdownofthedocker-composefile:)

    * [General :](#Thewordpresswebsite-General:)
    * [Now in the wordpress service, we have a few new important tags:](#Thewordpresswebsite-Nowinthewordpressservice,wehaveafewnewimportanttags:)
    * [If you wish to modify this file, beware of the following points :](#Thewordpresswebsite-Ifyouwishtomodifythisfile,bewareofthefollowingpoints:)
* [Related articles](#Thewordpresswebsite-Relatedarticles)

**Important : if you have any issues with docker that don't seem to make sense, verify that you do NOT have the snap version of docker, it is very problematic.**

## Transferring the wordpress website :

1. Stop your website containers with **sudo docker-compose stop**. You may also remove the existing containers withe **docker-compose down**(make sure to do this if transferring elsewhere on the same server or the container will have naming conflicts ...
2. Copy the existing docker-compose file (important to conserve the existing database names and passwords or you will get database connection errors). Use sudo cp ./file-to-copy /directory-to-copy-to
3. Copy the existing database and wordpress files (db and wordpress in our exemple). Use sudo cp -R ./db ./wordpress /directory-to-copy-to. **Note**:*You want these files to be at the same level as your new docker-compose file or to modify that file to point to their new location.*
4. Change ownership of these files with sudo chown -R \[USER:\[GROUP\]\] db wordpress docker-compose.yml
5. Start your new containers with **sudo docker-compose up -d** in the directory with your data and new docker-compose file.

## Resetting the wordpress website :

1. Firstly stop your present website with **sudo docker-compose down** in its directory (**docker-compose down**stops the containers in the docker-compose file in the working directory and removes them from the machine).
2. Remove the data folders with**sudo rm -R db wordpress** (rm = remove, -R = flag to remove recursively, db wordpress = the folders to remove). **MAKE SURE THIS IS A GOOD IDEA FIRST !!**
3. Relaunch the website anew with **sudo docker-compose up d** (-d is a flag meaning detached, it will not run in your present terminal but in the background instead).
4. Connect to your new Wordpress website and quickly set an admin user and password

**Note : the folders used in the step-by-step instructions are those of the example docker-compose file, cross reference with your local file before executing anything.**

**To modify a docker-compose file use vim with sudo**

## Docker-compose file exemple :

### docker-compose.yml

**docker-compose.yml**

```
version: '3.1'

services:

  mariadb:
    image: mariadb
    container_name: db-asuqtr
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: db-asuqtr
      MYSQL_USER: asuqtr-Admin
      MYSQL_PASSWORD: UwUplsChangeMe
	  MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - ./db:/var/lib/mysql

  wordpress:
    depends_on: 
      - mariadb
    image: wordpress
    links:
      - mariadb:mysql
    container_name: wp-asuqtr 
    restart: unless-stopped
    ports:
      - 10569:80
    environment:
      WORDPRESS_DB_HOST: mariadb
      WORDPRESS_DB_USER: asuqtr-Admin
      WORDPRESS_DB_PASSWORD: UwUplsChangeMe
      WORDPRESS_DB_NAME: db-asuqtr
    volumes:
      - ./wordpress:/var/www/html

volumes:
  wordpress:
  db:
```

**For docker-compose volumes :**

The local file should always have **./** before it.

Ex. ./wp-data *In linux*

*. is the current directory, while the / indicates a directory, without these elements you risk not forming a proper link between your folders* **!!!**

### Breakdown of the docker-compose file :

> ```
> every word in bold is a different keyword in the docker-compose file
> ```

##### General :

First of you different **services** are different *containers* that you wish to rely.Each service will either have an**image**or**build**a Dockerfile. That is what will*run*in the container.

The**container\_name**is self explanatory, we use it to identify the service in*htop*or using*docker ps.*

The **restart** tag indicates to docker compose whether to restart the container or not, in this case we will always restart the container unless it was manually stopped (ex. for maintenance).

The **environment** tag indicates the environment variable you should pass to that container. It is also common practice to pull these form a *.env* file as it allows you to simply add your *.env*file to your *.gitignore*, avoiding sending sensitive data to your git.

The **volumes** tag is rather important, it allows you to link a folder inside your container to one outside your container. This allows for persistent data across restarts. In this case the *database* and the *wordpress* files are included in different volumes. The format used here is : **./local-data : /container-data**

##### Now in the **wordpress** service, we have a few new important tags:

The **depends\_on** indicates that our worpress service depends on our mariadb service, so it should not be initialise before it.

The **links** indicates to wordpress to treat our mariadb image as if it was directly mysql. This is necessary because wordpress is made to use mysql. Even if mariadb is a fork of mysql, we must make sure it is treated as if it was directly mysql (basically everytime the wordpress container is instructed to look for mysql, we redirect to mariadb).

The **ports** tag is extremely important. It works in a similar fashion to the volumes tag, we will link a port outside our container to one inside it (external-port:internal-port). You can also specify the ip adresse, it default to localhost (127.0.0.1). In our present setup, we only wish to redirect *http* to port 80 as *https i*s managed by cloudflare. The external port is what you will tell cloudflare to redirect users to (ex. *asuqtr.com*will be http forwarding to **localhost:10569.**

Lastly, in our wordpress environment variable we must indicate our database service (**WORDPRESS\_DB\_HOST**).

##### If you wish to modify this file, beware of the following points :

* You should keep the wordpress internal port to 80, the external port can be changed if you also change it on cloudflare, but keep it on the higher side.
* If you are keeping the files that already exist, don't change the images of either service. If you do change them, make sure you have a clean copy of everything) **Note** : using an fpm version of wordpress will dramatically increase the difficulty of transfers or backups (**KISS** - Keep It Simple Stupid).
* If you change the volumes in the docker-compose file, remember to transfer your existing data to those new folders if you wish to keep it.
* You can't easily change the env variable for the database without getting an "***Error establishing a database connection***". So simply don't change them for an existing setup (you would have to first change all that in the existing wordpress).

If you wish to modify the docker-compose file, make sure to understand what you're doing ... refer to the breakdown of the docker-compose for the website up top

**It is important to execute the docker-compose file you created in a directory owned by your user.** *I.E. do not place your docker-compose in an other place than in the home directory or a folder below there.*

For exemple, I keep my docker projects on my personnal server in a srv directory and after that in folders for each project. Exemple, the path to my portfolio docker-compose.yml is ~/srv/web/portfolio/dockercompose.yml

## Related articles

* Page:

[The wordpress website](/display/SUBUQTR/The+wordpress+website)
* Page:

[Running/testing ROS node in Docker](/pages/viewpage.action?pageId=29851650)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)