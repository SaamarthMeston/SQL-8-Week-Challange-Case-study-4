
# Case Study Questions

1. How many unique nodes are there on the Data Bank system?
2. What is the number of nodes per region?
3. How many customers are allocated to each region?
4. How many days on average are customers reallocated to a different node?
5. What is the median, 80th and 95th percentile for this same reallocation days metric for each region?


## 1. How many unique nodes are there on the Data Bank system?

SELECT count(DISTINCT node_id) AS unique_nodes
FROM customer_nodes;

![A1](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/67d51e4a-211d-4de0-96f9-4780717f5457)

## 2. What is the number of nodes per region?

SELECT region_id,
       region_name,
       count(node_id) AS node_count
FROM customer_nodes
INNER JOIN regions USING(region_id)
GROUP BY region_id;
![A2](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/029c6cb6-b6a4-45e5-a8ab-5cdb1eb1f010)


## 3. How many customers are allocated to each region?

SELECT region_id,
       region_name,
       count(DISTINCT customer_id) AS customer_count
FROM customer_nodes
INNER JOIN regions USING(region_id)
GROUP BY region_id;
![A3](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/c1467e9c-dd59-4ec4-a2fc-840f7880cf08)


## 4.How many days on average are customers reallocated to a different node?

SELECT round(avg(datediff(end_date, start_date)), 2) AS avg_days
FROM customer_nodes
WHERE end_date!='9999-12-31';
![A4](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/38a2d0b2-38df-4133-b90b-f63b8a9389ad)


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
![A51](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/c6123ffd-9048-4b2a-a667-e75ff1ee4998)


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
![A52](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/7df4c8e8-94a6-463a-bfd7-53a8a0139171)


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
GROUP BY region_id;
![A53](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/5e537c85-2487-4687-ad01-8adce3c2a6c2)


