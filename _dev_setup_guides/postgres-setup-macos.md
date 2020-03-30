---
layout: resource
title: "Postgres MacOS Dev Setup"
slug: postgres-setup-macos
description: "PostgreSQL setup & default 'postgres' user configuration for MacOS"
order_number: 4
---

## Installing with `postgres` as the Default User

Most MacOS Postgres install guides will provide just two commands to be up and running (`brew install` then `brew services start`), but this common setup method has a flaw: the first, default superuser created on the Postgres instance will be `[your MacOS username]`.  This differs from a standard install on Linux environments, where the default username is `postgres`.

This minor difference can have its costs. Not only will default configurations differ between your local MacOS Postgres and your Linux servers or Docker containers, but most how-tos, Stack Overflow answers, and troubleshooting tips you come across will operate under the assumption that `postgres` is the original superuser name on any Postgres instance. Further, this first superuser is nearly impossible to remove later on as it owns the system tables essential to Postgres' inner workers.

Fortunately, we can avoid this all with a one-time detour from the common install approach.