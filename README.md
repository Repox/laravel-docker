# Docker Compose environment for Laravel

This is a project that you can use in existing projects or new projects. 

[Download](https://github.com/Repox/laravel-docker/archive/master.zip) this repository's contents and add the files to the root of your Laravel project. Discard this `README.md` file to avoid overwriting your own or an existing one.

## Stack

The stack is be default configured to the following:

- PHP 8.0 ([image](https://hub.docker.com/r/repox/laravel-dev-php))
- MySQL 8.0 ([image](https://hub.docker.com/_/mysql))
- Nginx ([image](https://hub.docker.com/_/nginx), latest)
- Redis ([image](https://hub.docker.com/_/redis), latest)
- Composer v2 (built into PHP image)

You can use [PHP 7.4 and PHP 7.3](hhttps://hub.docker.com/r/repox/laravel-dev-php) by specifyng the version tag in `docker-compose.yml` where the `repox/laravel-dev-php` image is referenced; i.e.:

```
repox/laravel-dev-php:7.3
repox/laravel-dev-php:7.4
repox/laravel-dev-php:8.0
repox/laravel-dev-php:latest # Provides the latest PHP version
```

The PHP images used for this repository is made specifically for developing with Laravel in this context.

### Start the local environment

Starting the environment is prette straight forward.

```bash
$ docker-compose up -d
```

This will have a working environment available at [http://localhost](http://localhost)

You'd might already have things running locally on port 80, which might give you some errors. In that case it might be beneficial to change the exposed port to something like `8000` or `8080`. I just chose `80` for my own convenience. Exposing the `nginx` service on port `8000` instead would mean that you configure the `docker-compose.yml` and change the `port` configuration to something like the following:

```yaml
  nginx:
    image: nginx
    ports:
      - "8000:80"
```

Left side of the port specification is the Docker host port which maps to the container port specified on the right side.

Hostnames for MySQL and Redis (and other added services) is accessed via their service name.<br>
The MySQL container has been labeled `db` in `docker-compose.yml` and therefore should the `DB_HOST` parameter in your `.env` file in Laravel reflect this as the hostname:

```ìni
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

### Stop the environment

To stop the environment, but keeping volume data, run the following:

```bash
$ docker-compose stop
```

### Removing the environment

To stop the environment and remove volume data and container network (eg. for starting over):

```bash
$ docker-compose down -v
```

## Helping commands

There's been added some containers that can interact directly with the environment to utilize Composer and Artisan within the docker network.

Examples:

```bash
$ docker-compose run artisan migrate:refresh
$ docker-compose run artisan queue:work --once
$ docker-compose run composer require laravel/sanctum
$ docker-compose run composer dump
```



## Accessing MySQL

This repository contains a configuration made specifically for allowing you to access it with your favorite client directly from your host by connecting to the exposed port via `127.0.0.1`. As an example:

```bash
$ mysql -h 127.0.0.1 -p 3306 -u root -proot
```

Same procedure can be used in ie. Sequel Ace, MySQL Workbench, HeidiSQL or what ever tool you'd might prefer.
