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

### UUID

1) Usage of UUID data type vs text

Don't use text fields to keep UUIDs. It is very space inefficient. UUID type takes just 16 bytes, while text representation takes 37 bytes. It also proportionally increases index size.

2) Usage of UUIDs in tables for low number of different UUIDs

We see many tables containing columns containing UUIDs where in reality the number of different UUIDs is very low (e.g. warehouse IDs or Stock IDs). A better solution in these cases is to actually create another table and use it to map UUIDs to smallint. This significantly reduces index sizes across your other tables!

## Partial Index
Use partial indexes on columns with uneven distribution and low cardinality!

```bash
create table t (id serial, state enum, msg jsonb, primary key(id));
create index on t (id) where state='in_progress';
create index on t (id) where state='open';
```

This saves about 90% of space and still allows you to get the next open row sorted by id efficiently.

(Just an example, not advertising queues in PG)

