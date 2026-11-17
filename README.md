# Symfony Docker

A minimal Docker Compose + Make starter that bootstraps a new Symfony 6.4 web application with PHP-FPM, Nginx, MySQL
and MailHog.

## Context

Built in May 2025 as a reusable starting point for Symfony projects, so a new application can be created and run
with two commands instead of setting up PHP, a web server and a database by hand. The same Docker setup is used in the
`symfony-conference` project. A variant for Symfony 7.2 lives in the
[`symfony-7.2`](https://github.com/okarabudka/symfony-docker/tree/symfony-7.2) branch.

## Features

- One command to build and start the whole stack
- One command to create a Symfony 6.4 skeleton with the `webapp` pack inside the container and move it into the
  project root
- PHP 8.2-FPM image with Composer and the `pdo_mysql` and `gd` extensions
- Nginx serving `public/` on port 8080
- MySQL 8.0 with a preconfigured `DATABASE_URL`
- MailHog for catching outgoing mail (web UI on port 8025, SMTP on port 1025)
- Commented-out Redis and Node services ready to enable

## Tech stack

- Docker, Docker Compose
- PHP 8.2-FPM, Composer
- Nginx (alpine)
- MySQL 8.0
- MailHog
- GNU Make

## Architecture

- `Dockerfile`: PHP 8.2-FPM image with system libraries, PHP extensions and Composer
- `docker-compose.yml`: services `app` (PHP-FPM), `web` (Nginx), `mysql`, `mailhog`; the project directory is mounted
  into `/var/www` of both `app` and `web`
- `docker/nginx/default.conf`: Nginx virtual host forwarding PHP requests to the `app` container
- `Makefile`: thin wrappers around `docker compose` and `docker exec` (`up-containers`, `init-webapp`,
  `composer-install`, `help`)

The development database credentials in `docker-compose.yml` are local defaults only and must be changed for any
non-local use.

## Getting started

Requirements: Docker with Docker Compose and Make.

```bash
git clone https://github.com/okarabudka/symfony-docker.git my-project
cd my-project

make up-containers   # docker compose up -d --build
make init-webapp     # composer create-project symfony/skeleton:"6.4.*" + composer require webapp
```

Then open:

- http://localhost:8080 - Symfony
- http://localhost:8025 - MailHog

After `init-webapp`, run `make composer-install` whenever dependencies need to be reinstalled. Run `make help` for the
list of targets.

## Testing

There are no automated tests; the repository contains only infrastructure configuration.

## Status

Completed starter template from May 2025. Not actively developed.

## License

MIT, see [LICENSE](LICENSE).
