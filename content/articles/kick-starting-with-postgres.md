+++
title = "Kick Starting with PostgreSQL"
date = 2026-04-28
authors = ["Hildeberto Mendonca",]
description = "The basics to start with PostgreSQL and create a database for an application."
+++

## Installation

Execute the following commands to install PostgreSQL:

    $ sudo apt-get install postgresql
    $ sudo su - postgres -c "createuser -s $USER"
    $ createdb

### Docker

    $ docker pull postgres:18
    $ docker volume create postgres_data
    $ docker run --name postgres_lab -e POSTGRES_PASSWORD=secret -d -p 5432:5432 -v postgres_data:/var/lib/postgresql postgres:18

## Database

### Create a Database

Then create the database for the application:

    $ sudo su - postgres
    $ psql
      postgres=# create database <database_name>;
      postgres=# create user <database_user> with encrypted password 'secret';
      postgres=# grant all privileges on database <database_name> to <database_user>;
      postgres=# \c <database_name> postgres
      digger=# grant all on schema public to <database_user>;
      digger=# \q
    $ exit

If you accidentally made a mistake while creating the user, just drop the user and create it again:

    $ dropuser <database__wrong_user>
    $ createuser <database_user> -P

If the database was already created and the username has changed, some tables may remain under the ownership of a previous user. If you get an error like "_permission denied for table [table_name]_" then it might be the case. Use the command `\dt` to list all the tables with their ownership. For each one of the impacted tables, use the `alter table` statement:

    $ psql -d <database_name>
      =# alter table table_name owner to <database_user>
      =# \q

To drop the database after use:

    $ dropdb <database_name>

PostgreSQL directories are the followings:

    /var/log/postgresql/
    /var/lib/postgresql/
    /etc/postgresql/