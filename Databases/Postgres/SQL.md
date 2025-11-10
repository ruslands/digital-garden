SQL consist of commands, tokens, identifiers, double-quoted identifiers (")

commands:

SELECT

FROM

UPDATE

DELETE

SELECT DISTINCT: return only distinct (different) values

ON,

LEFT OUTER JOIN,

ORDER BY,

SET,

BEGIN,

COMMIT,

ROLLBACK

OVER,

PARTITION BY,

DESC, ASC,

UNION,

INHERITS,

ONLY,

CAST,

COLLATE: set of rules that tell database engine how to compare and sort the character data 

CREATE TABLE,

DROP TABLE,

DEFAULT,

SERIAL,

CONSTRAINT,

CHECK

COALESCE

ANY

EXPLAIN - show the execution plan of a statement

ON CONFLICT - clause in PostgreSQL works with unique columns or unique constraints. It is used to handle conflicts that arise when attempting to insert a row that would violate a unique constraint.

Constants: there are three kinds of implicitly-typed constants in PostgreSQL: strings, bit strings, and numbers. 

Special Characters:

- A dollar sign ($) followed by digits is used to represent a positional parameter in the body of a function definition or a prepared statement. In other contexts the dollar sign can be part of an identifier or a dollar-quoted string constant.
- Parentheses (()) have their usual meaning to group expressions and enforce precedence. In some cases parentheses are required as part of the fixed syntax of a particular SQL command.
- Brackets ([]) are used to select the elements of an array.
- Commas (,) are used in some syntactical constructs to separate the elements of a list.
- The semicolon (;) terminates an SQL command. It cannot appear anywhere within a command, except within a string constant or quoted identifier.
- The colon (:) is used to select “slices” from arrays.
- The asterisk (*) is used in some contexts to denote all the fields of a table row or composite value. It also has a special meaning when used as the argument of an aggregate function, namely that the aggregate does not require any explicit parameter.
- The period (.) is used in numeric constants, and to separate schema, table, and column names.

Comments:

-- This is a standard SQL comment

/* multiline comment  
 * with nesting: /* nested block comment */  
 */

SELECT city, temp_lo, temp_hi, prcp, date FROM weather;

SELECT city, (temp_hi + temp_lo)/2 AS temp_avg, date FROM weather;

SELECT DISTINCT city FROM weather ORDER BY city;

HAVING and WHERE clauses are used to filter rows resulting from select statement. HAVING clause is used to return the rows that meet a specific condition. WHERE clause is more of the same as HAVING but while it is used to filter through each row, the HAVING clause filters grouped rows. Syntactically, the difference between the two clauses is that the GROUP BY clause occurs before HAVING clause while it occurs after the WHERE clause.

DEFAULT: The default value can be an expressio

CREATE TABLE products (  
    product_no integer DEFAULT nextval('products_product_no_seq'),  
    ...  
);

WHERE: selects input rows before groups and aggregates are computed (thus, it controls which rows go into the aggregate computation). Must not contain aggregate functions

FILTER: another way to select the rows that go into an aggregate computation is to use FILTER, which is a per-aggregate option. FILTER is much like WHERE, except that it removes rows only from the input of the particular aggregate function that it is attached to.

HAVING: selects group rows after groups and aggregates are computed. Always contains aggregate functions. It is allowed to write a HAVING clause that doesn't use aggregates, but it's seldom useful. The same condition could be used more efficiently at the WHERE stage. Occurs before HAVING clause

GROUP BY:

LIKE: operator does pattern matching. LIKE 'S%'

JOIN: Queries that access multiple tables (or multiple instances of the same table) at one time are called join queries.

SELECT * FROM weather JOIN cities ON city = name;

SELECT weather.city, weather.temp_lo, weather.temp_hi, weather.prcp, weather.date, cities.location FROM weather JOIN cities ON weather.city = cities.name;

This query is called a left outer join because the table mentioned on the left of the join operator will have each of its rows in the output at least once, whereas the table on the right will only have those rows output that match some row of the left table. 

SELECT * FROM weather LEFT OUTER JOIN cities ON weather.city = cities.name;

We can also join a table against itself. This is called a self join. 

SELECT w1.city, w1.temp_lo AS low, w1.temp_hi AS high, w2.city, w2.temp_lo AS low, w2.temp_hi AS high  
FROM weather w1 JOIN weather w2 ON w1.temp_lo < w2.temp_lo AND w1.temp_hi > w2.temp_hi;

Aggregate Functions:

SELECT max(temp_lo) FROM weather;

SELECT city FROM weather WHERE temp_lo = (SELECT max(temp_lo) FROM weather);

SELECT city, count(*), max(temp_lo) FROM weather GROUP BY city;

SELECT city, count(*), max(temp_lo) FROM weather GROUP BY city HAVING max(temp_lo) < 40;

SELECT city, count(*), max(temp_lo) FROM weather WHERE city LIKE 'S%'  GROUP BY city;

VIEW: Suppose you do not want to type the query each time you need it. You can create a view over the query, which gives a name to the query that you can refer to like an ordinary table. It defines a view of a query. The view is not physically materialized. Instead, the query is run every time the view is referenced in a query. Parameters: Temporary or Recursive. Temporary views are automatically dropped at the end of the current session.

Is a stored query. When you select from it, you essentially run the query.

CREATE VIEW foo AS SELECT col1, col2 FROM bar;

is same as

SELECT foo.* FROM (SELECT col1, col2 FROM bar) AS foo;

FOREIGN KEY:

CREATE TABLE cities (name varchar(80) primary key, location point);

CREATE TABLE weather (city varchar(80) references cities(name),  
        temp_lo   int,  
        temp_hi   int,  
        prcp      real,  
        date      date  
);

Inheritance: Let's create two tables: A table cities and a table capitals. Naturally, capitals are also cities, so you want some way to show the capitals implicitly when you list all cities. 

CREATE TABLE cities (  
  name       text,  
  population real,  
  elevation  int     -- (in ft)  
);

CREATE TABLE capitals (  
  state      char(2) UNIQUE NOT NULL  
) INHERITS (cities);

SELECT name, elevation  
    FROM ONLY cities  
    WHERE elevation > 500;

Here the ONLY before cities indicates that the query should be run over only the cities table, and not tables below cities in the inheritance hierarchy. Many of the commands that we have already discussed — SELECT, UPDATE, and DELETE — support this ONLY notation.

Generated columns: is a special column that is always computed from other columns. it is for columns what a view is for tables. There are two kinds of generated columns: stored and virtual. A stored generated column is computed when it is written (inserted or updated) and occupies storage as if it were a normal column. A virtual generated column occupies no storage and is computed when it is read. Thus, a virtual generated column is similar to a view and a stored generated column is similar to a materialized view (except that it is always updated automatically). PostgreSQL currently implements only stored generated columns.

A generated column cannot be written to directly. In INSERT or UPDATE commands, a value cannot be specified for a generated column, but the keyword DEFAULT may be specified. Consider the differences between a column with a default and a generated column. The column default is evaluated once when the row is first inserted if no other value was provided; a generated column is updated whenever the row changes and cannot be overridden. 

CREATE TABLE people (  
    ...,  
    height_cm numeric,  
    height_in numeric GENERATED ALWAYS AS (height_cm / 2.54) STORED);