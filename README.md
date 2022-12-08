# SQL-Query-Log
Here's a log of my SQL skillset!

# Aggregate Functions
AVG() - returns an average value<br/>
ROUND() to specify precision after a decimal<br/>
COUNT() - returns number of values<br/>
MAX() - returns maximum value<br/>
MIN() - returns minimum value<br/>
SUM() - returns the sum of all values

# ALTER Table
Allows for changes to an existing table structure

**Adding columns**<br/>
ALTER TABLE table_name<br/>
ADD COLUMN new_col TYPE

**Alter constraints**<br/>
ALTER TABLE table_name<br/>
ALTER COLUMN col_name<br/>
SET DEFAULT value

(Can also ADD CONSTRAINT constraint_name in place of SET DEFAULT value)

Example Query:<br/>
ALTER TABLE information<br/>
RENAME TO new_info

ALTER TABLE new_info<br/>
RENAME COLUMN person TO people

ALTER TABLE new_info<br/>
ALTER COLUMN people DROP NOT NULL

# BETWEEN
Used to match a value against a range of values.

SELECT COUNT column<br/>
FROM table<br/>
WHERE column BETWEEN 8 AND 9

Can input NOT BETWEEN to return values not inbetween the designated values.

# CASE
Executes an SQL code when certain conditions are met, (similar to an IF/ELSE statement)

General CASE Example query:<br/>
SELECT customer_id,<br/>
CASE<br/>
    WHEN (customer_id <= 100) THEN 'Premium'
    WHEN (customer_id BETWEEN 100 AND 200) THEN 'Plus'<br/>
    ELSE 'Normal'<br/>
END AS customer_class<br/>
FROM customer

Another example:<br/>
SELECT<br/>
SUM(CASE rental_rate<br/>
    WHEN 0.99 THEN 1<br/>
    ELSE 0<br/>
END) AS number_of_bargains<br/>
FROM film

- This would return a summed number of everything given the value 1

# CAST
Converts one data type to another<br/>
- Must be reasonable conversion
- Ex: '5' to an integer will work, 'five' to an integer will not

Syntax:<br/>
SELECT CAST('5' AS INTEGER)

Can use in a SELECT query with a column name instead of a single instance.<br/>
Example:<br/>
SELECT CAST(date AS TIMESTAMP)<br/>
FROM table

# Changing the case of a string<br/>
**Upper Case**<br/>
SELECT UPPER(email)<br/>
FROM customer

**Lower Case**<br/>
SELECT LOWER(email)<br/>
FROM customer

**Title Case**<br/>
SELECT INITCAP(title)<br/>
FROM film

# CHECK
Creates more customized constraints that adhere to a certain condition.<br/>
Example: Making sure all inserted integer values fall below a certain threshold.

Example query:<br/>
CREATE TABLE employees(<br/>
    emp_id SERIAL PRIMARY KEY,<br/>
    first_name VARCHAR(50) NOT NULL,<br/>
    last_name VARCHAR(50) NOT NULL,<br/>
    birthdate DATE CHECK (birthdate > '1900-01-01'),<br/>
    hire_date DATE CHECK (hire_date > birthdate),<br/>
    salary INTEGER CHECK (salary > 0)<br/>
    )

# COALESCE
Accepts an ulimited number of arguments. It returns the first argument that is not null. If all arguments are null, the COALESCE function will return null.
- Useful when querying a table that contains null values and substituting it with another value.

Example query:<br/>
SELECT item,(price - COALESCE(discount,0))<br/>
AS final FROM table

Example query:<br/>
SELECT COALESCE(country, 'Both countries') AS country,<br/>
COALESCE(medal, 'All medals') AS medal,<br/>
COUNT(* ) AS awards<br/>
FROM summer_medals<br/>
WHERE year = 2008 AND COUNTRY IN ('CHN, 'RUS')<br/>
GROUP BY ROLLUP(country, medal)<br/>
ORDER BY country ASC, medal ASC);

# COUNT/COUNT DISTINCT
The COUNT function returns the number of input rows that match a specific condtion of a query.<br/>
COUNT DISTINCT will return only the distinct number of values from a column.<br/>

SELECT COUNT (name) FROM table

SELECT COUNT(DISTINCT name)<br/>
FROM table

# CREATE TABLE
General syntax:

CREATE TABLE players(<br/>
    player_id SERIAL PRIMARY KEY,<br/>
    age INTEGER NOT NULL,<br/>
)

Another example:<br/>
CREATE TABLE account(<br/>
  user_id SERIAL PRIMARY KEY,<br/>
  username VARCHAR(50) UNIQUE NOT NULL,<br/>
  password VARCHAR(50) NOT NULL,<br/>
  email VARCHAR(250) UNIQUE NOT NULL,<br/>
  created_on TIMESTAMP NOT NULL,<br/>
  last_login TIMESTAMP<br/>
  )

**SERIAL** - typical for the primary key type as it logs unique integer entries automatically upon insertion

# DELETE
Removes rows from a table

Syntax:<br/>
DELETE FROM table<br/>
WHERE row_id = 1

Delete rows based on their presence in other tables<br/>
Example:<br/>
DELETE FROM tableA<br/>
USING tableB<br/>
WHERE tableA.id=tableB.id

Delete all rows from a table<br/>
DELETE FROM table

# DROP
Removes a column in a table along with its indexes and constraints.<br/>
Does not remove columns used in views, triggers, or stored procedures without the CASCADE clause.

General syntax:<br/>
ALTER TABLE table_name<br/>
DROP COLUMN col_name

(Insert CASCADE at the very end to remove all dependencies)

Check for existence to avoid error:<br/>
ALTER TABLE table_name<br/>
DROP COLUMN IF EXISTS col_name

(Can drop multiple columns as well)

# FRAMES

**ROWS BETWEEN**<br/>
ROWS BETWEEN [START] AND [FINISH]

**n PRECEDING:** n rows before the current row

**CURRENT ROW:** the current row

**n FOLLOWING:** n rows after the current row

Examples:<br/>
ROWS BETWEEN 3 PRECEDING AND CURRENT ROW<br/>
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING<br/>
ROWS BETWEEN 5 PRECEDING AND 1 PRECEDING

**RANGE BETWEEN**:<br/>
RANGE BETWEEN [START] AND [FINISH]<br/>
Similar to ROWS BETWEEN<br/>
RANGE treats duplicates in OVER's ORDER BY subclause as a single entity

# GROUP BY
Aggregates columns per category.

Example syntax:<br/>
SELECT customer_id,SUM(amount)<br/>
FROM payment<br/>
GROUP BY customer_id<br/>
ORDER BY SUM(amount)

# HAVING
Places a filter after an aggregation has already taken place

SELECT company,SUM(sales)<br/>
FROM finance_table<br/>
WHERE company != 'Google'<br/>
GROUP BY company<br/>
HAVING SUM(sales) > 1000

# IN
Creates a condition that checks to see if a value is included in a list of multiple options.

SELECT color<br/>
FROM table WHERE color IN ('red','blue')

# INSERT
Add rows into a table<br/>
General syntax:<br/>
INSERT INTO table (column1, column2)<br/>
VALUES<br/>
(value1, value2),<br/>
(value1, value2)

Syntax for sharing values from another table:<br/>
INSERT INTO table(column1,column2)<br/>
SELECT column1,column2<br/>
FROM another_table<br/>
WHERE condition;

Note: SERIAL columns do not need to be provided a value

# JOINS (INNER/OUTER/LEFT/RIGHT) | AS Statement | UNION
JOINS combine information from multiple tables

**AS** creates an "alias" for a column or result<br/>
Example syntax:<br/>
SELECT SUM(column) AS new_name<br/>
FROM table

**INNER JOIN** returns results that match two different tables<br/>
Examples syntax:<br/>
SELECT * FROM TableA<br/>
INNER JOIN TableB<br/>
ON TableA.col_match = TableB.col_match

**FULL OUTER JOIN** Combines unique values from two different tables<br/>
Example syntax:<br/>
SELECT * FROM customer<br/>
FULL OUTER JOIN payment<br/>
ON customer.customer_id = payment.customer_id<br/>
WHERE customer.customer_id IS null<br/>
OR payment.payment_id IS null

**LEFT OUTER JOIN** Returns set of records that are in the left table AND in both tables. If there is no match with the right table, the results are null<br/>
(Order does matter!)

**RIGHT OUTER JOIN** The same as LEFT OUTER JOIN but reversed

**UNION** Combines the results of tow or more SELECT statements.<br/>
Essentially concatenates two results together.

Example syntax:<br/>
SELECT column_name(s) FROM table1<br/>
UNION<br/>
SELECT column_name(s) FROM table2

# LIKE/ILIKE
Performs pattern matching against string data with the use of **wildcard** characters

% - matches any sequence of characters<br/>
_ - matches any single character

LIKE is case-sensitive<br/>
ILIKE is not

Example syntax:<br/>
SELECT * FROM customer<br/>
WHERE first_name ILIKE 'J%' AND last_name LIKE '%her%'

# LIMIT
Limits the number of rows returned for a query.<br/>
Goes at the very end of a query

SELECT column<br/>
FROM table<br/>
ORDER BY column DESC<br/>
LIMIT 5

# Logical Operators
Combine multiple comparison operators

AND<br/>
OR<br/>
NOT<br/>

# NULLIF
Takes 2 inputs and returns NULL if both are equal. Otherwise it returns the first argument passed.<br/>
- Useful in cases where a NULL value would cause an error or unwanted result

Example:<br/>
NULLIF(10,10)<br/>
- Returns NULL

NULLIF(10,12)<br/>
- Returns 10

# ORDER BY
Sorts rows based on a column value, in ascending or descending order.

SELECT column_1, column_2<br/>
FROM table<br/>
ORDER BY column_1 ASC/DESC

# REPLACE<br/>
Replace characters in a string.

SELECT REPLACE(description, 'A Astounding', 'An Astounding') AS description<br/>
FROM film

# REVERSE<br/>
Reverses the order of a string.

SELECT title, REVERSE(title)<br/> 
FROM film AS f

# ROLLUP and CUBE

**ROLLUP**
GROUP BY subclause that includes extra rows for group-level aggregations.

Example syntax:<br/>
SELECT country, medal, COUNT(*) AS awards<br/>
FROM summer_medals<br/>
WHERE year = 2008 AND country IN ('CHN', 'RUS')<br/>
GROUP BY country, ROLLUP(country, medal)<br/>
ORDER BY country ASC, medal ASC;

GROUP BY COUNTRY, ROLLUP(medal) will count all country- and medal- level totals, then count only country- level totals and fill in medal with nulls for these rows.<br/>
ROLLUP is hierarchical, de-aggregating from the leftmost provided column to the right-most<br/>
ROLLUP(country, medal) includes country- level totals<br/?
ROLLUP(medal, country) includes medal-level totals

**CUBE**<br/>
CUBE is a non-hierarchical ROLLUP<br/>
It generates all possible group-level aggregations

CUBE(country,medal) counts country-level, medal-level, and grand totals

# SELF-JOIN

A query in which a table is joined to itself.<br/>
Useful for comparing values in a column of rows within the same table.<br/>
It is necessary to use an alias for the table.

Example syntax:<br/>
SELECT tableA.col, tableB.col<br/>
FROM table AS tableA<br/>
JOIN table AS tableB ON<br/>
tableA.some_col = tableB.other_col

SELECT f1.title, f2.title, f1.length<br/>
FROM film AS f1<br/>
INNER JOIN film AS f2 ON<br/>
f1.film_id = f2.film_id<br/>
AND f1.length = f2.length

# String Functions and Operators<br/>
Edits, combines and alters text data columns

Example syntax:<br/>
SELECT **LENGTH**(first_name) FROM customer<br/>
Example return: 5

SELECT title, **CHAR_LENGTH**(title)<br/>
FROM film;

SELECT first_name || last_name FROM customer<br/>
Example return: JackJohnson

SELECT first_name || ' ' || last_name<br/>
Example return: Jack Johnson

SELECT LOWER(LEFT(first_name,1)) || LOWER(last_name) || '@gmail.com'<br/>
FROM customer<br/>
Example return: jjohnson@gmail.com

SELECT CONCAT(first_name, ' ', last_name) AS full_name<br/>
FROM customer;

**POSITION**<br/>
SELECT email, POSITION('@' IN email)<br/>
FROM customer;

**STRPOS** Analogous to POSITION
SELECT email, STRPOS(email, '@')<br/>
FROM customer;

**LEFT**<br/>
SELECT LEFT(description, '50') FROM film;

**RIGHT**<br/>
SELECT RIGHT(description, '50') FROM film;

**SUBSTRING**<br/>
Extract a substring from data.

SELECT SUBSTRING(description, 10, 50) FROM film AS f;<br/>
Starting at the 10th character, the next 50 characters will be returned.

SELECT SUBSTRING(email FROM 0 FOR POSITION('@' IN email))<br/>
FROM customer;<br/>
Example return: MARY.SMITH (returns everything before '@' in the email)

# STRING_AGG<br/>
STRING_AGG(column, separator) takes all the values of a column and concatenates them, with separator in between each value

# SubQuery

Performs a query on the results of another query

SELECT title,rental_rate<br/>
FROM film<br/>
WHERE rental_rate ><br/>
(SELECT AVG(rental_rate) FROM film)

SELECT film_id,title<br/>
FROM film<br/>
(SELECT inventory.film_id<br/>
FROM rental<br/>
INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id<br/>
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30')

SELECT first_name,last_name<br/>
FROM customer AS c<br/>
WHERE EXISTS<br/>
(SELECT * FROM payment AS p<br/>
WHERE p.customer_id = c.customer_id<br/>
AND amount > 11)

# TIMESTAMPS and EXTRACT

TIME - Contains only time<br/>
DATE - Contains only date<br/>
TIMESTAMP - Contains date and time<br/>
TIMESTAMPTZ - Contains date, time, and timezone

TIMEZONE<br/>
NOW<br/>
TIMEOFDAY<br/>
CURRENT_TIME<br/>
CURRENT_DATE<br/>

**EXTRACT()** - Obtain a sub-component of a date value (year, month, day, week, or quarter)<br/>
Example syntax:<br/>
EXTRACT(YEAR FROM date_col)

SELECT EXTRACT(MONTH FROM payment_date)<br/>
AS pay_month<br/>
FROM payment

**AGE()** - Calculates and returns the current age given to a timestamp<br/>
AGE(date_col)<br/>
Example return: 13 years 1 mon 5 days 01:34:13.003423

**TO_CHAR()** - Converts data types to text<br/>
Usage:<br/>
T0_CHAR(date_col,'mm-dd-yyyy')

SELECT TO_CHAR(payment_date,'MONTH-YYYY')<br/>
FROM payment

# UPDATE
Changes values of columns in a table

General syntax:<br/>
UPDATE table<br/>
SET column1 = value1,<br/>
    column2 = value2<br/>
WHERE<br/>
    condition;

Example:<br/>
UPDATE account<br/>
SET last_login = CURRENT_TIMESTAMP<br/>
    WHERE last_login IS NULL;
    
**UPDATE join**<br/>
UPDATE TableA<br/>
SET original_col = TableB.new_col<br/>
FROM tableB<br/>
WHERE tableA.id = TableB.id

To simply return affected rows, use the RETURNING function<br/>
Example:<br/>
UPDATE account<br/>
SET last_login = created_on<br/>
RETURNING account_id,last_login

# VIEW
Stores a specific query

Example query:<br/>
CREATE VIEW customer_info AS<br/>
SELECT first_name,last_name,address FROM customer<br/>
INNER JOIN address<br/>
ON customer.address_id = address.address_id

When you want to use this VIEW, you would simply input:<br/>
SELECT * FROM customer_info

To modify the VIEW, input CREATE OR REPLACE VIEW at the beginning of the query and then make the needed adjustments to the VIEW syntax

# WHERE
The WHERE function specifies conditions on columns for the rows to be returned.

SELECT column1, column2<br/>
FROM table<br/>
WHERE conditions (ex: name='David')

# Window Functions

**FIRST_VALUE(column)**<br/>
Returns the first value in the table or partition.

**LAST_VALUE(column)**<br/>
Returns the last value in the table or partition.

Example Syntax for FIRST_VALUE and LAST_VALUE:<br/>
SELECT year, city<br/>
FIRST_VALUE(city) OVER(ORDER BY year ASC) AS first_city,<br/>
LAST_VALUE(city) OVER(ORDER BY year ASC<br/>
RANGE BETWEEN<br/>
UNBOUNDED PRECEEDING AND<br/>
UNBOUNDED FOLLOWING)<br/>
AS last_city<br/>
FROM hosts<br/>
ORDER BY year ASC;


**ROW_NUMBER()**<br/>

SELECT col_name,<br/>
    ROW_NUMBER() OVER() AS alias<br/>
FROM table_name<br/>
ORDER BY alias;

**LAG**<br/>
Returns column's value at the row n rows before the current row.

SELECT column1, column2<br/>
LAG(column2, 1) OVER (ORDER BY column1 ASC)<br/>
FROM table<br/>
ORDER BY column1 ASC;

**LEAD**<br/>
Returns column's value at the row n rows after the current row.

WITH hosts AS (<br/>
SELECT DISTINCT year, city<br/>
FROM summer_medals)<br/>

SELECT year, city,<br/>
LEAD(city, 1) OVER (ORDER BY year ASC) AS next_city<br/>
LEAD(city, 2) OVER (ORDER BY year ASC) AS after_next_city<br/>
FROM hosts<br/>
ORDER BY year ASC;

**NTILE(n)**<br/>
Splites the data into n approximately equal pages.

WITH disciplines AS (<br/>
SELECT DISTINCT discipline<br/>
FROM summer_medals)

SELECT discipline, NTILE(15) OVER () AS page<br/>
FROM disciplines<br/>
ORDER BY page ASC;

WITH Athlete_Medals AS (<br/>
  SELECT Athlete, COUNT(*) AS Medals<br/>
  FROM Summer_Medals<br/>
  GROUP BY Athlete<br/>
  HAVING COUNT(*) > 1)<br/>
  
SELECT<br/>
  Athlete,<b/r>
  Medals,<br/>
  NTILE(3) OVER() ORDERY BY medals AS Third<br/>
FROM Athlete_Medals<br/>
ORDER BY Medals DESC, Athlete ASC;

**PARTITION BY**<br/>
Splits the table into partitions based on a column's unique values

SELECT column1, column2, column3,<br/>
LAG(column3) OVER (PARTITION BY column2<br/>
ORDER BY column2 ASC, column1 ASC)<br/>
FROM table_name<br/>
ORDER BY column2 ASC, column1, ASC<br/>

**ROW_NUMBER()**<br/>
Assigns unique numbers to rows, regardless of duplicates.

**RANK()**<br/>
Assigns the same number to rows with identical values, skipping over the next numbers in such cases.

**DENSE_RANK()**<br/>
Also assigns the same number to rows with indetical values, but doesn't skip over the next numbers.

Example syntax for ROW_NUMBER, RANK and DENSE_RANK:<br/>
SELECT country, games,<br/>
ROW_NUMBER() OVER (ORDER BY games DESC) AS row_n,<br/>
RANK() OVER (ORDER BY games DESC) AS rank_n,<br/>
DENSE_RANK() OVER (ORDER BY games DESC) AS dense_rank_n<br/>
FROM table<br/>
ORDER BY games DESC, country ASC;
