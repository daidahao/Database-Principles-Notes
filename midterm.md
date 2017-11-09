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

## `NULL`

It indicates the absence of a value, because we don't yet
know it, or because in that case the adribute is irrelevant, or because we haven't the slightest idea about what this should be.

**For more on `NULL`, see Lecture 3.**

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

# Lecture 3

## `SELECT`

`select * from movies` $\approx$ print table

> In a program, always name columns.

**Restriction**


```sql
select ...
from ...
where ...
```

`SELECT` is followed by the names of the columns you want to return, `FROM` by the name of the tables that you query, and `WHERE` by filtering conditions.

```sql
select *
from movies
where country = 'us'
  and year_released between 1940 and 1949
```
`and` is "stronger" than `or`

- `=`
- `<>` or `!=`
- `<` `<=`
- `>` `>=`

Whenever you are comparing data of slightly different types, you should use functions that "cast" data types. It will avoid bad surprises.

Another frequent mistake is with datetime values.

```sql
where issued >= `<Monday’s date>`
  -- <Monday 00:00:00>
  and issued <= `<Friday's date>`
  -- <Friday 00:00:00>
```

### `IN()`

```sql
where country in ('us', 'gb')
  and year_released between 1940 and 1949
```

`country not in ('us', 'gb')`

### `LIKE`

- `%` any number of characters, including none

- `_` one and only one character

```sql
select * from movies
  where title not like '%A%'
    and title not like '%a%'
```

```sql
select * from movies
  where upper(title) not like '%A%'
```

> Not good to apply a function to a searched column.

### `NULL`
The only way to test `NULL`.

`where column_name is null`

`where column_name is not null`

### Concatenating Strings

`'hello' || ' world'` Most products

`'hello' + ' world'` SQL Server

`concat('hello', ' world')` MySQL

Avoid applying functions to columns that are used for comparison.

### `CASE .. END`
```sql
case upper(color)
  when 'Y' then 'Color'
  when 'N' then 'B&W'
  else '?'
end as color,
```

NULL cannot be tested in a WHEN branch.

## Useful Functions

### Numerical Functions

`round(3.141592, 3)` 3.142

`trunc(3,141592, 3)` 3.141

`floor()`
`ceiling()`

### String Functions

`upper`
`lower()`

`substr('Citizen Kane'), 5, 3` `'zen'`

`trim(' Oops ')` `'Oops'`

`replace('Sheep', 'ee', 'i')` `'Ship'`

`length()`

### Date Functions

![](midterm/datefunctions.png)

### `cast(__ as __)`

## `distinct`

```sql
select distinct country
from movies
```

## Aggregate Functions

aggregate function will aggregate all rows that share a feature and return a characteristic of each group of aggregated rows.

### `GROUP BY`

```sql
select country, year_released, count(*) number_of_movies
from movies
group by country, year_released
```

You can also group on several columns. Every column that isn't an aggregate function and appears after `SELECT` must also appear after `GROUP BY`.

- `count(*)` `count(col)`
- `min(col)`
- `max(col)`
- `avg(col)`
- `sum(col)`
- `stddev()`

> SQLite hasn't `stddev()`, which computes the standard deviaTon

```sql
select * from (
   select country,
          min(year_released) oldest_movie
   from movies
   group by country
   ) earliest_movies_per_country
where oldest_movie < 1940
```

### `HAVING`

```sql
select country,
        min(year_released) oldest_movie
from movies
group by country
having min(year_released) < 1940
```

`having country = 'us'` hurts performance.

`where country = 'us'` is way more efficient.

> When you apply a funcTon or operators to a null, with very few exceptions the result is null because the result of a transformation applied to something unknown is an unknown quantity.

**Aggregate functions ignore `NULL`s.**

Counting a mandatory column such as `BORN` will return the same value as `COUNT(*)`. The third count, though, will only return the number of dead people in the table.

```sql
select count(*) people_count,
       count(born) birth_year_count,
       count(died) death_year_count
from people
```

`select count(distinct colname)`

### How many people are both actors and directors?

```sql
select peopleid, count(*) as number_of_roles
from (select distinct peopleid, credited_as
    from credits
    where credited_as in ('A', 'D')
    ) all_actors_and_directors
group by peopleid
having count(*) = 2
```

# Lecture 4

## `JOIN`

```sql
select title, country_name, year_released
from movies
  join countries
    on country_code = country
where country_code <> 'us'
```

### `USING`

Not supported by SQL Server

```sql
select distinct first_name, surname
from people
  join credits
    using (peopleid)
where credited_as = 'D'
```

### Alias

```sql
select distinct p.first_name, p.surname
from people p
  join credits c
    on c.peopleid = p.peopleid
where credited_as = 'D'
```

### Self-join

### `INNER JOIN`

the regular join

### `OUTER JOIN`

Use `LEFT OUTER JOIN` only.

CONTRARY TO WHAT HAPPENS WITH THE ORDINARY (inner) JOIN, ORDER IS IMPORTANT.

### `COALESCE()`

takes an indeterminate number of parameters and returns the first one that isn't `NULL`, available with all products.

### Filter close to tables

With LEFT OUTER JOINs, apply all condiIons before joining.

#### British movie titles with director names when available?

> It's not necessary to have a subquery of British films for MOVIES, but it is necessary to have a subquery that only returns directors.


```sql
select m.year_released, m.title,
       p.first_name, p.surname
from (select movieid, year_released, title
      from movies
      where country = 'gb') m
left outer join (select movieid, peopleid
                 from credits
                 where credited_as = 'D') c
             on c.movieid = m.movieid
left outer join people p
on p.peopleid = c.peopleid
```

## Filtering and Qualifying

An outer join is always a qualifying join, unless it is associated with an IS `NULL` condiIon, meaning that not finding a match is significant.

If a join is removed, MORE rows?

- Yes -> Filtering
- No -> Qualifying

## Set Operations

### `UNION`

takes two result sets and combine them into a single result set.

- must return the same number of columns,
- the data types of corresponding columns must match.

### `UNION ALL`

### `INTERSECT`

![](midterm/intersect.png)

## `EXCEPT` / `MINUS`

![](midterm/except.png)

`intersect` -> `inner join`

`except` -> `outer join`

#### Find country codes that are both in movies and countries.

```sql
select distinct country
from movies
```

#### Find countries for which we haven't any film.
```sql
select country_code
from countries
except
select distinct country
from movies
```

```sql
select c.country_code
from countries c
      left outer join
          (select distinct country
           from movies) m
      on m.country = c.country_code
 where m.country is null
```

## SUBQUERY

### Correlation

```sql
select m.title, m.year_released,
        (select c.country_name
         from countries c
         where c.country_code = m.country)
            as country_name
from movies m
where m.country <> 'us'
```

> Strictly speaking, a subquery afer the SELECT is more equivalent to a LEFT OUTER JOIN.

```sql
select m.title,
       c.country_name
from movies m
     left outer join countries c
          on c.country_code = m.country
where m.country <> 'us'
```

**A subquery in the FROM cannot be correlated.**

`from` clause **uncorrelated**

`select` list **correlated**

`where` clause **uncorrelated** or **correlated**

### `IN()`

```sql
in (select col
    from ...
    where ...)
```

```sql
select country, year_released, title
from movies
where country in
       (select country_code
        from countries
        where continent = 'EUROPE')
```

Some products (Oracle, DB2, PostgreSQL with some twisting) even allow comparing a set of column values (the correct word is "tuple") to the result of a subquery.

```sql
(col1, col2) in
          (select col3, col4
           from t
           where ...)
```

`IN()` means an implicit DISTINCT in the subquery.

If **demonstrably unique**, no `distinct` wtih a `JOIN` (no need with a `IN()`).

![](midterm/uniquejoin.png)

# Lecture 5































...
