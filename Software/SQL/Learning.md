Learning SQL with [Alex the Analyst](https://www.youtube.com/watch?v=HYD8KjPB9F8&list=PLUaB-1hjhk8Fq6RBY-3MQ5MCXB5qxb8VA&index=2).

## Databases, tables, columns and rows (values) and datatypes

## Clauses
### SELECT 

SELECT selects columns from a database table.

```sql
SELECT *
FROM table_001;
```

For this to work, the database has to be preselected. Otherwise it wouldn't work.

For it to work, it should be;
```sql
SELECT *
FROM dbname.table_001;
```

\* selects all. 
if we want a particular column or set of columns from the table, we can replace the star as follows;

```sql
SELECT col_name
FROM dbname.table_001;
```

- Commenting is done with the # like python.

We can even do simple arithmetic on columns.

Select unique values within a column:
```sql
SELECT DISTINCT col_name
FROM dbname.table_001;
```

### WHERE

WHERE puts a condition on the selection.

```sql
SELECT DISTINCT col_name
FROM dbname.table_001
WHERE first_name = 'Leslie' AND salary > 100000;
```

Operators: AND, OR , LIKE
AND, OR is self explanatory
LIKE is doing kind of a regex match without requiring an exact match

### GROUP BY/ORDER BY

This groups or orders things together by some condition

```sql
SELECT DISTINCT col_name
FROM dbname.table_001
GROUP BY gender;
```

