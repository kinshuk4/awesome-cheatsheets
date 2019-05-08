---
title: PostgreSQL Best Practices
category: Databases
---

### UUID

1) Usage of UUID data type vs text

Don't use text fields to keep UUIDs. It is very space inefficient. UUID type takes just 16 bytes, while text representation takes 37 bytes. It also proportionally increases index size.

2) Usage of UUIDs in tables for low number of different UUIDs

We see many tables containing columns containing UUIDs where in reality the number of different UUIDs is very low (e.g. warehouse IDs or Stock IDs). A better solution in these cases is to actually create another table and use it to map UUIDs to smallint. This significantly reduces index sizes across your other tables!

### Use partial indexes on columns with uneven distribution and low cardinality!
Use partial indexes on columns with uneven distribution and low cardinality!

```bash
create table t (id serial, state enum, msg jsonb, primary key(id));
create index on t (id) where state='in_progress';
create index on t (id) where state='open';
```

This saves about 90% of space and still allows you to get the next open row sorted by id efficiently.

(Just an example, not advertising queues in PG)

### JSON Columns
**Wide rows with large JSON(B) columns and updates**
Not as easy as the above, but keep in mind that due to how Postgres works updates of rows need to copy all the data to the new modified row.
Thus if you have rows where columns or parts of the data in a json object change, it may make sense to move the static data outside of the table, to reduce the amount of data copied.

