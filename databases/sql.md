# SQL

## Resources

* [Joe Celko's books](https://www.goodreads.com/author/list/93496.Joe_Celko) esp. *SQL for Smarties: Advanced SQL Programming* and *SQL Puzzles and Answers* 
* [SQL Cookbook](https://learning.oreilly.com/library/view/sql-cookbook-2nd/9781492077435/) by Anthony Molinaro and Robert de Graaf (O'Reilly)
* [Learning SQL](https://learning.oreilly.com/library/view/learning-sql-3rd/9781492057604/) by Alan Beaulieu (O'Reilly) -- MySQL-based
* [Practical SQL](https://nostarch.com/practicalSQL) by Anthony DeBarros (No Starch Press)
* [SQL for Data Analysis](https://learning.oreilly.com/library/view/sql-for-data/9781492088776/) by Cathy Tanimura (O'Reilly)
* [O'Reilly SQL Pocket Guide 4th Edition (early)](https://learning.oreilly.com/library/view/sql-pocket-guide/9781492090397/) by Alice Zhao (O'Reilly)
* [O'Reilly SQL Pocket Guide 3th Edition](https://learning.oreilly.com/library/view/sql-pocket-guide/9781449397586/) by Jonathan Gennick
* [Effective SQL: 61 Specific Ways to Write Better SQL](https://learning.oreilly.com/library/view/effective-sql-61/9780134579078/) by Viescas, Steele, and Clothier
* [Hyperpolyglot -- Relational Data Tools: SQL, Awk, Pig](https://hyperpolyglot.org/data)

## Terminology

* You open a **session** with a database
* You start and later commit or rollback a **transaction** 

## Table Definition

Temporary tables for intermediate calculations

NOT NULL by default with default value if applicable
CONSTRAINT x CHECK BETWEEN 0 AND 100
and, can check contraints in this and other tables -- todo: find some examples of this
UNIQUE
PRIMARY KEY
REFERENCES table2(col1) -- another table
Referential actions -- ON UPDATE, ON DELETE -- CASCADE, SET NULL, SET DEFAULT, NO ACTION


MySQL - AUTO_INCREMENT
PostgreSQL - `SERIAL PRIMARY KEY` or `BIGSERIAL`

* Numeric: INTEGER, BIGINT, SMALLINT, DECIMAL(p,s), REAL, DOUBLE PRECISION

* TRUNC - towards zero
* ROUND - nearest integer
* FLOOR - down, even for negative values
* COALESCE - first non-null
* ABS, MOD, LEAST, GREATEST

String

* LOWER, UPPER
* SUBSTRING(s FROM i FOR len) 
* TRIM

## Table Operations

* DELETE FROM table_name
* SELECT cols FROM table_name
* INSERT INTO table_name (c1, c2, ...) VALUES (v1, v2, ...)
* UPDATE table_name SET (c1, c2, ...) = (v1, v2, ..)

## Query

* `=` not `==`
* comments with `--` or `/* /*`
* strings -- 'single quotes', not "double quotes"

Sweaty feet will give horrible odors

SELECT 
FROM  
WHERE 
GROUP BY 
HAVING 
ORDER BY

computed in order

FROM  
WHERE 
GROUP BY 
HAVING 
SELECT 
ORDER BY

## JSON

* PostgreSQL JSON [types](https://www.postgresql.org/docs/current/datatype-json.html) and [functions](https://www.postgresql.org/docs/current/functions-json.html)
* [MySQL - The JSON Data Type](https://dev.mysql.com/doc/refman/8.0/en/json.html)
  
## Regexes

Not standard, implementation specific.

* [SQL Pocket Guide - Regular Expressions](https://learning.oreilly.com/library/view/sql-pocket-guide/9781449397586/ch01s45.html)
* [Oracle Regex Pocket Reference](https://learning.oreilly.com/library/view/oracle-regular-expressions/0596006012/)

### MySQL

https://dev.mysql.com/doc/refman/8.0/en/pattern-matching.html

```
s REGEX p
```

* case-insensitive by default
* `\` is escape char, so `\\` for a literal `\` 
* character classes - [:alnum:], [:alpha:], [:blank:], [:cntrl:], [:digit:], [:graph:], [:lower:], [:print:], [:punct:], [:space:], [:upper:], [:xdigit:], [[:<:]] (beginnning of word), [[:<:]] (end of word)


### PostgreSQL

[PostgreSQL Pattern Matching](https://www.postgresql.org/docs/current/functions-matching.html)

`LIKE` and `NOT LIKE` -- `%` matches 0 or more, `_` matches single
ILIKE -- insensitive

ESCAPE '@' -- alternate escape char if string contains `\`

```
s SIMILAR TO p`
```

supports POSIX

\1 backref


## Datetime functions

INTERVAL - duration
`timestamp with time zone` or `timestamptz`

### PostgreSQL
[types](https://www.postgresql.org/docs/current/datatype-datetime.html) and [functions](https://www.postgresql.org/docs/current/functions-datetime.html)


PostgreSQL TO_TIMESTAMP(epoch_seconds)
PostgreSQL SELECT CURRENT_DATE;

### MySQL

[types](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) and [functions](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html)

MySQL SELECT CURDATE();

## Query Operation: JOIN
 
In Postgres, the default JOIN is INNER JOIN, so inner is used if only the term JOIN is specified.
 
> CROSS JOIN has the same behavior as a inner join ON TRUE:
 
> SELECT * FROM user INNER JOIN thread ON TRUE  
 
Instead of ON TRUE, we typically have a more restrictive predicate.  For example, the Jive user properties table has properties for every user in the system.  To select a result set that matches which users have which properties, we can select based on the userid column in each row having the same value:
 
> SELECT * from user JOIN userprop ON user.userid = userprop.userid  
 
An alternate syntax of USING can be used when the column names that are matched are the same in both tables.  The argument to USING is a comma-separated list of column names that are the same in both tables.
 
> SELECT * from user JOIN userprop USING (userid);  
 
One key property of the inner join is that rows from the right side table will only appear in the results if their is a matching entry in the left table.  This makes sense if you think of it like a CROSS JOIN is performed, and then rows that don't match the predicate are deleted -- none of the rows are going to match the predicate. 
 
Outer joins do one more thing -- they act like there's a row in each table that has every column value as null and don't apply the ON predicate to any result row that contains that null row. This means that even if there's no matching value for the ON predicate in the other table, you'll still get a row for it. 
 
The default OUTER JOIN in Postgres is a LEFT OUTER JOIN, meaning that the null row is on the first table specified.  You can also do a RIGHT OUTER JOIN (which is equivalent to a LEFT OUTER JOIN with the table order reversed) or a FULL OUTER JOIN, where both tables get the null row.  LEFT is the most commonly done, but RIGHT is useful when the right "table" is a result set from another select.
 
The most common use of outer join is to find rows in one table that don't have some matching entries in another.  For example, we may want to find all of the userprops entries who no longer have a user associated with them (e.g., they've orphaned).  We can do this with:
 
> SELECT * FROM userprop LEFT OUTER JOIN user USING (userid) WHERE user.userid IS NULL  
 
We could also use a postgres trick and set the predicate as "user IS NULL", which ensures that all of the result set's row values from that table are null.
 
> SELECT * FROM userprop LEFT OUTER JOIN user USING (userid) WHERE user IS NULL  
 
Another (quasi-dangerous) join is the NATURAL join.  This is equivalent to using the other JOINs with a USING clause that contains all of the columns with the same name.  This query would do the same as our previous one that included USING(userid), since that's the only column with the same name.
 
> SELECT * FROM user NATURAL LEFT JOIN userprop  
 
The danger here is that the column names change, and suddenly you start getting different results.  On Postgres, I found that I had to explicitly say NATURAL LEFT JOIN instead of NATURAL JOIN or NATURAL INNER JOIN, even though they should be the same.

A pretty useless variation of JOIN is the CROSS JOIN.  This takes every row in each table and creates a result row of them.
 
```
SELECT * 
FROM users CROSS JOIN threads
ON user.id = threads.userid
```
 
## Query Operation: GROUP BY

only terms in GROUP BY or other aggregates can appear in SELECT

The GROUP BY clause defines a partition of the result set.  The effect of this is that the result of the query is not a collection of rows, but rather a collection of groups of rows.  Because of this, you can only select values that are either common to all rows in each group (e.g., a group predicate) or are aggregations over the values in the rows of group.  For example, if we wrote the query:
 
> SELECT parentobjectid, count(*) FROM comment GROUP BY parentobjectid;  
 
We can use parentobjectid because it is a partition of result set and count(*) because it is an aggregate over the elements within a group.  We cannot use a value like commentid because each row within a group will have a different value. Note here that the count (as with all aggregate functions) is over the elements within each group, rather than the entire result set. 
 
In more formal language, GROUP BY defines a partition of the set of returned elements such that union of all the sets is the original set and the intersection is empty.  This means that every matching row is in exactly one group.
 
If a column used in the group by partition is nullable, all null rows are are typically put into one group.  To create a result set where each null is in its own group, we can create one partitioned result set that excludes rows with the null values and union it with another unpartitioned result set that includes the null rows.
 
> SELECT foo FROM table1  
>     WHERE foo IS NOT NULL  
>     GROUP BY foo   
> UNION ALL   
> SELECT foo FROM table1   
>     WHERE foo IS NULL;  
 
The aggregation functions provide a useful way of aggregating the column values within a group.  count is the most well-known of these. 
 
A useful aggregation function is string_agg, which you can use to concatenate all of the values in a column into a single result set row. For example:
 
> SELECT parentobjectid, count(*), string_agg(''||commentid,',') FROM comment GROUP BY parentobjectid;  
 
The format is string_agg(expression, delimiter)
 
Other commonly used aggregation functions are avg, max, min, and sum.
 
You can also add DISTINCT to the function within the aggregation function, for example:
 
> COUNT (DISTINCT *)
> COUNT (DISTINCT foo)
> COUNT (DISTINCT foo || bar)
 
## Query Operation: HAVING
 
The simple difference between WHERE and HAVING is that WHERE operates on individual rows, whereas HAVING operates at the group level. This means that any column that can appear in the SELECT of a query with a GROUP BY can appear in the HAVING predicate.  HAVING only operates on grouping columns and aggregate functions applied to the group.
 
A simple illustration of this is selecting every comment that has a commentid > 0 (which they all should)
 
> SELECT commentid FROM comment HAVING commentid > 0;  
 
fails with:
 
> ERROR:  column "comment.commentid" must appear in the GROUP BY clause or be used in an aggregate function  
 
This is because the group of the result set is everything, and every row doesn't have the same commentid (or, more precisely, we have not defined a group by predicate that ensures that every row has the same commentid, regardless of whether they happened to or not).
 
If we define a group partition on commentid (which will effectively create one group for each row in the table), we can then use a HAVING clause with it:
 
> SELECT commentid FROM comment GROUP BY commentid HAVING commentid > 0;  
 
An interesting result -- a WHERE clause predicate on a column with unique values for each row is the same as a group by on that column and having with the same predicate!
 
When using HAVING, one thing to watch out for is that results returned by the WHERE predicate include all the groups you want to operate on.  For example, if you want to know all of a certain group partition that includes no results, the groups for those results won't be created because they don't exist.
 
A more useful query is "find me how many docs have more than 10 comments".
 
> SELECT parentobjectid FROM comment GROUP BY parentobjectid HAVING count(*) > 10;  
 
The HAVING clause matches against the the aggregation on each group partitioned by the GROUP BY, rather than the individual rows as WHERE operates on.

## Set Operations

* UNION -- union w/ unique rows (set semantics)
* UNION ALL -- union w/ duplicate row preservation (bag semantics)
* INTERSECT -- intersection
* INTERSECT ALL -- intersection with duplicate row preservation
* EXCEPT (difference, subtraction)
* EXCEPT ALL  - a row that has m duplicates in the left table and n duplicates in the right table will appear max(m-n,0) times in the result set.


## Views

## indexes

## Notes

HAVING is like WHERE but with agg functions

Window functions

SELECT venue_id, count(*) OVER (PARTITION BY venue_id) FROM events ORDER BY venue_id;
returns all rows, rather than aggregated rows

BEGIN trans;
ROLLBACK;
COMMIT;


CREATE OR REPLACE viewname AS 

pivot tables using cross tab

http://www.postgresql.org/docs/9.1/static/functions-aggregate.html


INSERT INTO... RETURNING id
CREATE TABLE venues ( venue_id SERIAL PRIMARY KEY,

Describe table
\d+ table_name
select column_name, data_type, character_maximum_length
from INFORMATION_SCHEMA.COLUMNS where table_name = '';
ilike
WHERE title ~ 'z.*'
! - not match
* - case insensitive

fuzzystrmatch levenshtein() - steps to change one string into another


CASE

SELECT *, 
CASE WHEN flag = 0 THEN 'N' ELSE 'Y' END AS Yes_or_No
FROM table1;


FROM table1 t1, table2 t2
vs 
FROM table1 t1 INNER JOIN table2 t2

IS NULL
IS NOT NULL

COUNT(x)

ALL vs DISTINCT (**not** UNIQUE)

WITH clause vs. subquery - PostgreSQL, Oracle, and SQL Server

x LIKE '%foo%'
x NOT LIKE '%foo%'

AND OR

x IN (subquery)

COALESCE(x, '') -- for nulls
