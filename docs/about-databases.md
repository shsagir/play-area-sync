---

template:         article
naviTitle:        About databases
reviewed:         2016-02-25
title:            Using external databases with fortrabbit

section:          Extending_fortrabbit
groupList:        databases

tags:
     - advanced

keywords:
     - nosql
     - RDBMS
     - DBMS
     - SQL
     - relational database
     - DB2
     - Oracle
     - PostgreSQL
     - SQLite
     - SQL Server
     - Sybase
     - MongoDB

seeAlsoLinks:
     - elephantsql
     - redis-cloud

---

[MySQL](/mysql) is a core Component with fortrabbit. But it's not the only the database out there. NoSQL is a popular topic. There is MongoDB, Rdedis, Riak; just to name a few. You can use external services in combination with fortrabbit to utilize them with your App.

## Standard setup

To connect an external database service provider you usually need to:

### Choosing a data center location

Data latency is critical for databases. fortrabbit runs on AWS. Most service provider are there as well. We recommend to use a service hosted in the same data center region as your fortrabbit App.

### Enabling the PHP extension

Database drivers are usually implemented as a PHP extension. Speed and all that. We support very many and you can enable them in the fortrabbit Dashboard.

### Requesting a firewall rule

In some cases the database providers provides you with a non standard port, which is not on the fortrabbit default whitelist. In that case you can request a whitelisting in the fortrabbit Dashboard under the firewall settings of your App.