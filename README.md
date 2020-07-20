# php-mysql-using-docker-compose

<!--- See https://shields.io for others or to customize this set of shields.  --->
![GitHub language count](https://img.shields.io/github/languages/count/ssabit/php-mysql-using-docker-compose?style=flat-square)
![GitHub top language](https://img.shields.io/github/languages/top/ssabit/php-mysql-using-docker-compose?style=flat-square)
![GitHub last commit](https://img.shields.io/github/last-commit/ssabit/php-mysql-using-docker-compose?color=red&style=flat-square)
![GitHub license](https://img.shields.io/github/license/ssabit/php-mysql-using-docker-compose?style=flat-square)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/ssabit/php-mysql-using-docker-compose?style=flat-square)
![GitHub stars](https://img.shields.io/github/stars/ssabit/php-mysql-using-docker-compose?style=flat-square)




* We'll start by creating a folder for this project:
```mkdir docker && cd docker```

* Create another subdirectory, ```/php``` which contains the following ```index.php``` file:

```
<html>
    <head>
        <title>Hello Docker</title>
    </head>

    <body>
        <?php
            echo "Hello, Docker!";
        ?>
    </body>
</html>
```

* Populate ```docker-compose.yml``` with the following configuration:

```
# docker-compose.yml

version: '3'
 
services:
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: pass
      MYSQL_DATABASE: db
      MYSQL_USER: root
      MYSQL_PASSWORD: pass
    ports:
    - "9906:3306"
 
  web:
    image: php:7.2.2-apache
    container_name: php_web
    depends_on:
    - db
    volumes:
    - ./php/:/var/www/html/
    ports:
    - "8080:80"
    stdin_open: true
    tty: true
 
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    depends_on:
    - db
    external_links:
    - db:mysql
    ports:
    - "9191:80"
    environment:
      MYSQL_USER: root
      MYSQL_PASSWORD: pass
      MYSQL_ROOT_PASSWORD: pass
      PMA_HOST: db

```

* Our directory structure should look as follows:
```
.
├── docker-compose.yml
└── php
    └── index.php

```

* Execute ```docker-compose up -d``` in the terminal and load http://localhost:8080/ in your browser.

* You can use ```docker-compose start``` and ```docker-compose stop``` to manage the containers after the creation.

* We use port-forwarding to connect to the inside of containers from our local machine.
  * webserver: http://localhost:8080
  * db: mysql://root:pass@localhost:9191/db
  * OR
    * http://localhost:9191/
    * username: root
    * password: pass

* Our local directory, ```./php```, is mounted inside of the webserver container as ```/var/www/html/```
  * The files within in our local folder will be served when we access the website inside of the container



