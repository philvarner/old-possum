# Postgresql

Postgresql notes

Basics

http://reinout.vanrees.org/weblog/2012/06/04/djangocon-postgres.html
Linux+PostgreSQL: http://www.yolinux.com/TUTORIALS/LinuxTutorialPostgreSQL.html
Intro to PG9 by Simon Riggs: http://www.packtpub.com/article/introduction-to-postgresql-9
Installing using Yum: https://wiki.postgresql.org/wiki/YUM_Installation
PostgreSQL 9 Admin Cookbook by Simon Riggs and Hannu Krosing: http://www.packtpub.com/postgresql-9-admin-cookbook/book

http://rollerweblogger.org/roller/entry/postgresql_startup_items_for_macos
http://stackoverflow.com/questions/2017004/setting-shmmax-etc-values-on-mac-os-x-10-6-for-postgresql
MacBook-Air-Master-Image:9.0 root# sudo sysctl -w kern.sysv.shmmax=134217728
kern.sysv.shmmax: 12582912 -> 134217728
MacBook-Air-Master-Image:9.0 root# sudo sysctl -w kern.sysv.shmall=134217728

OS X  - everything installed in /Library/PostgreSQL/9.0/
you must be root to see most of these files, so enable the root account

Data directory - ../data
Multiple tablespaces may be defined within this directory

Unix - PGLIB=/usr/lib/pgsql


data directory 

PGDATA

OS X 

both: /Library/PostgreSQL/9.0/data

RedHat 

both: /var/lib/pgsql/data/

Debian

data files: /var/lib/postgresql/R.r/main
configuration: /etc/postgresql/R.rNn/main/

Subdirectory      Purpose
base      Main data directory. Beneath this directory each database has its own directory within which are the files for each database table or index.
global      Database server catalog tables that are shared across all databases.
pg_clog      Transaction status files.
pg_multixact      Row-level lock status files
pg_subtrans      Subtransaction status files
pg_tblspc      Links to external tablespaces
pg_twophase      "2-phase commit", or Prepared transaction status
pg_xlog      Transaction log (or Write Ahead Log - WAL)

psql Command

psql -h localhost -U postgres dbname 

-c "SELECT * FROM jiveuser"
-f filename 


To change the database to run against: \c migtest
To quit: \q
To list the databases: \l
To list tables in db: \d
Describe table: \d table-name
metacommand help: \?
sql help: \h
expanded display (list instead of table): \x
Execution timing by using the \timing command

PGHOST or PGHOSTADDR
PGPORT (or set to 5432 if this is not set)
PGDATABASE
PGUSER
PGPASSWORD (though this one is definitely not recommended)

Change password - \password
or ALTER USER postgres PASSWORD ' md53175bce1d3201d16594cebf9d7eb3f9d';

~/.pgpass 
host:port:dbname:user:password (either values or *)
chmod 0600 ~/.pgpass

service postgresql-9.0 start|stop|restart|reload

Config

what's the diff between postgresql.conf and pg_hba.conf?

Allow network connections 
postgresql.conf: listen_addresses = '*'
pg_hba.conf: 
# TYPE DATABASE USER CIDR-ADDRESS METHOD
host all all 0.0.0.0/0 md5

Reload changes with

RedHat: service postgresql-9.0 reload
OS X:  SystemStarter restart PostgreSQL
The old-fashioned way: pg_ctl -D data reload

UBUNTU/DEBIAN pg_ctlcluster 9.0 main reload
RED HAT/FEDORA pg_ctl -D /var/lib/pgsql/data start
SOLARIS pg_ctl -D /var/lib/pgsql/data start
MAC OS pg_ctl -D /var/lib/pgsql/data start

RED HAT/FEDORA service postgresql start

Shutdown

-m fast : shutdown as fast as possible
 -m immediate : essentially crash the database

Reload config

UBUNTU/DEBIAN pg_ctlcluster 9.0 main reload
RED HAT/FEDORA service postgresql reload pg_ctl -D /var/lib/pgsql/data reload
SOLARIS pg_ctl -D /var/lib/pgsql/data reload
MAC OS pg_ctl -D /var/lib/pgsql/data reload

select pg_reload_conf();

See what the current settings are:

./pg_controldata <data dir>


Disconnecting users
SELECT count(pg_terminate_backend(procpid))
FROM pg_stat_activity
WHERE usename NOT IN
(SELECT usename
FROM pg_user
WHERE usesuper);

Running 

SET work_mem = '16MB';

will change the value.  RESET ALL will reset to configured values in .conf files

SHOW setting_name;

Settings
shared_buffers - how much memory…?

On Linux/Mac OS/FreeBSD, you will need to either edit the /etc/sysctl.conf file or use sysctl -w with the following values:
Linux: kernel.shmmax=value
Mac OS: kern.sysv.shmmax=value

effective_cache_size is not important
make wal_buffers and checkpoint_segments large if doing heavy writes
work_mem high if many large queries

fsync - don't touch, maybe no performance benefit.


Db Mgmt

CREATE DATABASE my_db;
 created my_db;

List all database's config
select * from pg_database;

Monitoring

pgAdmin : Tools : Server Status

check tables pg_stat_user_tables and pg_stat_user_indexes

Who is connected? SELECT * FROM pg_stat_activity;

What queries are being run?
SET track_activities = on

SELECT datname,usename,current_query
FROM pg_stat_activity
WHERE current_query != '<IDLE>' ;

Track all statements run: 
http://www.postgresql.org/docs/9.0/interactive/pgstatstatements.html

Run-time stats: 
http://www.postgresql.org/docs/9.0/interactive/runtime-config-statistics.html

Queries waiting on other queries: 
SELECT datname,usename,current_query
FROM pg_stat_activity
WHERE waiting;

and which query is blocking them

SELECT
w.current_query as waiting_query,
w.procpid as w_pid,
w.usename as w_user,
l.current_query as locking_query,
l.procpid as l_pid,
l.usename as l_user,
t.schemaname || '.' || t.relname as tablename
from pg_stat_activity w
join pg_locks l1 on w.procpid = l1.pid and not l1.granted
join pg_locks l2 on l1.relation = l2.relation and l2.granted
join pg_stat_activity l on l2.pid = l.procpid
join pg_stat_user_tables t on l1.relation = t.relid
where w.waiting;

To kill a process: pg_cancel_backend(processid) or  pg_terminate_backend(processid)
very useful if the db forgets to release 

select pg_terminate_backend(procpid)
from pg_stat_activity
where current_query = '<IDLE> in transaction'
and current_timestamp - query_start > '10 min';





Logs

Debian - /var/log/postgresql
RedHat - /var/lib/pgsql/data/pg_log
OS X - /Library/PostgreSQL/9.0/data/pg_log

log_min_messages
log_error_verbosity

Todo: how to change logging
Running shell commands to repeatedly query the db and dump to file

Log statements which take longer than a certain amount of time to execute
log_min_duration_statement = 10000;
log_duration = on

Slow queries -- too slow to be usable (looking up a user's CRM info takes minutes) or query that is run frequently that takes seconds instead of ms
table pg_stat_activity has actively running queries

select now()-query_start as running_for, current_query from pg_stat_activity order by 1 desc limit 5;


pg_stat_user_tables and pg_statio_user_tables
In the pg_stat_user_tables, fast growth of seq_tup_read means that there are lots of sequential scans occurring. The ratio of seq_tup_read/seq_scan shows how many tuples each seqscan reads.

In the pg_statio_user_tables, watch the fields heap_blks_hit and idx_blks_read, which give you a fairly good idea on how much of your data is found in PostgreSQL shared buffers (heap_blks_hit), and how much had to be fetched from disk (idx_blks_read). If you continuously see large numbers of blocks being read from disk, you may want to tune those queries, or if you determine that the disk reads were justified, you can make the configured shared_buffers value bigger.

If you have queries that you suspect are slowing you down, change log_min_duration_statement to a low-ish number, then lock that table to capture the queries in the slow query log

mydb=# begin;
mydb=# lock table mysuspecttable;
mydb=# select pg_sleep(12);
mydb=# rollback;

if PREPARE/EXECUTE is used, change log_line_prefix so that it includes either process ID ('%p') or session ID ('%c') to match up the two statements

http://pgfoundry.org/projects/pgstatspack

Prefix any query with EXPLAIN ANALYZE

explain analyse select count(*) from t;

Data doesn't fit in memory, so consistently re-read: heap_blks_read, idx_blks_read, toast_blks_read in the pg_stat*

PG does a pretty good job with MVCC of now locking poorly, but if there is lock contention
select * from pg_locks where not granted;

If you have a high load average with still lots of CPU idle left, you are probably out of disk bandwidth. In this case, you should also have lots of postgres processes in status D.

use WITH to define a view inline

BEGIN;
CREATE TEMPORARY TABLE nlqp_temp ON COMMIT DROP
AS

COMMIT;

or use materialized views as long-lived temp tables

default_statistics_target defines how much stat info is collected.

field by field basis:
alter table mytable alter col_with_bad_stats set statistics 500;

Add a multi-column index if a query selects on a and sorts on b
CREATE INDEX t1_a_b_ndx ON t1(a,b);

CLUSTER t1_a_b_ndx ON t1
rewrites the table in index order

table partitioning http://www.postgresql.org/docs/9.0/interactive/ddl-partitioning.html

if rows are updated but index is left unchanged, HOT updates are faster by Order of mag.  set fillfactor < 100

force pg to use indexes w/ set enable_seqscan to false;
but this is usually wrong

Optmistic locking



Backup & Restore
pg_restore -h pdxpsdb01 -d qliktech -U postgres qlicktech.dmp 
pg_dump -Upostgres -F c -b -v -f qlicktech.dmp qliktech-1.0-SNAPSHOT
pg_dump -Upostgres -Fc -O -x -d <db_name> <filename.dmp>


Syntax
-- single line comment
/* */ -- multiline


Terminology
database server - the running binary
data directory - the directory containing all of the data
tablespace - the separate files that store the data -- comes with a default one
database - a collection of schemas, etc, with tables

In some RDBMS (e.g., SQLServer) the term "database" is more similar to a "schema" in PostgreSQL.

Conventions

{tablename}_{columnname(s)}_{suffix}

where the suffix is one of the following:

pkey for a Primary Key constraint

key for a Unique constraint

excl for an Exclusion constraint

idx for any other kind of index

seq for all sequences 




Other

DB size: SELECT pg_database_size(current_database());
SELECT sum(pg_database_size(datname)) from pg_database;

Quoted table names are case-sensitive, otherwise not

Dupe rows: SELECT * FROM cust WHERE customerid IN (SELECT customerid FROM cust GROUP BY customerid HAVING count(*) > 1);
And deleting them: BEGIN; LOCK TABLE new_cust IN ROW EXCLUSIVE MODE; DELETE FROM new_cust WHERE ctid NOT IN (SELECT min(ctid) FROM new_cust 
WHERE customer_id IN (4) --specify exact duplicate ids 
GROUP BY customerid); COMMIT; 

Generating data 
generate_series 

Random int (random()*(2*10^9))::integer 
Random bigint (random()*(9*10^18))::bigint 
Random numeric data (random()*100.)::numeric(4,2); 
Random length string, up to a maximum length repeat('1',(random()*40)::integer) 
Random length substring substr('abcdefghijklmnopqrstuvwxyz',1, (random()*26)::integer) 
Random string from a list of strings (ARRAY['one','two','three'])[1+random()*3] 

Reference:

Manuals: http://www.postgresql.org/docs/manuals/
Command reference: http://www.postgresql.org/docs/9.0/interactive/reference.html 

pgtune http://pgfoundry.org/projects/pgtune/ 

pgfincore http://pgfoundry.org/projects/pgfincore/ 
http://www.pgcon.org/2010/schedule/attachments/148_pgfincore_pgcon10.pdf

Processes 

postmaster is main one 


ON_ERROR_STOP=on -- stop on first error in a script 



