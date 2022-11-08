### Downloading Docker Files
- ```docker pull ghcr.io/secure-compliance-solutions-llc/gvm-docker:debian-master-data-full```
- Use this command to install the files needed before running the second command to install docker Hub.
- ```docker pull securecompliance/gvm:debian-master-data-full```
* This will ensure you are currently running the updated version of docker.
### Downloading Docker WordPress
- This will download all the files you need for wordpress to work.
- ```git clone https://github.com/aschmelyun/docker-compose-wordpress```
- After run the command
- ```docker-compose up -d --build site```
- Go to the docker-compose.yml file and change it to this 
```services:
  db:
    # We use a mariadb image which supports both amd64 & arm64 architecture
    image: mariadb:10.6.4-focal
    # If you really want to use MySQL, uncomment the following line
    #image: mysql:8.0.27
    command: '--default-authentication-plugin=mysql_native_password'
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    expose:
      - 3306
      - 33060
  wordpress:
    image: wordpress:latest
    volumes:
      - wp_data:/var/www/html
    ports:
      - 80:80
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data:
  wp_data:
```
- After that use this command to bring them all up and bind them to their ports and then you should be able to contact using your ip.
- ```docker-compose up -d```
- This will show all 3 of the running containers hopefully
- ``docker ps```
- Now you just follow along with the admin set up and fill out whatever information you need to but wordpress is set up
- To turn off the WordPress database use the command.
- ```docker-compose down```
