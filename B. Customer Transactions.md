# Case Study Questions

1. What is the unique count and total amount for each transaction type?
2. What is the average total historical deposit counts and amounts for all customers?
3. For each month - how many Data Bank customers make more than 1 deposit and either 1 purchase or 1 withdrawal in a single month?
4. What is the closing balance for each customer at the end of the month?
5. What is the percentage of customers who increase their closing balance by more than 5%?

### 1. What is the unique count and total amount for each transaction type?

SELECT txn_type,
       count(*) AS unique_count,
       sum(txn_amount) AS total_amont
FROM customer_transactions
GROUP BY txn_type;

![B1](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/5aa646fb-5abc-429d-8f8f-9afe874d3a00)


### 2. What is the average total historical deposit counts and amounts for all customers?

SELECT round(count(customer_id)/
               (SELECT count(DISTINCT customer_id)
                FROM customer_transactions)) AS average_deposit_count,
       concat('$', round(avg(txn_amount), 2)) AS average_deposit_amount
FROM customer_transactions
WHERE txn_type = "deposit";

![B2](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/b2527aef-fea5-48b2-84cd-4c6372777812)

### For each month - how many Data Bank customers make more than 1 deposit and either 1 purchase or 1 withdrawal in a single month?

WITH transaction_count_per_month_cte AS
  (SELECT customer_id,
          month(txn_date) AS txn_month,
          SUM(IF(txn_type="deposit", 1, 0)) AS deposit_count,
          SUM(IF(txn_type="withdrawal", 1, 0)) AS withdrawal_count,
          SUM(IF(txn_type="purchase", 1, 0)) AS purchase_count
   FROM customer_transactions
   GROUP BY customer_id,
            month(txn_date))
SELECT txn_month,
       count(DISTINCT customer_id) as customer_count
FROM transaction_count_per_month_cte
WHERE deposit_count>1
  AND (purchase_count = 1
       OR withdrawal_count = 1)
GROUP BY txn_month;

![B3](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/e5a39f06-e947-4f11-9906-31a05d9146f1)

### 4. What is the closing balance for each customer at the end of the month?

WITH txn_monthly_balance_cte AS
  (SELECT customer_id,
          txn_amount,
          month(txn_date) AS txn_month,
          SUM(CASE
                  WHEN txn_type="deposit" THEN txn_amount
                  ELSE -txn_amount
              END) AS net_transaction_amt
   FROM customer_transactions
   GROUP BY customer_id,
            month(txn_date)
   ORDER BY customer_id)
SELECT customer_id,
       txn_month,
       net_transaction_amt,
       sum(net_transaction_amt) over(PARTITION BY customer_id
                                     ORDER BY txn_month ROWS BETWEEN UNBOUNDED preceding AND CURRENT ROW) AS closing_balance
FROM txn_monthly_balance_cte;

![B4](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/321824cb-0bb3-4348-b522-5b9257e335de)

### What is the percentage of customers who increase their closing balance by more than 5%?
