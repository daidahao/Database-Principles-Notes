# Database Principles Mid-term Exam Review

# Lecture 1

- No books required
- Avoid textbooks, use professional books

Grade | Percentage
-- | --
Midterm | 24%
Midterm | 25%
Lab & Quizzes | 20%
Final Exam | 25%
Project* | 30%

\* Groups of 3 or 4 for project

**Information** Data with a meaning

> Store data is mostly text, numbers, dates, and (less onen) mulfmedia documents.

### Purposes of Databases

### RETRIEVE data
as efficiently as possible.

## Relational Theory
1970

Edgar F. Codd 1923 - 2003

![](midterm/tables.png)

Each column in the table stores a piece of data, and one row represents a "known fact".

All the pieces of data in a row are related, hence **"relational"**.

### Operate on relations
Codd's big idea was that as relations are well known mathemafcal animals (sets), you could operate on them, and get new sets.

![](midterm/operations.png)

> If tables are variables, you can change their content massively, and combine them in complex operafons to obtain new results, as you could with numbers.

## Modeling

Before you can do fun operafons with tables, you must design your database well, a step known as modelling.

First of all, you must know which data you need/want to manage. It depends on whom you are.

### Film Database

Duplicates are forbidden in relafonal tables.

#### Key

What differentiates one row from another.

Key cannot be changed.

> For purely commercial reasons, though, it may be assumed that the combinafon ftle/country/year will be unique.

#### Primary Key

####  Normalizafon

**First Normal Form**

Each column should only contain ONE piece of informafon.

### Making the right choices

Most choice revolves along assessing whether an attribute is or can be considered **unambiguous** or whether even unusual cases are important and could break everything, in which case you may need to split data between two or even three tables.

### Entity

In a database model, you call "enfty" something that has a life ot its own. eg. people, films, countries.

Relafonships connect entities. They have no life of their own.

> If a film project is abandoned, there will be no rows for this film in CREDITS.

An "Enfty Relafonship Diagram" (E/R Diagram) is a way to represent tables in a database (you onen have hundreds of tables in a diagram). Crow feet indicate **"cardinality"**.

# Lecture 2

![](midterm/erdiagram.png)

> "Cardinality" is just a complicated word to say "number".

## Cardinality

### `(1, n)`

There is always one country per film, but there are several films per country.

### `(0, n)`

In some cases, some attributes haven't always a value; "main language" for instance, because you have silent films.

### `(m, b)`

Several actors usually appear in a film, and an actor usually plays in several films.

`(m,n)` cardinality implies a relationship table.

## Normalization
All about splitting

### 1 Simple Attributes

### 2 Attributes depend on the full key

### 3 Non-key attributes not dependent on each other

>  Every non key attribute must provide a fact about the key, the whole key, and nothing but the key. -- William Kent (1936 - 2005)

## Query Language

- ALPHA
- SQL (born SEQUEL, Don Chamberlin with Ray Boyce)
- QUEL (Michael Stonebraker)
- QBE (Query By Example, Moshe Zloof)

Oates, Ellison, Miner - The founders of Oracle

```sql
select ...
from ...
where ...
```

`SELECT` is followed by the names of the columns you want to return, `FROM` by the name of the tables that you query, and `WHERE` by filtering conditions.

## Two Main Components of SQL

### Data Definition Language
deals with tables

```sql
CREATE
ALTER
DROP
```

### Data Manipulation Language
deals with data

```sql
INSERT
UPDATE
DELETE
SELECT
```

> There is an official SQL standard that no product fully implements, and many subtly and someUmes irritatingly different dialects. We'll see the main variants of SQL.

- PostgreSQL
- ORACLE
- MySQL
- IBM DB2
- Microsoft SQL Server

## Key propriety of relations

### ALL ROWS ARE DISTINCT

- CAN BE enforced for tables in SQL

But you have to create your tables well.
- NOT enforced for query results in SQL

You have to be extra-careful if the result of a query is the starting point for another query, which happens often.

## Case Sensitivity

### Keywords

SQL keywords (words that have a special meaning in SQL) are NOT case sensitive and can be typed in any case you want.

### Identifiers
Same story with identifiers, the names you give to tables or, as you will soon see, columns, aren't case-sensitive.

Table (and column) names must start with a leder (PostgreSQL tolerates an underscore) and only contain letters, digits, or underscores.

> The $ sign is also accepted, and some products allow #.

> Note that names can sometimes be quoted between double quotes or square brackets, in which case spaces are allowed AND names become case-sensitive. Beder to avoid it.

```sql
create table table_name
  (column_name datatype,
                       ,
  )
```

## Data Types

- Text
- Number
- Date
- Binary

That's basically what you find in a database; nothing fancy.

### Text

`char(length)`
`char`

char() is for fixed-size columns (data is padded with spaces if shorter). Used for codes.

`varchar(max length)`
`varchar2(max length)`

varchars don't pad. They are limited in length (a few thousand bytes).

`text clob`

CLOB (called TEXT in MySQL) allows to store much bigger text (Gb).

### Number

`int`
`float`
`numeric(precision, scale)`
`number(precision, scale)`

### Date

`date`

includes time, down to second with Oracle, not with other products.

`datetime`

down to second (other than Oracle, except DB2)

`timestamp`

down to 0.000001 second

### Binary

`raw(max length)`

`varbinary(max length)`

RAW in Oracle, and VARBINARY (SQL Server) are the binary equivalent of VARCHAR.

`blob`

BLOB is the binary equivalent of CLOB (BLOB means Binary Large Object).

PostgreSQL calls the binary datatype BYTEA, don't ask me why.

## NULL

It indicates the absence of a value, because we don't yet
know it, or because in that case the adribute is irrelevant, or because we haven't the slightest idea about what this should be.

```sql
create table people ( peopleid int not null,
                      first_name varchar(30),
                      surname varchar(30) not null,
                      born numeric(4),
                      died numeric(4))
```

## Comments
```sql
comment on column people.surname is 'Surname or stage name';
-- comments in an SQL statement start with a double dash
```

## Constraints

```sql
create table people ( peopleid int not null
                                  primary key,
                      first_name varchar(30),
                check(first_name = upper(first_name)),
                      surname varchar(30) not null,
                check(surname = upper(surname)),
                      born numeric(4),
                      died numeric(4),
                      unique(first_name, surname))
-- MySQL accepts CHECK but doesn't enforce it.
```

### `PRIMARY KEY`
`PRIMARY KEY` indicates two things:

1. The value is mandatory (the additional NOT NULL doesn't hurt but is redundant).
2. The values are unique (no duplicates allowed in the column).

### `UNIQUE`

With many products data **is** case sensitive and different
capitalizaUon means different values that wouldn't violate a
uniqueness constraint. You **must** standardize case. eg. Oracle, PostgreSQL and DB2.

### Referential Integrity

```sql
create table movies (movieid    int not null primary key,
                     title      vercahr(60) not null,
                     country    char(2) not null,
                     year_released numeric(4) not null,
                check(year_released >= 1895),
                     unique (title, country, year_released),
                     foreign key(country)
                              references countryies(country_code))
```

A foreign key can be composed of a combinaUon of columns (rare).

Note that **the constraint works both ways**.

> We won't be able to delete a country if movies reference it, because we would get "orphaned rows" and the database would become inconsistent.

Creating tables requires:

- Proper modelling (cardinalities)
- Defining keys (what identifies rows)
- Determining correct data types
- Defining constraints

## `INSERT`

`INSERT` enters data into a table.

```sql
insert into table_name
  (list of columns)
values (list of values)
```

Values must match column-names one by one.

```sql
insert into countries
  (country_code, country_name, continent)
values('us', 'United States', 'AMERICA')
```

### Strings
For **strings**, **SINGLE quotes** is the standard and what works everywhere.

The standard SQL way to **escape a quote** is to double it.

### Date

There are only two ways to enter a date, **as a string** or **as the result of a function or computation**.

`to_date('07/20/1969' , 'MM/DD/YYYY')`

#### CURRENT DATE

`CURRENT_DATE` (no time) and `CURRENT_TIMESTAMP` (time included) are recognized by all products

## `SELECT`

`select * from movies` $\approx$ print table

> In a program, always name columns.

**Restriction**







...
