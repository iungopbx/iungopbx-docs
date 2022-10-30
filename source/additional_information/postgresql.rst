############
PostgreSQL
############


PostgreSQL is a enterprise grade open source database.
http://www.postgresql.org/

Backup
------

The following assumes the database username is iungopbx and the
database to backup is iungopbx. Make sure you have the database
password ready.

| ``su postgres``
| ``pg_dump -U iungopbx iungopbx -b -v -f /tmp/iungopbx.sql``

Restore
-------

Assuming username iungopbx and database iungopbx

``psql -U iungopbx -d iungopbx -f iungopbx.sql``

Console
-------

| ``su postgres``
| ``psql``

list the databases

``\l``

connect to the database

``\connect iungopbx``

or

``\c iungopbx``

list tables

``\d``

drop the database

``DROP DATABASE iungopbx;``

create the database

``CREATE DATABASE iungopbx;``

Links
-----

http://www.mkyong.com/database/backup-restore-database-in-postgresql-pg_dumppg_restore/

http://www.postgresql.org/docs/9.1/static/backup.html
