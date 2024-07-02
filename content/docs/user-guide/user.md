---
title: User
weight: 1
prev: docs/user-guide/
next: docs/user-guide/scheduler
---

The user is the most classic role, that allows to access every functionality described on this page.

## New user

If you do not have a user account, you can request an account with 2 options :

{{% steps %}}

### Request using the web app
- On the login page, click "Request access" and enter your email address to send a link to the administrators
- The administrator have to review your request and accept it
- Using the token you recieved by email, you can then create your account using "Register"

### From an administrator
- Contact an Administrator so that they invite you using your email address
- Using the token you recieved by email, you can then create your account using "Register"

{{% /steps %}}


## User interface

Once a user is logged in, the home page looks like this :

![Home page of PassiveDNS](/screenshots/User-Home.png)

From there, a user can :
- View details of the existing domain names
- Add a new domain name
- Filter the domain names and export in json or csv

## Alerts

The alerts page allow you to see every alert created since the server was setup.
An alert is created when a change is detected during the resolution of a known domain name.
When an alert is created, all of the subscribed users recieve it from the selected channel.

## Channels

Channels are used to send alerts, either by email or using a Redis pub/sub channel. 
A user can see all of the channels that are available and subscribe to them, by giving an email address. 
They can then verify their email, and once that is done, they will recieve the new alerts.

[Redis](https://redis.io/docs/latest/develop/interact/pubsub/) channels do not have a setup button, as they do not need it. Please contact your administrator if you want to know how to get the alerts from there.

{{< callout type="info" >}}
  Please note that the `_default` channel is an email channel, and needs to be manually set up by the administrator in order to be used.
{{< /callout >}}

## Extern Apis

The Extern Apis page allows the user to connect to other apis using their API keys. Once a user has connected an api key, they can get the history of a domain name by clicking on the extern API button on the home page, and then clicking on the domain name.

![a domain name having the VirusTotal button visible](/screenshots/extern-api.png)

A user can also edit the existing api, in case of an api update for example. 

The current availables APIs are : [VirusTotal](https://www.virustotal.com) and [AlienVault](https://otx.alienvault.com/api)

## Preferences

By clicking the User icon on the far right of the window, a user can toggle light or dark mode, change their password or logout.