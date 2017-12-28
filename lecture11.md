# Lecture 11

## Distributed Transactions

One big problem is with transactions that **involve several servers**. Remember that transactions are meant to be atomic operations (a principle challenged by NoSQL databases).

### TWO-PHASE COMMIT

- JUST before committing, you ask to all servers involved whether they are ready to commit. If you don't get an all clear, you can rollback.
- Otherwise, you can then (2nd phase) send the official "commit" signal.

#### IN-DOUBT transactions

> You can still have cases when you don't know if one of the servers did or didn't commit in the end, and you end up with "in-doubt transactions" that you have to resolve (which means cancel) manually. It happens far more often than you may think, simply because the number of transactions in a big company is enormous, and a tiny percentage still means a few cases every week.

#### Latency

1 KM = 0.000005 s

#### Synchronous or Asynchronous

## Programming with Databases

a PHP example

## Object Relational Mapper

### django

> As soon as things become just a little bit complicated, it all goes downhill. You end up with a complex SPECIFIC syntax even for simple queries.

### Hibernate

Escape route: HQL

In reality, among all the people who dream of database independence most of them won't migrate their applications to another DBMS. If they migrate, they'll also rewrite everything.

If you want to minimise DBMS impact, it's far better to isolate YOUR SQL code than rely on something that is generic and often inefficient.

- Isolate SQL code
- Use DBMS features

## DBAs

### Production DBA

Production DBAs have a LOT of databases to care about. There are usually many independent databases in a company, and each production database is shadowed by multiple databases.

- Performance
- Physical Storage
- User Management
- Backup / Recovery

### Development DBA







...
