---
layout: post
title: 'Installing postgresql with brew'
date: 2020-01-15
tags:
  - db
keywords:
  - Installing postgresql with brew
  - Installing psql with brew
  - brew postgresql
  - brew psql
  - how to install psql
  - how to install postgresql
---

This is copy from [https://gist.github.com/ibraheem4](https://gist.github.com/ibraheem4/ce5ccd3e4d7a65589ce84f2a3b7c23a3){:target="\_blank"}
Thank you @ibraheem4

<!--more-->

# Installing Postgres via Brew

## Pre-Reqs

[Brew Package Manager](http://brew.sh)

In your command-line run the following commands:

1. `brew doctor`
1. `brew update`

## Installing

1. In your command-line run the command: `brew install postgresql`
2. Run the command: `ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents`
3. Create two new aliases to start and stop your postgres server. They could look something like this:

   ```
   alias pg_start="launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist"
   alias pg_stop="launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist"
   ```

4. Run the alias you just created: `pg_start`. Use this comment to start your database service.
   - alternatively, `pg_stop` stops your database service.
5. Run the command: `` createdb `whoami` ``
6. Connect to your postgres with the command: `psql`
7. `brew reinstall readline` - **if needed**
8. `createuser -s postgres` - fixes `role "postgres" does not exist`
9. Test with `psql` command

   ```
   $ psql
   psql (10.0)
   Type "help" for help.

   ibraheem=#
   ```

## Details

### What is this `ln` command I ran in my Terminal?

_from the `man ln` command_

> The ln utility creates a new directory entry (linked file) which has the same modes as the original file. It is useful for maintaining multiple copies of a

     file in many places at once without using up storage for the ``copies''; instead, a link ``points'' to the original copy.  There are two types of links; hard
     links and symbolic links.  How a link ``points'' to a file is one of the differences between a hard and symbolic link.

### What is `launchctl`?

_from the `man launchctl` command_

> launchctl interfaces with launchd to manage and inspect daemons, angents and XPC services.

## Commands

### Create database

```
createdb <database_name>
```

> `createdb mydjangoproject_development`

### List databases

```
psql -U postgres -l
```

### Show tables in database

```
psql -U postgres -d <database_name>
```

> `psql -U postgres -d mydjangoproject_development`

### Drop database

```
dropdb <database_name>
```

> `dropdb mydjangoproject_development`

### Restart database

```
dropdb <database_name> && createdb <database_name>
```

> `dropdb mydjangoproject_development && createdb mydjangoproject_development`
