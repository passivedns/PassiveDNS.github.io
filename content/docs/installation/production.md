---
title: Production
weight: 1
type: docs
prev: docs/installation/
next: docs/installation/development
---

This page explains how to run the app in production environment

## Requirements

In order to properly run the application, you need at least the following installed:
- Git
- Docker

## Installation

Start of by downloading or cloning the [pdns-docker](https://github.com/passivedns/pdns-docker/) repository

{{< tabs items="HTTPS, SSH, GitHub CLI" >}}

    {{< tab >}}
    ```bash
    git clone https://github.com/passivedns/pdns-docker.git
    ```
    {{< /tab >}}
    {{< tab >}}
    ```bash
    git clone git@github.com:passivedns/pdns-docker.git
    ```
    {{< /tab >}}
    {{< tab >}}
    ```bash
    gh repo clone passivedns/pdns-docker
    ```
    {{< /tab >}}

{{< /tabs >}}

## Setup the database password

Before running the application, you need to change the database root password. This can be done in the `pdns-docker/prod/docker-compose.yml` file:

```docker {filename="docker-compose.yml", linenos=table,linenostart=9}
...
    environment:
      - ARANGO_ROOT_PASSWORD= [ADD PASSWORD HERE]
...
```

{{< callout type="warning" >}}
  Please note that if you do not setup a password, the database will be accessible freely.
{{< /callout >}}

## Run the application

```bash
cd pdns-docker/prod
docker compose up
```

This should start all of the containers. 
If you are on MacOS and encounter any problem with the volumes while running the containers:
The volume management with Docker and MacOS is a little bit tricky, so here is a simple fix:
Update the file `pdns-docker/prod/docker-compose.yml`, by replacing the volume path with `./` instead of `/`.

In arango:
```docker {filename="docker-compose.yml", linenos=table,linenostart=7}
...
    volumes:
      - ./passivedns_arangodb:/var/lib/arangodb3
...
```
And in Redis:
```docker {filename="docker-compose.yml", linenos=table,linenostart=48}
...
    volumes:
      - ./passivedns_redis_data:/data
...
```

## Create an admin user

By default, the database is not initialized. You **need** to create an admin user in order to initialize it and be able to use the application.

Either by Docker Desktop or by CLI, in a shell of the API container :

{{< tabs items="Docker Desktop, CLI" >}}

    {{< tab >}}
    Open the api container and go into the `Exec` tab.
    {{< /tab >}}
    {{< tab >}}
    Open a shell inside the api container:
    ```bash
    docker exec -it api sh
    ```
    This should open a new window shell.
    {{< /tab >}}

{{< /tabs >}}

Then, create a new admin user with:
```bash
../docker-entrypoint.sh create-user [USERNAME] [PASSWORD] --admin
```
Replacing `[USERNAME]` and `[PASSWORD]` with the corresponding data.

## Setup the scheduler

Once the application is up and running, you can log in with the admin credentials, and you can setup a scheduler in the `Users` section. Once that is done, you need to update the `.env` file that is located in the `/pdns-docker/prod/`:

```env {filename=".env", linenos=table,linenostart=4}
...
SCHEDULER_USERNAME=[USERNAME]
SCHEDULER_PASSWORD=[PASSWORD]
...
```

Replacing `[USERNAME]` and `[PASSWORD]` with the corresponding data.

You then have to restart the scheduler container, and everything is ready!