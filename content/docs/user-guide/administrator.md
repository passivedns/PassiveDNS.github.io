---
title: Administrator
weight: 3
prev: docs/user-guide/
next: docs/user-guide/scheduler
---

An administrator has access to everything a user has, and a few other functionalities:

![The administrator home page](/screenshots/admin-home.png)

They can:
- Manage the channels
- Manage the users
- Manage the schedulers

## Settings

The settings page allows the administrator to manage the existing channels and create new ones.
There are two types of channels :
- The email channel requires an SMTP host, port, a sender email and a sender password.
- The Redis channel requires a database number, a host, port, a queue name and a password (optional).

## Users

The users page allows the administrator to:
- Remove existing users,
- Create or edit a scheduler,
- Send an invitation for a new user,
- Manage requests from users.

## Create a new administrator or user

If there is a need for multiple administrators, a new administrator can be created using the shell inside the API container (see the [production installation](../../installation/production/#create-an-admin-user) for more details) using the following command:

```bash
../docker-entrypoint.sh create-user [USERNAME] [PASSWORD] [EMAIL] --admin
```
Replacing `[USERNAME]`, `[PASSWORD]` and `[EMAIL]` with the corresponding data.
Removing the `--admin` option will create a simple user.

## Add a new extern API

{{< callout type="info" >}}
  Please note that adding a new extern API requires to use the development environment and to rebuild the docker images after, to be using it in production environment.
{{< /callout >}}

If you need to add other externs APIs, it can be done this way: 

{{% steps %}}

### Add API data in the database

First, you need to gather the data of the api you want to add, and insert it in the `/passiveDNS/db/extern_apis.yml` document as follows:

```yml {filename="extern_apis.yml"}
...
---
  _key: "API_NAME"
  base_url: "BASE_URL"
  header: "NAME_OF_APIKEY_HEADER"
  ip: 
      method: "GET/POST"
      uri: "URI_FOR_IP_REQUEST"
  domain: 
      method: "GET/POST"
      uri: "URI_FOR_DOMAIN_REQUEST"

```

Replacing each line with the appropriate data.
This data will automatically be added to the database the next time you run the application.

### Add data formatting

Next, you need to update the `/passiveDNS/analytics/extern_api.py` file, to add the data formatting for your new API. To do so, you can get inspiration from the Virustotal or AlienVault formatting already existing.

There are 3 things to modify in this file :

First, add a global variable with the name you specified as `_key` in the `.yml` file.
```python {filename="extern_apis.yml", linenos=table,linenostart=9}
...
VIRUSTOTAL_API = "VirusTotal"
ALIENVAULT_API = "AlienVault"
NEW_API = "API_NAME"
...
```

Then, you need to add the formatting function for you new API, at the end of the file:
```python {filename="extern_apis.yml", linenos=table,linenostart=127}
...
    #New API formatting
    def __formatNEW(self, response):
        #Add the formatting of the response based on the other formatting functions.

        ...

        return out


...
```
{{< callout type="info" >}}
Note : The data returned should be a list with this format after passing through the function :
```json
[
    {
        "domain_name": ,
        "ip_address": ,
        "first_updated_at": ,
        "last_updated_at": ,
    },
    "..."
]
```
{{< /callout>}}


Finally, in `get_api` function:
```python {filename="extern_apis.yml", linenos=table,linenostart=74}
...
    def get_api(self, api_name: str):
        __MAPPING = {VIRUSTOTAL_API: self.__formatVT, ALIENVAULT_API: self.__formatAV, NEW_API: self.__formatNEW}

        assert api_name in __MAPPING

        return __MAPPING[api_name]
...
```

{{% /steps %}}