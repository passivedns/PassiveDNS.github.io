---
title: Database
weight: 1
prev: docs/architecture/
next: docs/architecture/docker
---

The database is a graph-oriented database from [ArangoDB](https://arangodb.com/).

It is initialized when creating an admin user when the app is started for the first time (in `/passiveDNS/db/database.py`)

## Database schema

![database schema](/screenshots/ArangoDB_PassiveDNS.drawio.png)

## Structure of collections

### Nodes

#### Users

```json
{
    "_key": "username",
    "email": "str",
    "hashed_password": "str",
    "role": "User|Admin|Scheduler",
    "api_keys": {
        "api_name":"str",
        "..."
    }
}

```

#### Domain Name

```json
{
    "_key": "domain",
    "records": ["..."],
    "registrar": "str",
    "created_at": "datetime",
}

```

#### Channel

```json
{
    "_key": "name",
    "type": "email|redis",
    "infos": {"..."},
}

```

#### IPAddress

```json
{
    "_key": "address",
    "location": {
        "country":"str",
        "country_code":"str",
        "region":"str",
        "region_name":"str",
        "city":"str",
        "zip_code":"str",
        "latitude":"float",
        "longitude":"float",
        "timezone":"str",
        "ISP":"str",
        "organization":"str",
        "AS":"str",
    },
}

```

#### Tag

```json
{
    "_key": "name",
}

```

#### UserRequest

```json
{
    "_key": "email",
    "requested-at": "datetime",
}

```

#### UserPending

```json
{
    "_key": "token",
    "email": "str",
    "invited_at": "datetime",
}

```

#### ApiIntegration

```json
{
    "_key": "name",
    "base_url": "str",
    "header": "str",
    "ip": {
        "method": "GET|POST",
        "uri": "str",
    },
    "domain": {
        "method": "GET|POST",
        "uri": "str",
    },
}

```

### Edges

#### UserChannel

```json
{
    "_from": "Users/username",
    "_to": "Channel/name",
    "username": "str",
    "channel_name": "str",
    "contact": "str",
    "verified": "str",
    "token": "str",
}

```

#### UserDn

```json
{
    "_from": "Users/username",
    "_to": "DomainName/domain",
    "username": "str",
    "domain_name": "str",
    "owned": "str",
}

```

#### DomainNameResolution

```json
{
    "_from": "DomainName/domain",
    "_to": "IPAddress/address",
    "domain_name": "str",
    "ip_address": "str",
    "resolver": "str",
    "last_updated_at": "datetime",
    "first_updated_at": "datetime",
}

```

#### TagDnIp

```json
{
    "_from": "Tag/name",
    "_to": "DomainName/domain|IPAddress/address",
    "tag": "str",
    "object": "str",
    "type": "DomainName|IPAddress",
}

```