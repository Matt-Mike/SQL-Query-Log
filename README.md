# SQL-Query-Log
Here's a log of my SQL skillset!

# BETWEEN
Used to match a value against a range of values.

SELECT COUNT column<br/>
FROM table<br/>
WHERE column BETWEEN 8 AND 9

Can input NOT BETWEEN to return values not inbetween the designated values.

# COUNT/COUNT DISTINCT
The COUNT function returns the number of input rows that match a specific condtion of a query.<br/>
COUNT DISTINCT will return only the distinct number of values from a column.<br/>

SELECT COUNT (name) FROM table

SELECT COUNT(DISTINCT name)<br/>
FROM table

# IN
Creates a condition that checks to see if a value is included in a list of multiple options.

SELECT color<br/>
FROM table WHERE color IN ('red','blue')

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

# ORDER BY
Sorts rows based on a column value, in ascending or descending order.

SELECT column_1, column_2<br/>
FROM table<br/>
ORDER BY column_1 ASC/DESC

# WHERE
The WHERE function specifies conditions on columns for the rows to be returned.

SELECT column1, column2<br/>
FROM table<br/>
WHERE conditions (ex: name='David')
