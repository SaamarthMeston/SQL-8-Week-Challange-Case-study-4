
# Case Study Questions

1. How many unique nodes are there on the Data Bank system?
2. What is the number of nodes per region?
3. How many customers are allocated to each region?
4. How many days on average are customers reallocated to a different node?
5. What is the median, 80th and 95th percentile for this same reallocation days metric for each region?


## 1. How many unique nodes are there on the Data Bank system?

SELECT count(DISTINCT node_id) AS unique_nodes
FROM customer_nodes;

## 2. What is the number of nodes per region?

SELECT region_id,
       region_name,
       count(node_id) AS node_count
FROM customer_nodes
INNER JOIN regions USING(region_id)
GROUP BY region_id;

## 3. How many customers are allocated to each region?

SELECT region_id,
       region_name,
       count(DISTINCT customer_id) AS customer_count
FROM customer_nodes
INNER JOIN regions USING(region_id)
GROUP BY region_id;

## 4.How many days on average are customers reallocated to a different node?

SELECT round(avg(datediff(end_date, start_date)), 2) AS avg_days
FROM customer_nodes
WHERE end_date!='9999-12-31';

## 5.What is the median, 80th and 95th percentile for this same reallocation days metric for each region?

* reallocation days metric: days taken to reallocate to a different node
* Percentile found by partitioning the dataset by regions and arranging it in ascending order of reallocation_days
* 95th percentile -> 95% of the values are less than or equal to the current value.

### 95th percentile

WITH reallocation_days_cte AS
  (SELECT *,
          (datediff(end_date, start_date)) AS reallocation_days
   FROM customer_nodes
   INNER JOIN regions USING (region_id)
   WHERE end_date!='9999-12-31'),
     percentile_cte AS
  (SELECT *,
          percent_rank() over(PARTITION BY region_id
                              ORDER BY reallocation_days)*100 AS p
   FROM reallocation_days_cte)
SELECT region_id,
       region_name,
       reallocation_days
FROM percentile_cte
WHERE p >95
GROUP BY region_id;

### 80th percentile

WITH reallocation_days_cte AS
  (SELECT *,
          (datediff(end_date, start_date)) AS reallocation_days
   FROM customer_nodes
   INNER JOIN regions USING (region_id)
   WHERE end_date!='9999-12-31'),
     percentile_cte AS
  (SELECT *,
          percent_rank() over(PARTITION BY region_id
                              ORDER BY reallocation_days)*100 AS p
   FROM reallocation_days_cte)
SELECT region_id,
       region_name,
       reallocation_days
FROM percentile_cte
WHERE p >95
GROUP BY region_id;

### 50th percentile

WITH reallocation_days_cte AS
  (SELECT *,
          (datediff(end_date, start_date)) AS reallocation_days
   FROM customer_nodes
   INNER JOIN regions USING (region_id)
   WHERE end_date!='9999-12-31'),
     percentile_cte AS
  (SELECT *,
          percent_rank() over(PARTITION BY region_id
                              ORDER BY reallocation_days)*100 AS p
   FROM reallocation_days_cte)
SELECT region_id,
       region_name,
       reallocation_days
FROM percentile_cte
WHERE p >50
GROUP BY region_id;# Case Study Questions
