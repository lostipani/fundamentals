# Group by and reduce
The problem is the one in which records are reduced to those having the maximum value of a given feature `values` after having been grouped by another feature `category`. The full row has to be selected.

## Records in the same table
Given a table `tab`:
```sql
SELECT * FROM tab AS t1
JOIN
(
  SELECT category, MAX(values) as max_val
  FROM tab
  GROUP BY category
) AS t2
ON
(
  t1.category = t2.category AND
  t1.values = t2.max_val
);
```
## Records features in different tables
Given a one-to-multi relationship between the table with the `category` feature and a table with the other features including `value`.

```sql

```
