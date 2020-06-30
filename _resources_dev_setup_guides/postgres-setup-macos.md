---
layout: resource-post
title: "Postgres MacOS Dev Setup"
slug: postgres-macos
description: "PostgreSQL setup & default 'postgres' user configuration for MacOS"
date: 2020-03-29
author: Franco Posa
order_number: 6
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

Do not use `brew services start` yet! This is the command we want to avoid at first, as it does not allow us to set the initial superuser name for a new Postgres instance.

The [PostgreSQL wiki First Steps page](https://wiki.postgresql.org/wiki/First_steps) alludes to the solution: [the `initdb` command](https://www.postgresql.org/docs/9.5/app-initdb.html). `initdb` is the CLI for initializing a new Postgres instance, and it allows the user to set all instance parameters, including the one we care about here:

>"`-U username`
>`--username=username` selects the user name of the database superuser. This defaults to the name of the effective user running initdb.
>It is really not important what the superuser's name is, but one might choose to keep the customary name postgres, even if the operating system user's name is different."

The only other information that `initdb` needs is the directory to initialize the database cluster into, provided by the `-D/--pgdata` flag. For a Homebrew install, this should be `/usr/local/var/postgres`.

Put it all together to initialize with our preferred superuser:

```
% initdb -D /usr/local/var/postgres -U postgres
```

This replaces the initialization step that would normally be run (without the option to name the superuser) the first time you call `brew services start postgres`

See the [Troubleshooting](#troubleshooting) section for common issues that can arise at this step.

## 3. Start Postgres

You may start and stop Postgres with either the built-in [`pg_ctl` command](https://www.postgresql.org/docs/9.5/app-pg-ctl.html) or the [`brew services`](https://github.com/Homebrew/homebrew-services) process manager. The primary difference is that a Postgres process started with `pg_ctl` will die when you exit the terminal session, while `brew services` will keep Postgres running in the background. Keeping Postgres running is probably the desired behavior for most developers.

`pg_ctl` requires the `-D` argument to know where the Postgres instance is located:

```
% pg_ctl -D /usr/local/var/postgres start
...[startup logs]
```

If you receive an error from `pg_ctl`, Postgres is likely already running.

`brew services` does not require specifying the directory:

```
% brew services start postgres
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
```


## 4. Login to the Postgres CLI

[`Psql`](https://www.postgresql.org/docs/9.3/app-psql.html) is the Postgres interactive terminal, used to query interactively as well as manage the cluster with meta-commands.

By default, `psql` will attempt to log in with the currently-active username, so we have to provide the `-U/--username` flag to log in as `postgres`:

```
% psql -U postgres
psql (12.2)
Type "help" for help.

postgres=#
```

Now we can see that `postgres` is the only user on the instance:

```
postgres=# \du 
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```

As well as the owner of the existing databases:
```
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
```

## 5. Create Your First Database

Postgres allows hyphenated database names but hyphenated names don't work with the CLI's autocomplete, so delimiting with underscores will make your life a bit easier.

```
postgres=# CREATE DATABASE first_test;
CREATE DATABASE
```

Connect & try the tab-completion available on the database names:

```
postgres=# \c f[press tab here to autocomplete database name]
postgres=# \c first_test
You are now connected to database "first_test" as user "postgres".
first_test=#
```

## 6. Exit & Stop Postgres

From the `psql` command line, `\quit`, `\q`, and `Ctrl-D` will all work to exit the interactive session.

If you want to stop the Postgres instance altogether, use `brew services` - `pg_ctl` has a `stop` command but it doesn't always stop all the background processes that Postgres runs.

```
% brew services stop postgres
Stopping `postgresql`... (might take a while)
==> Successfully stopped `postgresql` (label: homebrew.mxcl.postgresql)
```

## Troubleshooting

I have sometimes found that the Homebrew Postgres installer will take it upon itself to initialize the cluster even if we don't call `brew services start`. If this is the case, you will receive an error like this from `initdb`:

```
initdb: error: directory "/usr/local/var/postgres" exists but is not empty
If you want to create a new database system, either remove or empty
the directory "/usr/local/var/postgres" or run initdb
with an argument other than "/usr/local/var/postgres".
```

If this happens, it's likely too late - Homebrew has initialized the instance with your MacOS username as the owner of everything. Confirm by listing the databases to see their owners:

```
% psql -d postgres -c "\l"

                                   List of databases
   Name    |    Owner    | Encoding | Collate | Ctype |        Access privileges        
-----------+-------------+----------+---------+-------+---------------------------------
 postgres  | franco.posa | UTF8     | C       | C     | 
 template0 | franco.posa | UTF8     | C       | C     | =c/"franco.posa"               +
           |             |          |         |       | "franco.posa"=CTc/"franco.posa"
 template1 | franco.posa | UTF8     | C       | C     | =c/"franco.posa"               +
           |             |          |         |       | "franco.posa"=CTc/"franco.posa"
```

Assuming this is truly a fresh Postgres install and we don't have any old data in `/usr/local/var/postgres` that we want to keep, this can be "fixed" by deleting the whole instance.

**THIS WILL DELETE ALL OF YOUR POSTGRES DATA**

```
% rm -rf /usr/local/var/postgres
```

Now you can jump back to [Step 2](#2--initialize-postgres-with-postgres-as-the-default-user).