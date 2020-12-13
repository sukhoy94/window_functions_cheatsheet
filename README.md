# Window Functions Cheatsheet

A window function is an SQL function where the input values are taken from a "window" of one or more rows in the results set of a SELECT statement.
Window functions are distinguished from other SQL functions by the presence of an OVER clause. If a function has an OVER clause, then it is a window function. If it lacks an OVER clause, then it is an ordinary aggregate or scalar function. Window functions might also have a FILTER clause in between the function and the OVER clause.

MySQL supports window functions that, for each row from a query, perform a calculation using rows related to that row.

A window function performs an aggregate-like operation on a set of query rows. However, whereas an aggregate operation groups query rows into a single result row, a window function produces a result for each query row:

- The row for which function evaluation occurs is called the current row.

- The query rows related to the current row over which function evaluation occurs comprise the window for the current row.
