---
title: Scheduler
weight: 2
prev: docs/user-guide/user
next: docs/user-guide/administrator
---

The scheduler has the task of resolving the domain names every day and sending alerts if a change is detected.

## How to use

As an administrator, you can create a scheduler on the Users page, by clicking on the `Add scheduler user...` button.

If you want the scheduler to run, you need to update the `.env` file that is located in the `/pdns-docker/prod/`:

```env {filename=".env", linenos=table,linenostart=4}
...
SCHEDULER_USERNAME=[USERNAME]
SCHEDULER_PASSWORD=[PASSWORD]
...
```

Replacing `[USERNAME]` and `[PASSWORD]` with the corresponding data.

You then have to restart the scheduler container, and it will run correctly. 