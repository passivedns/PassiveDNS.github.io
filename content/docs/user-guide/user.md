---
title: User
weight: 1
prev: docs/user-guide/
next: docs/user-guide/scheduler
---

The user is the most classic role, that allows to access every functionality described on this page.

## New user

If you do not have a user account, you need to ask the administrator to create one for you. They will then give you a username and a password that you can change in the settings once logged in.


## User interface

Once a user is logged in, the home page looks like this :

![Home page of PassiveDNS](/screenshots/User-Home.png)

From there, a user can :
- View details of the existing domain names
- Add a new domain name
- Import a list of domains using a file
- Filter the domain names and export in json or csv

## Domain names

If a domain name fails to be created, it may have been created by another user. Filtering the domain names with `no ownership filter` will display the domains that are not owned or followed by the user. It is then possible to follow them by clicking the heart shaped button.

## Import from file

It is possible to import a list of domain names from a file rather than adding them one by one.
The file must be `.csv` or `.txt`, and there must be one domain per line.
For example:
``` {filename="domains.csv"}
example.com
github.com
dns.google.com
```

## Alerts

The alerts page allow you to see every alert created since the server was setup.
An alert is created when a change is detected during the resolution of a known domain name.
When an alert is created, all of the subscribed users recieve it from the selected channel.

## Channels

Channels are used to send alerts using a [Redis](https://redis.io/docs/latest/develop/interact/pubsub/) pub/sub channel. 
A user can see all of the channels that are available and subscribe to them via redis directly. 
To subscribe, the user needs to connect to redis (using `redis-cli`), and start listening to the channel using :
```
subscribe [CHANNEL_NAME]
```
Replacing `[CHANNEL_NAME]` by the name displayed on the app.
Please contact your administrator if you want to know how to get the alerts from there.

{{< callout type="info" >}}
  Please note that the `_default` channel is not setup initially, and needs to be manually set up by the administrator in order to be used.
{{< /callout >}}

## Extern Apis

The Extern Apis page allows the user to connect to other apis using their API keys. Once a user has connected an api key, they can get the history of a domain name by clicking on the extern API button on the home page, and then clicking on the domain name.

![a domain name having the VirusTotal button visible](/screenshots/extern-api.png)

A user can also edit the existing api, in case of an api update for example. 

The current availables APIs are : [VirusTotal](https://www.virustotal.com) and [AlienVault](https://otx.alienvault.com/api)

## Preferences

By clicking the User icon on the far right of the window, a user can toggle light or dark mode, change their password or logout.