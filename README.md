
# SQL-8-Week-Challange-Case-study-4

Introducttion

There is a new innovation in the financial industry called Neo-Banks: new aged digital only banks without physical branches.

Danny thought that there should be some sort of intersection between these new age banks, cryptocurrency and the data world…so he decides to launch a new initiative - Data Bank!

Data Bank runs just like any other digital bank - but it isn’t only for banking activities, they also have the world’s most secure distributed data storage platform!

Customers are allocated cloud data storage limits which are directly linked to how much money they have in their accounts. There are a few interesting caveats that go with this business model, and this is where the Data Bank team need your help!

The management team at Data Bank want to increase their total customer base - but also need some help tracking just how much data storage their customers will need.

This case study is all about calculating metrics, growth and helping the business analyse their data in a smart way to better forecast and plan for their future developments!

## Data

The Data Bank team have prepared a data model for this case study as well as a few example rows from the complete dataset below to get you familiar with their tables.


![case-study-4-erd](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/ab269bad-40c9-4e49-9951-a7bfd7f9c7e3)

Table 1: Regions

Just like popular cryptocurrency platforms - Data Bank is also run off a network of nodes where both money and data is stored across the globe. In a traditional banking sense - you can think of these nodes as bank branches or stores that exist around the world.

This regions table contains the region_id and their respective region_name values 


![table 1](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/1b0247b3-8b61-4f19-83d1-cb4989a41f6f)

Table 2: Customer Nodes

Customers are randomly distributed across the nodes according to their region - this also specifies exactly which node contains both their cash and data.

This random distribution changes frequently to reduce the risk of hackers getting into Data Bank’s system and stealing customer’s money and data!

Below is a sample of the top 10 rows of the data_bank.customer_nodes

![table 2](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/b41fcaaa-c0a3-4e44-b2ce-1e95898ad7bb)

Table 3: Customer Transactions

This table stores all customer deposits, withdrawals and purchases made using their Data Bank debit card.

![table 3](https://github.com/SaamarthMeston/SQL-8-Week-Challange-Case-study-4/assets/111190817/5d4b40f1-7bea-4179-b441-e32d4b8bb1bd)


