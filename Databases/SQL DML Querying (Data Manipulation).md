Querying in [[SQL]] is all about obtaining data from existing relations. 
This basically does [[Relational Algebra]] with a declarative language. 
###### Basic Queries

| Line                    | Step                        |
| ----------------------- | --------------------------- |
| `SELECT first_name`     | 3 - Filter down the columns |
| `FROM teachers`         | 1 - Get table               |
| `WHERE salary > 100000` | 2 - Filter down the rows    |

| Line                            | Step                                                                                                |
| ------------------------------- | --------------------------------------------------------------------------------------------------- |
| `SELECT AVG(salary) as avg_sal` | 5 - Select the data                                                                                 |
| `FROM teachers`                 | 1 - Get tables & perform joins from other relations<br>Can filter down custom relations before join |
| `WHERE salary > 100000`         | 2 - Eliminate specified rows/tuples                                                                 |
| `GROUP BY(dept_name)`           | 3 - Rows are grouped and aggregated                                                                 |
| `HAVING avg(salary) > 120000`   | 4 - Eliminate specified aggregated groups                                                           |
| `ORDER BY avg_sal DESC`         | 6 - Order the data                                                                                  |
| `LIMIT 5`                       | 7 - Limit output to only show 5 results                                                             |

###### SELECT
Two purposes: 
1. Select attributes
2. Create 'new attributes' or variables using any standard programming (operations, if-else, other variables as inputs, etc.)

- `SELECT distinct(A1)`: only get distinct tuples
- `SELECT *`: select all attributes
- `SELECT A1 as new_A1`: rename attribute/column
- `SELECT A1 + 20 as A1_multiplied`: perform operation on values in column and rename
- `SELECT E.name`: select attribute from specific relation
- `SELECT COALESCE(c.email, c.phone) AS contact_method`: get first non-null of the listed attributes and combine into 1
- `SELECT (CASE WHEN c.country = 'US' THEN c.state ELSE c.country END) as region`: create new attribute with if-else filtering & logic
###### FROM & JOIN, Nested
- `FROM employees`: from a single relation
- `FROM employees E`: rename/alias table

We can join multiple relations, including relations we've created from queries (nested):
- `FROM employees E INNER JOIN sales S ON (E.EID = S.EID)`: Inner join on common attributes
- `LEFT JOIN`: keep all the left rows, but add in extra data from right if it matches
	- Same idea for `RIGHT JOIN`
- `OUTER JOIN`: outer product, matching all rows on each side
- All types of joins are `CROSS JOIN` with a selection filter

In general, we use only `INNER JOIN` and `LEFT JOIN`:
- `LEFT JOIN`: use if we want to keep all records on the left / do an inner join, but fill in NULL
	- Left join of patients with admissions will keep all patients, and match admissions where applicable
- `INNER JOIN`: only keep records on the left that have a valid xxx on the right
	- Think about what the join means
	- Inner join of patients with admissions on patient id will return all admissions for all patients with at least 1 admission

| Join Type                      | CROSS JOIN Comparison                                                                                                | Join on unique (primary) attribute | Join on any attribute          |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------- | ------------------------------ |
| CROSS JOIN<br>(never used)     | Outer product<br>Cartesian product                                                                                   | (x * y) always returned            | (x * y) always returned        |
| INNER JOIN<br>JOIN             | Select rows of cross join <br>Cartesian product that <br>match the attribute filter                                  | min: 0 <br>max: max(x, y)          | min: 0<br>max: (x * y)         |
| LEFT OUTER JOIN<br>LEFT JOIN   | Same as `INNER JOIN` but <br>keep left rows, even if<br>they don't match the<br>filter (replace right rows with NULL | min: x <br>max: (x + y - 1)        | min: x<br>max: (x * y)         |
| RIGHT OUTER JOIN<br>RIGHT JOIN | Same thing, but keep right rows                                                                                      | min: y<br>max: (x + y - 1)         | min: y<br>max: (x * y)         |
| FULL OUTER JOIN<br>FULL JOIN   | Same thing, but keep both rows                                                                                       | min: max(x, y)<br>max: (x + y)     | min: max(x, y)<br>max: (x * y) |

Helpful stuff:
- `USING`: instead of `JOIN sales ON E.EID = S.EID`, you can do `JOIN sales USING (EID)`. **Remember parentheses**

Nested:
- The output of a query is a relation... can do `FROM A INNER JOIN (SELECT...) WHERE`
- `UNION`: combine rows of multiple queries if they have the same column types. `(SELECT ...) UNION (SELECT ...)`
	- `UNION ALL` allows duplicates while `UNION` removes duplicates

###### WHERE
- `WHERE A1 LIKE '%dar%'`: filter rows by string
	- Use **Single Quotes** for strings
	- `'%'`: match any substring, like `*` on Linux
	- `'_'`: match any single character
	- `'\'`: escape for special characters like %
	- `'||'`: concatenate
	- Many more! 
- Binary (3): `AND`, `OR`, `NOT`
	- `WHERE date IS NOT NULL`: skip nulls
- Comparator (6): `=`, `>`, `>=`, `<`, `<=`, `<>`
	- `WHERE A1 BETWEEN 90 AND 100`: basically 2 comparators, **inclusive**
- Lists: `WHERE ID IN (1, 30, 45)`: specify list of values
###### ORDER BY
- `ORDER BY A1 ASC` / `ORDER BY A1`: **default** order by ascending attribute
- `ORDER BY A1 DESC`: order by descending attributes
- `ORDER BY A1 ASC, A2 DESC`: sub-ordering
###### NULL
- Unknown **or** value does not exist
- Arithmetic: `null + 5 = null`
- Predicate: `WHERE A1 = NULL`: get all null values
- Comparison: `5 < null`: unknown 
###### GROUP BY, HAVING & Aggregation
Average **over** something, like department.
WHERE cannot be used for filtering aggregates - must use HAVING without the rename.
```sql
SELECT AVG(salary) as avg_sal
FROM teachers
GROUP BY(dept_name)
HAVING avg(salary) > 4200
```
- `AVG`, `MIN`, `MAX`, `SUM`, `COUNT`
- Without `GROUP BY`, it will do it over the whole table
	- `SELECT first_name, last_name, MAX(height)` will get a single entry with the max height
	- `SELECT COUNT(*)` will get the count of all rows that match the filter
- Without aggregate function, we can still use aggregates if we don't want to display the aggregation result
	- `GROUP BY first_name HAVING COUNT(*) = 1`: we know count is 1, don't show it
- Can aggregate with filters inside function
	- `SELECT SUM(Gender = 'M') as male_count` instead of `WHERE Gender = 'M'`
- `COUNT(A1)`: excludes NULL values
- `COUNT(DISTINCT A1)`: gets distinct count

1. Start by including aggregate in query
2. Group accordingly
3. Filter by the aggregation if needed
4. Order by the aggregation if needed
5. Remove the original aggregate if we don't want to display it
	1. If we do want to keep it, replace the group/having/order by with the name of the aggregation instead
###### LIMIT
- `LIMIT 5`: The final statement in a query can limit the number of rows
###### Time Functions
- These can be used for creating new variables/attributes, or for filtering rows
- Filter by people born in 2010
	- Not great: `WHERE birth_date >= '2010-01-01' AND birth_date <= '2010-12-31'`: filter by time range
	- Not great: `WHERE birth_date BETWEEN '2010-01-01' AND '2010-12-31'`
	- Better: `WHERE YEAR(birth_date) = 2010`
- `YEAR()`, `MONTH()`, `DAY()` returns them as integers (2000, 5, 31)
###### String Functions
- `CONCAT(first_name, ' ', last_name)` concatenates
	- Same as `first_name || ' ' || last_name`
- `LEN()` gets length of string
- `UPPER()` and `LOWER()` converts strings to upper or lower
###### Numeric Functions
- `ROUND(height / 30.48, 1)`: round decimal to 1 place
- `FLOOR(weight / 10)`, `CEIL()`: rounds down to nearest int / up to nearest int
- `POWER(height, 2)`: square in this case, but general exponential
- Division: to convert to floats, do `/10.0`
###### To Do
- Think about the foreign key constraints and what that means for choosing filter selection (INNER, LEFT, OUTER)