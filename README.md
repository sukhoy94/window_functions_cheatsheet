https://dev.mysql.com/doc/refman/8.0/en/window-functions-usage.html

# Window Functions Cheatsheet

A window function is an SQL function where the input values are taken from a "window" of one or more rows in the results set of a SELECT statement.
Window functions are distinguished from other SQL functions by the presence of an OVER clause. If a function has an OVER clause, then it is a window function. If it lacks an OVER clause, then it is an ordinary aggregate or scalar function. Window functions might also have a FILTER clause in between the function and the OVER clause.

MySQL supports window functions that, for each row from a query, perform a calculation using rows related to that row.

A window function performs an aggregate-like operation on a set of query rows. However, whereas an aggregate operation groups query rows into a single result row, a window function produces a result for each query row:

- The row for which function evaluation occurs is called the current row.

- The query rows related to the current row over which function evaluation occurs comprise the window for the current row.




aggregate example: 

```
SELECT SUM(views) FROM posts
SELECT SUM(views), author FROM posts GROUP BY author
```


A window function performs an aggregate-like operation on a set of query rows. However, whereas an aggregate operation groups query rows into a single result row, a window function produces a result for each query row.
```
SELECT id, author, title, body, category, views, SUM(views) OVER() as total_views FROM posts;
SELECT id, author, title, body, category, views, SUM(views) OVER() as total_views, SUM(views) OVER(partition BY author) as total_author_views FROM posts;
```


Each window operation in the query is signified by inclusion of an OVER clause that specifies how to partition query rows into groups for processing by the window function:

- The first OVER clause is empty, which treats the entire set of query rows as a single partition. The window function thus produces a global sum, but does so for each row.

- The second OVER clause partitions rows by country, producing a sum per partition (per author). The function produces this sum for each partition row.

Window functions are permitted only in the select list and ORDER BY clause. Query result rows are determined from the FROM clause, after WHERE, GROUP BY, and HAVING processing, and windowing execution occurs before ORDER BY, LIMIT, and SELECT DISTINCT.


# SQL's

- cume_dist()

```
SELECT
   id,
   author,
   title,
   body,
   category,
   views,
   CUME_DIST()  OVER (ORDER BY views) AS views_distribution
FROM
   posts
```

- dense_rank()

```
SELECT
   id,
   author,
   title,
   body,
   category,
   views,
   DENSE_RANK() over (ORDER BY views DESC) as views_rank
FROM
   posts
ORDER BY views_rank
```

- rank()

```
SELECT
   id,
   author,
   title,
   body,
   category,
   views,
   RANK() over (ORDER BY views DESC) as views_rank
FROM
   posts
ORDER BY views_rank
```

- first_value()

```
SELECT
   id,
   author,
   title,
   body,
   category,
   views,
   FIRST_VALUE(views) OVER (PARTITION BY category) as first_views
FROM
   posts
```

- last_value()

```
SELECT
   id,
   author,
   title,
   body,
   category,
   views,
   LAST_VALUE(views) OVER (
       PARTITION BY category
       ORDER BY id 
       RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
   ) as last_views
FROM
   posts
ORDER BY category, id
```

- lag()

```
SELECT 
    id, 
    author, 
    title, 
    body, 
    category, 
    views,
    row_number() OVER w AS rating,
    lag(views) OVER w - views  AS views_lag
FROM 
    posts
WINDOW w AS (ORDER BY views DESC)
ORDER BY rating;

SELECT 
    id, 
    author, 
    title, 
    body, 
    category, 
    views,
    LAG(views) OVER (
        PARTITION BY category ORDER BY id
    ) as views_lag
FROM 
    posts
ORDER BY category, id;

```

- lead()

```
SELECT
   id,
   author,
   title,
   body,
   category,
   views,
   LEAD(views) OVER (
       PARTITION BY category ORDER BY id
   ) as views_lead
FROM
   posts
ORDER BY category, id
```

- nth_value()

```
SELECT
    id,
    author,
    title,
    body,
    category,
    views,
    NTH_VALUE(views, 1) OVER (
        PARTITION BY category ORDER BY id
        ) as views_second_in_frame
FROM
    posts
ORDER BY category, id
```

- ntile()

```
SELECT
   id,
   author,
   title,
   body,
   category,
   views,
   NTILE(2) OVER (
       ORDER BY id
       ) as ntile_views
FROM
   posts
ORDER BY category, id
```

- percent_rank()

```
SELECT
    id,
    author,
    title,
    body,
    category,
    views,
    PERCENT_RANK() OVER (ORDER BY views) AS 'percent_rank'
FROM
    posts
```
