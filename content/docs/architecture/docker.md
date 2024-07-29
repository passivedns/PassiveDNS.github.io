---
title: Docker containers
weight: 1
prev: docs/architecture/database
next: docs/architecture/ci_cd
---

This application is divided into docker containers. Theses containers are used to build images for the production environment.
Here is a schema of the structure:

![Docker structure](/screenshots/Docker_structure.drawio.png)

## Front

The front (`front` folder) is made with Vue3. The components of the pages are in `front/src/components` and the services for managing the requests are in `front/src/services`.

In production environment, the front app is available at https://localhost.
In development environment, the front app is available at https://localhost:8081.

### Dependencies

- vue 3
- vite
- axios
- chokidar
- gridjs
- jsonwebtoken

More details can be found in the `front/package.json` file.

## Back

The api container contains the web server (`passiveDNS` folder), with models for interaction between the database the requests and controllers (`apiv2` folder) for interacting with the front. 

The documentation for the `apiv2` requests/responses can be found at https://localhost:8080/docs when the api container is running.

### Dependencies

- python 3.12
- fastapi
- pydantic

More details can be found in the `pyproject.toml` file.

## Database

The database container runs the arangoDB local database. 
By default, the database is available at https://localhost:8529, using username and password setup during the [installation](/docs/installation).

For more details on the structure of the database, please visit the [database](/docs/architecture/database) section.

## Scheduler

The scheduler is run on an external container so that it is independant and can be run from other places for instance.
It relies on Redis which is run automatically with it.

The scheduler is programmed to resolve every domain name once a day, check if there have been any changes and notify people using the channels if so. 

### Dependencies

- python 3.12
- celery

## Redis

Redis is used only by the scheduler, and is linked to it. For more information about Redis, visit their [website](https://redis.io/).