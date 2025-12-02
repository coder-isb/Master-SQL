# Dense Rank SQL Interview Solutions

## Index

### Section 1 — Top-N per Group
1. [Q1: Top 3 employees per department by salary — Microsoft](#q1-top-3-employees-per-department-by-salary-microsoft)
2. [Q2: Top-selling product per category — Amazon](#q2-top-selling-product-per-category-amazon)
3. [Q3: Top 5 most viewed posts per creator — Facebook](#q3-top-5-most-viewed-posts-per-creator-facebook)
4. [Q4: Top 3 movies per genre by rating — Netflix](#q4-top-3-movies-per-genre-by-rating-netflix)
5. [Q5: Top 2 athletes per event — Google](#q5-top-2-athletes-per-event-google)
6. [Q6: Top earning driver per city — Uber](#q6-top-earning-driver-per-city-uber)
7. [Q7: Top rated restaurant per cuisine — Yelp](#q7-top-rated-restaurant-per-cuisine-yelp)

### Section 2 — First / Last Records with Ties
8. [Q8: First purchase(s) per customer (earliest, may tie) — Amazon](#q8-first-purchases-per-customer-earliest-may-tie-amazon)
9. [Q9: Latest subscription(s) per user — Netflix](#q9-latest-subscriptions-per-user-netflix)
10. [Q10: First version(s) of each document — Google Docs](#q10-first-versions-of-each-document-google-docs)
11. [Q11: Last comment(s) before closing a ticket — Google](#q11-last-comments-before-closing-a-ticket-google)
12. [Q12: First failed login attempt(s) per user — Facebook](#q12-first-failed-login-attempts-per-user-facebook)

### Section 3 — Ranking with Ties
13. [Q13: Rank employees by tenure — Microsoft](#q13-rank-employees-by-tenure-microsoft)
14. [Q14: Rank customers by lifetime value — Amazon](#q14-rank-customers-by-lifetime-value-amazon)
15. [Q15: Rank stocks by weekly return per sector — Google Finance](#q15-rank-stocks-by-weekly-return-per-sector-google-finance)
16. [Q16: Rank users by activity score per country — Facebook](#q16-rank-users-by-activity-score-per-country-facebook)
17. [Q17: Rank items by popularity per category — Amazon](#q17-rank-items-by-popularity-per-category-amazon)
18. [Q18: Rank drivers by number of trips per city — Uber](#q18-rank-drivers-by-number-of-trips-per-city-uber)
19. [Q19: Rank athletes by performance per event — Google](#q19-rank-athletes-by-performance-per-event-google)

### Section 4 — Hard / Product Analytics
20. [Q20: First fraudulent transaction(s) per user — PayPal](#q20-first-fraudulent-transactions-per-user-paypal)
21. [Q21: Top 3 most recent promotions per employee with ties — Microsoft](#q21-top-3-most-recent-promotions-per-employee-with-ties-microsoft)
22. [Q22: Product with greatest day-over-day price increase per category — Amazon](#q22-product-with-greatest-day-over-day-price-increase-per-category-amazon)
23. [Q23: Daily anomalies where traffic increased >200% — Google](#q23-daily-anomalies-where-traffic-increased-200-google)
24. [Q24: Detect overlapping bookings per host — Airbnb](#q24-detect-overlapping-bookings-per-host-airbnb)

---

## Section 1 — Top-N per Group

## Q1: Top 3 employees per department by salary — Microsoft
```sql
SELECT *,
       DENSE_RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS dr
FROM employees
WHERE dr <= 3;

Q2: Top-selling product per category — Amazon
SELECT *,
       DENSE_RANK() OVER(PARTITION BY category ORDER BY total_sales DESC) AS dr
FROM products
WHERE dr = 1;


Explanation: Picks all top-selling products, even if multiple have the same sales.

Q3: Top 5 most viewed posts per creator — Facebook
SELECT *,
       DENSE_RANK() OVER(PARTITION BY creator_id ORDER BY views DESC) AS dr
FROM posts
WHERE dr <= 5;


Explanation: Handles ties in views to include multiple posts at same rank.

Q4: Top 3 movies per genre by rating — Netflix
SELECT *,
       DENSE_RANK() OVER(PARTITION BY genre ORDER BY rating DESC) AS dr
FROM movies
WHERE dr <= 3;


Explanation: Includes all movies tied at top 3 ranks per genre.

Q5: Top 2 athletes per event — Google
SELECT *,
       DENSE_RANK() OVER(PARTITION BY event ORDER BY performance DESC) AS dr
FROM athletes
WHERE dr <= 2;


Explanation: Multiple athletes with same score are ranked the same.

Q6: Top earning driver per city — Uber
SELECT *,
       DENSE_RANK() OVER(PARTITION BY city ORDER BY revenue DESC) AS dr
FROM drivers
WHERE dr = 1;


Explanation: Includes all drivers tied for top revenue.

Q7: Top rated restaurant per cuisine — Yelp
SELECT *,
       DENSE_RANK() OVER(PARTITION BY cuisine ORDER BY rating DESC) AS dr
FROM restaurants
WHERE dr = 1;


Explanation: Handles ties in ratings, returning all highest-rated restaurants.

Section 2 — First / Last Records with Ties
Q8: First purchase(s) per customer (earliest, may tie) — Amazon
SELECT *,
       DENSE_RANK() OVER(PARTITION BY customer_id ORDER BY purchase_date ASC) AS dr
FROM purchases
WHERE dr = 1;


Explanation: If multiple purchases happened at the same earliest timestamp, all are included.

Q9: Latest subscription(s) per user — Netflix
SELECT *,
       DENSE_RANK() OVER(PARTITION BY user_id ORDER BY subscription_date DESC) AS dr
FROM subscriptions
WHERE dr = 1;


Explanation: Includes all subscriptions starting on the latest date.

Q10: First version(s) of each document — Google Docs
SELECT *,
       DENSE_RANK() OVER(PARTITION BY doc_id ORDER BY version ASC) AS dr
FROM documents
WHERE dr = 1;


Explanation: Multiple documents could have the same earliest version number (tie).

Q11: Last comment(s) before closing a ticket — Google
SELECT *,
       DENSE_RANK() OVER(PARTITION BY ticket_id ORDER BY comment_time DESC) AS dr
FROM ticket_comments
WHERE dr = 1;


Explanation: Handles simultaneous last comments.

Q12: First failed login attempt(s) per user — Facebook
SELECT *,
       DENSE_RANK() OVER(PARTITION BY user_id ORDER BY login_time ASC) AS dr
FROM logins
WHERE status='failed' AND dr = 1;


Explanation: Includes all first failed attempts if multiple happened at the same time.

Section 3 — Ranking with Ties
Q13: Rank employees by tenure — Microsoft
SELECT *,
       DENSE_RANK() OVER(ORDER BY hire_date ASC) AS dr
FROM employees;


Explanation: Employees hired on same day have same rank.

Q14: Rank customers by lifetime value — Amazon
SELECT *,
       DENSE_RANK() OVER(ORDER BY lifetime_value DESC) AS dr
FROM customers;


Explanation: Customers with same total spend get same rank.

Q15: Rank stocks by weekly return per sector — Google Finance
SELECT *,
       DENSE_RANK() OVER(PARTITION BY sector ORDER BY weekly_return DESC) AS dr
FROM stocks;


Explanation: Stocks with same weekly return share rank.

Q16: Rank users by activity score per country — Facebook
SELECT *,
       DENSE_RANK() OVER(PARTITION BY country ORDER BY activity_score DESC) AS dr
FROM users;


Explanation: Handles ties in activity scores.

Q17: Rank items by popularity per category — Amazon
SELECT *,
       DENSE_RANK() OVER(PARTITION BY category ORDER BY sales DESC) AS dr
FROM products;


Explanation: Products with same sales count share the same rank.

Q18: Rank drivers by number of trips per city — Uber
SELECT *,
       DENSE_RANK() OVER(PARTITION BY city ORDER BY trip_count DESC) AS dr
FROM drivers;


Explanation: Ties in trip counts do not skip ranks.

Q19: Rank athletes by performance per event — Google
SELECT *,
       DENSE_RANK() OVER(PARTITION BY event ORDER BY performance DESC) AS dr
FROM athletes;


Explanation: Multiple athletes with same performance get same rank.

Section 4 — Hard / Product Analytics
Q20: First fraudulent transaction(s) per user — PayPal
SELECT *,
       DENSE_RANK() OVER(PARTITION BY user_id ORDER BY transaction_time ASC) AS dr
FROM transactions
WHERE fraud_flag = TRUE AND dr = 1;


Explanation: Includes all first fraudulent transactions per user.

Q21: Top 3 most recent promotions per employee with ties — Microsoft
SELECT *,
       DENSE_RANK() OVER(PARTITION BY emp_id ORDER BY promo_date DESC) AS dr
FROM promotions
WHERE dr <= 3;


Explanation: Includes ties; top 3 recent promotions per employee.

Q22: Product with greatest day-over-day price increase per category — Amazon
SELECT *,
       DENSE_RANK() OVER(PARTITION BY category ORDER BY price_diff DESC) AS dr
FROM product_prices
WHERE dr = 1;


Explanation: Picks the product(s) with the highest day-over-day increase per category.

Q23: Daily anomalies where traffic increased >200% — Google
SELECT *,
       DENSE_RANK() OVER(PARTITION BY page_id ORDER BY pct_increase DESC) AS dr
FROM traffic
WHERE dr = 1;


Explanation: Identifies pages with extreme traffic increases; includes ties.

Q24: Detect overlapping bookings per host — Airbnb
SELECT *,
       DENSE_RANK() OVER(PARTITION BY host_id ORDER BY start_date ASC) AS dr
FROM bookings;


Explanation: Includes ties to detect simultaneous or overlapping bookings.


---
