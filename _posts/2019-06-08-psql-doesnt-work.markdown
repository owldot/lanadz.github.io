---
layout: post
title: 'psql: could not connect to server'
date: 2019-06-08
tags:
  - terminal
  - db
keywords:
  - psql could not connect to server
  - psql errors
---

I am using postgresql on MacOS. And quite often I see this error:

| psql: could not connect to server: No such file or directory. Is the server running locally and accepting connections on Unix domain socket "/tmp/.s.PGSQL.5432"?

<!--more-->

So, most of the time helps the following:

```
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
```

or if that doesn't help

```
$ brew services stop postgresql
$ rm /usr/local/var/postgres/postmaster.pid # adjust path accordingly to your install
$ brew services start postgresql
```

If that doesn't help I used

```
brew postgresql-upgrade-database
```

Or sometimes reinstall gem will help

```
$ gem uninstall pg
$ cd rails-app/
$ bundle install
```

Finally, uninstall postgres and install it again. **Mind it will erase your data!**

```
brew install postgres
initdb /Users/<user>/db -E utf8
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
```

### Another solution to this problem.

The primary issue here is that the initdb step of installation will create the directory with root ownership instead of as the user on a Mac. To solve this issue:

Create the data directory before running initdb and set permissions of 0700

```
rm -rf /usr/local/var/postgres  # in case this is not your first try
mkdir /usr/local/var/postgres
chmod 0700 /usr/local/var/postgres
```

Then run `initdb` and it will respect the permissions of the data directory.

```
initdb -D /usr/local/var/postgres
```

Login to test:

```
psql
```
