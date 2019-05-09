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
Add column:
```sql
alter table your_table add column geog geography;
```

Insert value:
```sql
insert into your_table (geog) values ('SRID=4326;POINT(longitude latitude)');
```

4326 is Spatial Reference ID that says it's data in degrees longitude and latitude, same as in GPS. More about it: http://epsg.io/4326

Order is Longitude, Latitude - so if you plot it as the map, it is (x, y).

To find closest point you need first to create spatial index:

```sql
create index on your_table using gist (geog);
```

and then request, say, 5 closest to a given point:

```sql
select * 
from your_table 
order by geog <-> 'SRID=4326;POINT(lon lat)' 
limit 5;
```

https://tapoueh.org/blog/2018/08/geolocation-with-postgresql/
https://github.com/daryllxd/lifelong-learning/blob/master/_cheat-sheets/postgres.md
https://stackoverflow.com/questions/8150721/which-data-type-for-latitude-and-longitude



### Scaling Postgres
https://www.slideshare.net/ChrisTravers5/postgresql-at-20tb-and-beyond

