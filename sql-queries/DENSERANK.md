
# SQL Interview Solutions

## Index

### Section 1 — Deduplication
1. [Q1: Remove duplicate customers, keep earliest signup (Amazon)](#q1-remove-duplicate-customers-keep-earliest-signup-amazon)
2. [Q2: Deduplicate product catalog, keeping cheapest version (Walmart, Amazon Retail)](#q2-deduplicate-product-catalog-keeping-cheapest-version-walmart-amazon-retail)
3. [Q3: Deduplicate payment attempts, keep most recent success (Stripe, PayPal)](#q3-deduplicate-payment-attempts-keep-most-recent-success-stripe-paypal)
4. [Q4: Deduplicate transactions where same amount appears twice (Uber)](#q4-deduplicate-transactions-where-same-amount-appears-twice-uber)
...
### Section 2 — Top-N per Group
11. [Q11: Top 3 orders per customer by amount (Amazon)](#q11-top-3-orders-per-customer-by-amount-amazon)
12. [Q12: Top 2 highest-rated movies per genre (Netflix)](#q12-top-2-highest-rated-movies-per-genre-netflix)
...

### Section 3 — First & Last Records
21. [Q21: Get first purchase of each customer (Amazon)](#q21-get-first-purchase-of-each-customer-amazon)
22. [Q22: Get last login per user (Meta)](#q22-get-last-login-per-user-meta)
...

## Section 1 — Deduplication

## Q1: Remove duplicate customers, keep earliest signup (Amazon)
```sql
WITH c AS (
    SELECT *,
           ROW_NUMBER() OVER(PARTITION BY email ORDER BY signup_date) AS rn
    FROM customers
)
SELECT * 
FROM c 
WHERE rn = 1;
