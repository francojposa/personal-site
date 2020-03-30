---
layout: resource
title: "Postgres MacOS Dev Setup"
slug: postgres-setup-macos
description: "PostgreSQL setup & default 'postgres' user configuration for MacOS"
order_number: 4
---

## Why install with `postgres` as the Default User?

Most MacOS Postgres install guides will provide just two commands to be up and running (`brew install` then `brew services start`), but this common setup method has a flaw: the first, default superuser created on the Postgres instance will be `[your MacOS username]`.  This differs from a standard install on Linux environments, where the default username is `postgres`.

This minor difference can have its costs. Not only will default configurations differ between your local MacOS Postgres and your Linux servers or Docker containers, but most how-tos, Stack Overflow answers, and troubleshooting tips you come across will operate under the assumption that `postgres` is the original superuser name on any Postgres instance. Further, this first superuser is nearly impossible to remove later on as it owns the system tables essential to Postgres' inner workers.

Fortunately, we can avoid this all with a one-time detour from the common install approach.

## 1. Install Postgres with Homebrew

```
% brew install postgres
```

Upon a successful install, Homebrew will output the message below. Don't follow the instructions just yet!

```
To migrate existing data from a previous major version of PostgreSQL run:
  brew postgresql-upgrade-database

To have launchd start postgresql now and restart at login:
  brew services start postgresql
Or, if you don't want/need a background service you can just run:
  pg_ctl -D /usr/local/var/postgres start
```

## 2.  Initialize Postgres with `postgres` as the Default User

`brew services start` is the command we want to initially avoid, as it does not allow us to set the initial superuser name for a new Postgres instance.

The [PostgreSQL wiki First Steps page](https://wiki.postgresql.org/wiki/First_steps) alludes to the solution: [the `initdb` command](https://www.postgresql.org/docs/9.5/app-initdb.html). `initdb` is the CLI for initializing a new Postgres instance, and it allows the user to set all instance parameters, including the one we care about here:

>"_`-U username`
>`--username=username` selects the user name of the database superuser. This defaults to the name of the effective user running initdb.
>It is really not important what the superuser's name is, but one might choose to keep the customary name postgres, even if the operating system user's name is different."_

The only other information that `initdb` needs is the directory to initialize the database cluster into, provided by the `-D/--pgdata` flag. For a Homebrew install, this should be `usr/local/var/postgres`.

Put it all together to initialize with our preferred superuser:

```
% initdb -D /usr/local/var/postgres -U postgres
```