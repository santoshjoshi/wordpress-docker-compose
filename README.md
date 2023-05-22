# Wordpress Docker Compose
docker compose for wordpress

## Issues Faced
I was using ranchor desktop and faced lot of issues while mounting, This linke helped
https://github.com/rancher-sandbox/rancher-desktop/issues/1209#issuecomment-1155189938


## Description

The docker-compose.yaml file is a configuration file used to define and manage a Docker-based multi-container application. It describes the services, networks, and volumes required for the application to run.
### Services

The docker-compose.yaml file defines the following services:
### Database (db)

- Image: mysql:latest
- Volumes: Mounts the /mysql/data directory on the host machine to the /var/lib/mysql directory inside the container, allowing persistent storage of MySQL data.
- Restart: Always restarts the container if it stops.
- User: Runs the container as the root user.

**Environment Variables:**
- MYSQL_ROOT_PASSWORD: Sets the root password for the MySQL database.
- MYSQL_DATABASE: Creates a database named "wordpress".
- MYSQL_USER: Creates a user named "wordpress".
- MYSQL_PASSWORD: Sets the password for the "wordpress" user.
 
Networks: Connects to the "wpsite" network.

### phpMyAdmin (phpmyadmin)

- Depends on: "db" service, ensuring that the database service is up before starting phpMyAdmin.
- Image: phpmyadmin/phpmyadmin
- Restart: Always restarts the container if it stops.
- User: Runs the container as the root user.
- Ports: Maps port 8080 on the host machine to port 80 inside the container, allowing access to phpMyAdmin from the host machine.

**Environment Variables:**
- PMA_HOST: Specifies the host name of the database service, which is set to "db".
- MYSQL_ROOT_PASSWORD: Sets the root password for the MySQL database.

Networks: Connects to the "wpsite" network.

### Wordpress (wordpress)

- Depends on: "db" service, ensuring that the database service is up before starting Wordpress.
- Image: wordpress:latest
- User: Runs the container as the root user.
- Ports: Maps port 8000 on the host machine to port 80 inside the container, allowing access to Wordpress from the host machine.
- Restart: Always restarts the container if it stops.
- Volumes: Mounts the ./code directory on the host machine to the /var/www/html directory inside the container, allowing custom code to be added to Wordpress.

**Environment Variables:**
- WORDPRESS_DB_HOST: Specifies the host and port of the database service, set to "db:3306".
- WORDPRESS_DB_USER: Specifies the database user, set to "wordpress".
- WORDPRESS_DB_PASSWORD: Specifies the password for the database user, set to "wordpress".
- TAR_OPTIONS: Specifies options for the tar command used during file extraction, set to "--no-same-owner".
 
 Networks: Connects to the "wpsite" network.

### Networks

The docker-compose.yaml file defines a network named "wpsite"

## How To start

docker-compose up -d

## How To Stop

docker-compose down 

## Access

- To acces phpMyAdmin hit http://localhost:8080
- To access Wordpress browse to http://localhost:8000.
