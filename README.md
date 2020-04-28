# Preconfigured development environment for Symfony

The Symfony Docker Image is based official PHP-FPM docker file and it including several required and a few optional extensions for Symfony. Also, this docker image contains `composer` tool.

You can create `www.conf` file for customized pool configuration and then you can mount it to the image with this path: `/usr/local/etc/php-fpm.d/www.conf`.

# Available Tags

- delirehberi/php7-symfony:7.1
- delirehberi/php7-symfony:7.2
- delirehberi/php7-symfony:7.3
- delirehberi/php7-symfony:7.4

# Included Extensions and Tools:

- composer
- git
- pdo
- pdo_mysql
- pdo_pgsql
- mbstring
- intl
- zip
- iconv
- mcrypt
- gd


## Example docker-compose configuration

```yaml
version: '3'
services:
  php:
    image: delirehberi/php7-symfony:7.4
    ports:
      - 9000:9000
    volumes:
      - ./:/app/
      - ./docker/php/www.conf:/usr/local/etc/php-fpm.d/www.conf
    environment:
      APP_ENV: dev
      APP_SECRET: "CHANGE_ME"
      DATABASE_URL: "pgsql://dbuser:dbpassword@postgresql:5432/dbname"
      REDIS_HOST: redis
  nginx:
    image: nginx:1.17.8
    ports:
      - 80:80
    volumes:
      - ./:/app/
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php
  postgresql:
    image: postgres:11
    ports:
      - 5432:5432
    depends_on:
      - php
    volumes:
      - ./:/app/
    environment: 
      POSTGRES_DB: dbname
      POSTGRES_USER: dbuser
      POSTGRES_PASSWORD: dbpassword
  node:
    image: node:13
    volumes:
      - ./:/app/
    working_dir: /app
  redis:
    image: redis
    depends_on:
      - php
```
