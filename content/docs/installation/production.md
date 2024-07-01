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

Then, in the `/pdns-docker/dev/` :

{{< tabs items="Unix, Windows" >}}

    {{< tab >}}
    ```bash
    ./init.sh
    ```
    {{< /tab >}}
    {{< tab >}}
    ```bash
    ./init.ps1
    ```
    {{< /tab >}}

{{< /tabs >}}

```bash
cd PassiveDNS
git checkout -b <branch-name>
```

## Run the application

```bash
docker compose up
```

This should start all of the containers, but the web server and the frondend are not up yet. 
You can use [Docker Desktop](https://www.docker.com/products/docker-desktop/) to access and manage them or the [CLI](https://docs.docker.com/get-started/docker_cheatsheet.pdf) directly.

## Run web server and frontend

{{< tabs items="Docker Desktop, CLI" >}}

    {{< tab >}}
    **Web Server**: open the api container, then into the `Exec` tab:
    ```bash
    bash
    poetry run uvicorn passiveDNS.webserver:app --reload --host 0.0.0.0 --port 8080
    ```

    **FrontEnd**: open the front container, then into the `Exec` tab:
    ```bash
    bash
    npm install && npm run dev
    ```
    {{< /tab >}}
    {{< tab >}}
    **Web Server**: open a shell inside the api container:
    ```bash
    docker exec -it api sh
    ```
    Then inside the shell:
    ```bash
    poetry run uvicorn passiveDNS.webserver:app --reload --host 0.0.0.0 --port 8080
    ```

    **FrontEnd**: open a shell inside the front container:
    ```bash
    docker exec -it front sh
    ```
    Then inside the shell:
    ```bash
    npm install && npm run dev
    ```
    {{< /tab >}}

{{< /tabs >}}

## Create an admin user

By default, the database is not initialized. You **need** to create an admin user in order to initialize it and be able to use the application.

Either by Docker Desktop or by CLI, in a shell of the API container (separate from the one you are running the app with) :

```bash
../docker-entrypoint.sh create-user [USERNAME] [PASSWORD] [EMAIL] --admin

```

Replacing `[USERNAME]`, `[PASSWORD]` and `[EMAIL]` with the corresponding data.