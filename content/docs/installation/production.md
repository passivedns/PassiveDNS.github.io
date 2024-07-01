---
title: Production
weight: 1
type: docs
prev: docs/installation/
next: docs/installation/development
---

This page explains how to run the app in production environment

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


## Run the application

```bash
cd pdns-docker/prod
docker compose up
```

This should start all of the containers. 

## Create an admin user

By default, the database is not initialized. You **need** to create an admin user in order to initialize it and be able to use the application.

Either by Docker Desktop or by CLI, in a shell of the API container :

{{< tabs items="Docker Desktop, CLI" >}}

    {{< tab >}}
    Open the api container and go into the `Exec` tab:
    ```bash
    bash
    ```
    {{< /tab >}}
    {{< tab >}}
    Open a shell inside the api container:
    ```bash
    docker exec -it api sh
    ```
    Then inside the shell:
    ```bash
    bash
    ```
    {{< /tab >}}

{{< /tabs >}}

Then, create a new admin user with:
```
../docker-entrypoint.sh create-user [USERNAME] [PASSWORD] [EMAIL] --admin
```
Replacing `[USERNAME]`, `[PASSWORD]` and `[EMAIL]` with the corresponding data.

## Create the scheduler

One the application is up and running, you can log in with the admin credentials, and you can setup a scheduler in the `Users` section. Once that is done, you need to update the `.env` file that is in the `/pdns-docker/prod/`:

```env {filename=".env", linenos=table,linenostart=4}
...
SCHEDULER_USERNAME=[USERNAME]
SCHEDULER_PASSWORD=[PASSWORD]
...
```

Replacing `[USERNAME]` and `[PASSWORD]` with the corresponding data.