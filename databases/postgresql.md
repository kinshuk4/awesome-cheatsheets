---
title: PostgreSQL
category: Databases
---

[PostgreSQL](http://www.postgresql.org/) is as widely used as [MySQL](http://dev.mysql.com/) and essentially those data storages are the most popular open source [relational database management systems](https://en.wikipedia.org/wiki/Relational_database_management_system). Along with many similarities with [MySQL](http://dev.mysql.com/), [PostgreSQL](http://www.postgresql.org/) traditionally is few steps ahead offering better extensibility and a lot of advanced features.

Replace anything within `<placeholder>` accordingly

### Install on Xenial

```bash
sudo add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc| sudo apt-key add -
sudo apt-get update
sudo apt-get install postgresql-9.6
```


### Console

```bash
$ psql #logs in to default database & default user
$ sudo -u <rolename:postgres> psql #logs in with a particular user
```

### Commands

 * Show roles: `\du`
 * Show tables: `\dt`
 * Show databases: `\l`
 * Show scemas: `\dn`
 * Show view in schema: `\dv schema.*`
 * Show tables in schema: `\dt schema.*`
 * Connect to a database: `\c <database>`
 * Show columns of a table: `\d <table>` or `\d+ <table>`
 * Quit: `\q`

### Creating database

```bash
 $ createdb databasename
```


### Geolocation Support
https://tapoueh.org/blog/2018/08/geolocation-with-postgresql/
https://github.com/daryllxd/lifelong-learning/blob/master/_cheat-sheets/postgres.md


### Scaling Postgres
https://www.slideshare.net/ChrisTravers5/postgresql-at-20tb-and-beyond

