# Preconfigured development environment for Symfony

This docker file based official php docker file and includes some required extensions for Symfony development.

I used PHP-FPM as default so this docker file runs php-fpm.

You can change default pool configuration with mounting your pool configuration to that point `/usr/local/etc/php-fpm.d/www.conf` 


# Tags

- delirehberi/php7-symfony:7.1
- delirehberi/php7-symfony:7.3

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
    image: delirehberi/php7-symfony:7.3
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