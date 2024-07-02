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

## Create a new administrator

If there is a need for multiple administrators, a new administrator can be created using the shell inside the API container (see the [production installation](../../installation/production) for more details) using the following command:

```bash
../docker-entrypoint.sh create-user [USERNAME] [PASSWORD] [EMAIL] --admin
```
Replacing `[USERNAME]`, `[PASSWORD]` and `[EMAIL]` with the corresponding data.