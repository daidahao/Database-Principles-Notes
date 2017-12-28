# Lecture 12

- `INSERT`s are inserts
- many `UPDATE`s aren't updates they are inserts
- many `DELETE`s aren't deletes they are updates

## Performance

Strategies

- Parallelism
- Adding nodes
- Keeping scope constant
- Archiving

### Archiving

Partitions

> This is an area where partitioning by date can help a lot. You archive and drop the oldest partition, and creae a new one (beware of foreign keys!)

`truncate`

> Alternatively, you can use a fixed number of partitions in a circular way.

## User Management

In all products (other than SQLite, of course) you need to connect to a database a DB account which is different from (but can sometimes be tied to) an OS account.

Exception: IBM DB2

### Super-user account ("DBA account")

In all DBMS products, a default super-user account (often called "DBA account") is automatically created with the database.

### `SYSTEM`

- System privileges

Privileges to change the structure

- Table privileges

Privileges to access the data

### Three categories of users.

- Adminstrators

All system privileges

- Schema Owners

System privileges to create objects

- Regular Users

Minimum sysatem privileges + Object privileges

**Don't grant privileges "just in case"**

**JUST THE REQUIRED PRIVILEGES.**

`grants on present objects`

`grants on future objects`

### Developer Account

- `grant create table to ...`
- `grant create procedure to ...`
- `grant create view to ...`
- `grant create type to ...`

**default schema/database/tablespace**

You should also define (especially when inexperienced developers) where their tables will be created by default. You want to keep application tables separate from system tables.

### Role

As you don't want to repeat a long series of "GRANT" statements everytime a new hire must be given access to the database, you create a role, give all the privileges to the role (system and tables) and grant the role to users.

### Alias / Synonym

Normally you are supposed to prefix table names with schema names. Don't do that.

Use aliases (synonyms); they can be dropped and changed at will to point to different tables while running exactly the same queries.

`create synonym employees for hr.employees`

`create synonym employees for training.employees`

### `PUBLIC`

### Web Applications

DON'T CONNECT AS THE SCHEMA OWNER.

As a general rule, don't give full DBA privileges to many people. There should only be a handful of DBA accounts at most. Beware of third-party software that requires unneeded privileges.

## Big Management Issues

- Ethics are relative
- Not trusting people often makes people less trustable
- Cultural sensitivity
- Don't give unnecessary privileges
- Entrust the happy few with high privileges

Encryption

## SECURITY ISSUES

- Secure Server
- Secure Files and Directories

Even log files may contain a lot of information

- Check and remove unused defaults
- PASSWORDS

Same security standards for Production and Development.

- Try to keep track of connections

## BACKING UP A DATABASE

### Logical backup

- Beware of consistency
- SLOW recovery
- Some data loss

### Physical backup

COPY FILES

but Everything happens in memory!

#### Cold Backup

Shutting down the database before copying the files

- Consitstency: Good
- Data loss: Whatever happened between backup and restore is lost.
- Availability: Good

Hot Backups:

(Not supported by all products)

INCREMENTAL BACKUP

- Consitstency: Good
- Loss: No
- Availability: Good

In summary, Physical Backup

- Faster restart
- Can be incremental AND consistent
- No or little data loss
- Cannot recover ONE table
- More complicated (Host backup only)
- Big files

### Point-in-time Recovery

> Remember that if you drop one table, you cannot restore it from a physical backup. It's all or nothing.

RECYCLE BIN

PRACTICE RECOVERY

### High Avalaibility

A typical HA solution is a cluster of computers that may or not access the same database files.

#### Log Shipping

The idea is to periodically ship log files to a distant server that hosts a copy of the database and reapply them there.

With log shipping, you can reduce dramatically what can be lost to the content of logs that haven't yet been transferred.

#### Metrocluster

#### Cloud (for smaller companies)



























...
