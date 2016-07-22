pg_rman
=======
pg_rman is an online backup and restore tool for PostgreSQL.

The goal of the pg_rman project is providing a method for
online backup and PITR as easy as pg_dump. Also, it maintains
a backup catalog per database cluster. Users can maintain old
backups including archive logs with one command.

Branches
--------
There are several branches on pg_rman in order to work with
different PostgreSQL server versions without introducing
server version check code blocks.
Please choose branch with PostgreSQL version you use.

* master : branch for latest PostgreSQL develop version [![Build Status](https://travis-ci.org/ossc-db/pg_rman.svg?branch=master)](https://travis-ci.org/ossc-db/pg_rman)
* REL9_5_STABLE : branch for PostgreSQL 9.5 [![Build Status](https://travis-ci.org/ossc-db/pg_rman.svg?branch=REL9_5_STABLE)](https://travis-ci.org/ossc-db/pg_rman)
* REL9_4_STABLE : branch for PostgreSQL 9.4 [![Build Status](https://travis-ci.org/ossc-db/pg_rman.svg?branch=REL9_4_STABLE)](https://travis-ci.org/ossc-db/pg_rman)
* REL9_3_STABLE : branch for PostgreSQL 9.3 [![Build Status](https://travis-ci.org/ossc-db/pg_rman.svg?branch=REL9_3_STABLE)](https://travis-ci.org/ossc-db/pg_rman)
* REL9_2_STABLE : branch for PostgreSQL 9.2 [![Build Status](https://travis-ci.org/ossc-db/pg_rman.svg?branch=REL9_2_STABLE)](https://travis-ci.org/ossc-db/pg_rman)
* pre-9.2 : branch for PostgreSQL 9.1 and before [![Build Status](https://travis-ci.org/ossc-db/pg_rman.svg?branch=pre-9.2)](https://travis-ci.org/ossc-db/pg_rman)

How to use
----------
pg_rman can take an online backup with one command.

````
$ pg_rman backup --backup-mode=full --with-serverlog
INFO: database backup start
NOTICE:  pg_stop_backup complete, all required WAL segments have been archived
````

The backup taken before are listed by show command:

````
$ pg_rman show
 ==========================================================
 StartTime           Mode  Duration    Size   TLI  Status
 ==========================================================
 2015-03-27 14:59:47  FULL        0m  3404kB     3  OK
 2015-03-27 14:59:19  ARCH        0m    26kB     3  OK
 2015-03-27 14:59:00  ARCH        0m    26kB     3  OK
 2015-03-27 14:58:46  FULL        0m  3516kB     3  OK
 2015-03-27 11:43:31  INCR        0m    54kB     1  OK
 2015-03-27 11:43:19  INCR        0m    69kB     1  OK
 2015-03-27 11:43:04  INCR        0m   151kB     1  OK
 2015-03-27 11:42:56  INCR        0m    96kB     1  OK
 2015-03-27 11:34:55  FULL        0m  5312kB     1  OK
````

The restore from backup also can be done from pg_rman.
The recovery.conf is generated by pg_rman.

````
$ pg_ctl stop -m immediate
$ pg_rman restore
$ cat $PGDATA/recovery.conf
# recovery.conf generated by pg_rman 1.2.11
restore_command = 'cp /home/postgres/arclog/%f %p'
recovery_target_timeline = '1'
$ pg_ctl start
````

See documentation about detail usage.

http://ossc-db.github.io/pg_rman/index.html



How to build and install from source code
-----------------------------------------
Change directory into top directory of pg_rman source codes and
run the below commands.


````
 $ make
 # make install
````

There are some libraries to be installed before.

* pam-devel(libpam-dev)
* readline-devel(libedit-dev)
* zlib-devel


How to run regression tests
---------------------------
Start PostgreSQL server and run the below command.

````
 $ make installcheck
````





