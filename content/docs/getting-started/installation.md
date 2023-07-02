---
weight: 1
bookFlatSection: true
title: "Installation"
---

# Getting Started

## Install Tankman

Download binaries for your platform
- [Linux x86-64](https://example.com)
- [Linux ARM64](https://example.com)
- [macOS M1](https://example.com)
- [Windows x86-64 (untested)](https://example.com)
- [Source Code](https://github.com/lesser-app/tankman)

## Docker

Alternatively you can pull our docker images.

## Setting up Tankman

Step 1: Create a new database for tankman on your PostgreSQL server. Call it whatever you want, but most people call it tankmandb.

Step 2: Initialize the database. You can do that with the following command:

```sh
./tankman --initdb --dbhost YOUR_PG_HOST --dbport YOUR_PG_PORT --dbuser YOUR_PG_USER --dbpass YOUR_PG_PASSWORD
```

That's it. You're ready to roll.

## Running Tankman

The following command will start tankman on localhost:1989

```sh
./tankman --dbhost YOUR_PG_HOST --dbport YOUR_PG_PORT --dbuser YOUR_PG_USER --dbpass YOUR_PG_PASSWORD
```

You can change the hostname and port with the `--host` and `--port` CLI options.

```sh
./tankman --host 127.0.0.1 --port 1990 --dbhost YOUR_PG_HOST --dbport YOUR_PG_PORT --dbuser YOUR_PG_USER --dbpass YOUR_PG_PASSWORD
```

## Configuring via Environment Variables

Instead of using CLI options as given above, you may use $TANKMAN_HOST (instead of `--host`), $TANKMAN_PORT (instead of `--port`) and $TANKMAN_CONN_STR (instead of `--dbhost`, `--dbport`, `--dbuser`, `--dbpass`) environment variables.

Like this:

```sh
TANKMAN_HOST=localhost
TANKMAN_PORT=1990
TANKMAN_CONN_STR=Server=localhost;Port=5432;Database=tankmandb;User Id=postgres;Password=postgres

# run tankman!
./tankman
```

## ⚠️ Caution 

Tankman is an internal service which should be accessible only from your backend apps. Make sure that you don't expose Tankman ports publicly accessible.
