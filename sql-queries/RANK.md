# SQL Rank/Dense_Rank/Top-N Questions

## Index

### Section 1 — Dense Rank / Top-N Queries
1. [Top 3 employees per department by salary — Microsoft](#q1-top-3-employees-per-department-by-salary)
2. [Top-selling product per category — Amazon](#q2-top-selling-product-per-category)
3. [Top 5 most viewed posts per creator — Facebook](#q3-top-5-most-viewed-posts-per-creator)
4. [Top 3 movies per genre by rating — Netflix](#q4-top-3-movies-per-genre-by-rating)
5. [Top 2 athletes per event — Google](#q5-top-2-athletes-per-event)
6. [Top earning driver per city — Uber](#q6-top-earning-driver-per-city)
7. [Top rated restaurant per cuisine — Yelp](#q7-top-rated-restaurant-per-cuisine)

### Section 2 — First / Last Records With Ties
8. [First purchase(s) per customer (earliest, may tie) — Amazon](#q8-first-purchase-s-per-customer-earliest-may-tie)
9. [Latest subscription(s) per user — Netflix](#q9-latest-subscription-s-per-user)
10. [First version(s) of each document — Google Docs](#q10-first-version-s-of-each-document)
11. [Last comment(s) before closing a ticket — Google](#q11-last-comment-s-before-closing-a-ticket)
12. [First failed login attempt(s) per user — Facebook](#q12-first-failed-login-attempt-s-per-user)

### Section 3 — Ranking With Ties
13. [Rank employees by tenure — Microsoft](#q13-rank-employees-by-tenure)
14. [Rank customers by lifetime value — Amazon](#q14-rank-customers-by-lifetime-value)
15. [Rank stocks by weekly return per sector — Google Finance](#q15-rank-stocks-by-weekly-return-per-sector)
16. [Rank users by activity score per country — Facebook](#q16-rank-users-by-activity-score-per-country)
17. [Rank items by popularity per category — Amazon](#q17-rank-items-by-popularity-per-category)
18. [Rank drivers by number of trips per city — Uber](#q18-rank-drivers-by-number-of-trips-per-city)
19. [Rank athletes by performance per event — Google](#q19-rank-athletes-by-performance-per-event)

### Section 4 — Hard / Product Analytics
20. [First fraudulent transaction(s) per user — PayPal](#q20-first-fraudulent-transaction-s-per-user)
21. [Top 3 most recent promotions per employee with ties — Microsoft](#q21-top-3-most-recent-promotions-per-employee-with-ties)
22. [Product with greatest day-over-day price increase per category — Amazon](#q22-product-with-greatest-day-over-day-price-increase-per-category)
23. [Daily anomalies where traffic increased >200% — Google](#q23-daily-anomalies-where-traffic-increased-200)
24. [Detect overlapping bookings per host — Airbnb](#q24-detect-overlapping-bookings-per-host)

### Section 5 — Competition Ranking (Most Asked in FAANG)
25. [Rank employees by salary within job title — Uber/LinkedIn](#q25-rank-employees-by-salary-within-job-title)
26. [Rank users by monthly spending per city — DoorDash/Zomato](#q26-rank-users-by-monthly-spending-per-city)
27. [Rank players by score in each match — Gaming](#q27-rank-players-by-score-in-each-match)
28. [Rank stores by quarterly revenue — Service/Product](#q28-rank-stores-by-quarterly-revenue)
29. [Rank items by defect rate per factory — Manufacturing](#q29-rank-items-by-defect-rate-per-factory)
30. [Rank hotels by occupancy rate per city — Airbnb/Booking](#q30-rank-hotels-by-occupancy-rate-per-city)
31. [Rank support tickets by severity per team — Service](#q31-rank-support-tickets-by-severity-per-team)
32. [Rank doctors by patient satisfaction per department — Healthcare](#q32-rank-doctors-by-patient-satisfaction-per-department)
33. [Rank stocks by weekly return per sector — Bloomberg/Goldman](#q33-rank-stocks-by-weekly-return-per-sector)
34. [Rank product reviews by helpful votes per product — Amazon](#q34-rank-product-reviews-by-helpful-votes-per-product)

### Section 6 — Leaderboard / Scoring Systems
35. [Global leaderboard by total score — Meta/Google](#q35-global-leaderboard-by-total-score)
36. [Rank creators by total watch hours — YouTube](#q36-rank-creators-by-total-watch-hours)
37. [Rank employees by yearly performance score — Service-based](#q37-rank-employees-by-yearly-performance-score)
38. [Rank customers by number of orders last year — E-commerce](#q38-rank-customers-by-number-of-orders-last-year)
39. [Rank products by conversion rate — Amazon PM SQL](#q39-rank-products-by-conversion-rate)
40. [Rank restaurants by average rating — Yelp](#q40-rank-restaurants-by-average-rating)

---

## Q1: Top 3 employees per department by salary
```sql
SELECT *,
       DENSE_RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS dr
FROM employees
WHERE dr <= 3;

Q2: Top-selling product per category
SELECT *,
       DENSE_RANK() OVER(PARTITION BY category ORDER BY total_sales DESC) AS dr
FROM products
WHERE dr = 1;


Explanation: Picks all top-selling products, including ties.

Q3: Top 5 most viewed posts per creator
SELECT *,
       DENSE_RANK() OVER(PARTITION BY creator_id ORDER BY views DESC) AS dr
FROM posts
WHERE dr <= 5;


Explanation: Handles ties in views; includes all posts with same rank.

Q4: Top 3 movies per genre by rating
SELECT *,
       DENSE_RANK() OVER(PARTITION BY genre ORDER BY rating DESC) AS dr
FROM movies
WHERE dr <= 3;


Explanation: Includes all movies tied at top 3 ranks per genre.

Q5: Top 2 athletes per event
SELECT *,
       DENSE_RANK() OVER(PARTITION BY event ORDER BY performance DESC) AS dr
FROM athletes
WHERE dr <= 2;


Explanation: Multiple athletes with same performance get same rank.

Q6: Top earning driver per city
SELECT *,
       DENSE_RANK() OVER(PARTITION BY city ORDER BY revenue DESC) AS dr
FROM drivers
WHERE dr = 1;


Explanation: Includes all drivers tied for top revenue.

Q7: Top rated restaurant per cuisine
SELECT *,
       DENSE_RANK() OVER(PARTITION BY cuisine ORDER BY rating DESC) AS dr
FROM restaurants
WHERE dr = 1;


Explanation: Returns all top-rated restaurants per cuisine, handles ties.

Q8: First purchase(s) per customer
SELECT *,
       DENSE_RANK() OVER(PARTITION BY customer_id ORDER BY purchase_date ASC) AS dr
FROM purchases
WHERE dr = 1;


Explanation: Includes all earliest purchases per customer.

Q9: Latest subscription(s) per user
SELECT *,
       DENSE_RANK() OVER(PARTITION BY user_id ORDER BY subscription_date DESC) AS dr
FROM subscriptions
WHERE dr = 1;


Explanation: Returns all subscriptions starting on the latest date.

Q10: First version(s) of each document
SELECT *,
       DENSE_RANK() OVER(PARTITION BY doc_id ORDER BY version ASC) AS dr
FROM documents
WHERE dr = 1;


Explanation: Multiple documents may have same earliest version.

Q11: Last comment(s) before closing a ticket
SELECT *,
       DENSE_RANK() OVER(PARTITION BY ticket_id ORDER BY comment_time DESC) AS dr
FROM ticket_comments
WHERE dr = 1;


Explanation: Handles multiple simultaneous last comments.

Q12: First failed login attempt(s) per user
SELECT *,
       DENSE_RANK() OVER(PARTITION BY user_id ORDER BY login_time ASC) AS dr
FROM logins
WHERE status='failed' AND dr = 1;


Explanation: Includes all first failed attempts if multiple occurred simultaneously.

Q13: Rank employees by tenure
SELECT *,
       DENSE_RANK() OVER(ORDER BY hire_date ASC) AS dr
FROM employees;


Explanation: Employees hired on same day share same rank.

Q14: Rank customers by lifetime value
SELECT *,
       DENSE_RANK() OVER(ORDER BY lifetime_value DESC) AS dr
FROM customers;


Explanation: Customers with same spend get same rank.

Q15: Rank stocks by weekly return per sector
SELECT *,
       DENSE_RANK() OVER(PARTITION BY sector ORDER BY weekly_return DESC) AS dr
FROM stocks;


Explanation: Stocks with same weekly return share rank.

Q16: Rank users by activity score per country
SELECT *,
       DENSE_RANK() OVER(PARTITION BY country ORDER BY activity_score DESC) AS dr
FROM users;


Explanation: Ties in activity score handled per country.

Q17: Rank items by popularity per category
SELECT *,
       DENSE_RANK() OVER(PARTITION BY category ORDER BY sales DESC) AS dr
FROM products;


Explanation: Items with same sales share same rank.

Q18: Rank drivers by number of trips per city
SELECT *,
       DENSE_RANK() OVER(PARTITION BY city ORDER BY trip_count DESC) AS dr
FROM drivers;


Explanation: Ties in trip counts do not skip ranks.

Q19: Rank athletes by performance per event
SELECT *,
       DENSE_RANK() OVER(PARTITION BY event ORDER BY performance DESC) AS dr
FROM athletes;


Explanation: Multiple athletes with same performance get same rank.

Q20: First fraudulent transaction(s) per user
SELECT *,
       DENSE_RANK() OVER(PARTITION BY user_id ORDER BY transaction_time ASC) AS dr
FROM transactions
WHERE fraud_flag = TRUE AND dr = 1;


Explanation: Includes all first fraudulent transactions per user.

Q21: Top 3 most recent promotions per employee
SELECT *,
       DENSE_RANK() OVER(PARTITION BY emp_id ORDER BY promo_date DESC) AS dr
FROM promotions
WHERE dr <= 3;


Explanation: Returns top 3 promotions per employee; ties included.

Q22: Product with greatest day-over-day price increase per category
SELECT *,
       DENSE_RANK() OVER(PARTITION BY category ORDER BY price_diff DESC) AS dr
FROM product_prices
WHERE dr = 1;


Explanation: Returns all products tied for max price increase.

Q23: Daily anomalies where traffic increased >200%
SELECT *,
       DENSE_RANK() OVER(PARTITION BY page_id ORDER BY pct_increase DESC) AS dr
FROM traffic
WHERE dr = 1;


Explanation: Returns all pages with top % increase per day.

Q24: Detect overlapping bookings per host
SELECT *,
       DENSE_RANK() OVER(PARTITION BY host_id ORDER BY start_date ASC) AS dr
FROM bookings;


Explanation: Detects overlapping or simultaneous bookings.

Q25: Rank employees by salary within job title
SELECT employee_id, job_title, salary,
       RANK() OVER(PARTITION BY job_title ORDER BY salary DESC) AS rnk
FROM employee;


Explanation: Employees ranked within job title by salary; ties get same rank.

Q26: Rank users by monthly spending per city
SELECT city, user_id, monthly_spend,
       RANK() OVER(PARTITION BY city ORDER BY monthly_spend DESC) AS rnk
FROM spend;


Explanation: Ranks users per city by spending; ties included.

Q27: Rank players by score in each match
SELECT match_id, player_id, score,
       RANK() OVER(PARTITION BY match_id ORDER BY score DESC) AS rnk
FROM match_scores;


Explanation: Player ranking per match; ties handled.

Q28: Rank stores by quarterly revenue
SELECT store_id, quarter, revenue,
       RANK() OVER(PARTITION BY quarter ORDER BY revenue DESC) AS rnk
FROM revenue;


Explanation: Stores ranked per quarter; ties handled.

Q29: Rank items by defect rate per factory
SELECT factory_id, item_id, defect_rate,
       RANK() OVER(PARTITION BY factory_id ORDER BY defect_rate DESC) AS rnk
FROM quality;


Explanation: Items ranked per factory; ties included.

Q30: Rank hotels by occupancy rate per city
SELECT city, hotel_id, occupancy,
       RANK() OVER(PARTITION BY city ORDER BY occupancy DESC) AS rnk
FROM hotels;


Explanation: Hotels ranked per city; ties included.

Q31: Rank support tickets by severity per team
SELECT team, ticket_id, severity,
       RANK() OVER(PARTITION BY team ORDER BY severity DESC) AS rnk
FROM tickets;


Explanation: Tickets ranked per team; multiple same-severity tickets share rank.

Q32: Rank doctors by patient satisfaction per department
SELECT dept, doctor_id, rating,
       RANK() OVER(PARTITION BY dept ORDER BY rating DESC) AS rnk
FROM doctor_ratings;


Explanation: Doctors ranked per department by rating; ties included.

Q33: Rank stocks by weekly return per sector
SELECT sector, stock, weekly_return,
       RANK() OVER(PARTITION BY sector ORDER BY weekly_return DESC) AS rnk
FROM stock_returns;


Explanation: Stocks ranked per sector; ties handled.

Q34: Rank product reviews by helpful votes per product
SELECT product_id, review_id, helpful_votes,
       RANK() OVER(PARTITION BY product_id ORDER BY helpful_votes DESC) AS rnk
FROM reviews;


Explanation: Reviews ranked per product by helpful votes; ties included.

Q35: Global leaderboard by total score
WITH t AS (
    SELECT player_id, SUM(score) AS total_score
    FROM game_scores
    GROUP BY player_id
)
SELECT *,
       RANK() OVER(ORDER BY total_score DESC) AS rnk
FROM t;


Explanation: Leaderboard for players; ties handled.

Q36: Rank creators by total watch hours
SELECT creator_id, SUM(watch_time) AS wt,
       RANK() OVER(ORDER BY SUM(watch_time) DESC) AS rnk
FROM videos
GROUP BY creator_id;


Explanation: Creators ranked globally by total watch hours; ties included.

Q37: Rank employees by yearly performance score
SELECT employee_id, year_score,
       RANK() OVER(ORDER BY year_score DESC) AS rnk
FROM performance;


Explanation: Employees ranked by annual performance; ties included.

Q38: Rank customers by number of orders last year
WITH t AS (
    SELECT customer_id, COUNT(*) AS cnt
    FROM orders
    WHERE order_date >= CURRENT_DATE - INTERVAL '365 days'
    GROUP BY customer_id
)
SELECT *,
       RANK() OVER(ORDER BY cnt DESC) AS rnk
FROM t;


Explanation: Customers ranked by order count; ties included.

Q39: Rank products by conversion rate
SELECT product_id,
       purchases * 1.0 / clicks AS conversion_rate,
       RANK() OVER(ORDER BY purchases * 1.0 / clicks DESC) AS rnk
FROM metrics;


Explanation: Products ranked by conversion; ties handled.

Q40: Rank restaurants by average rating
SELECT restaurant_id,
       AVG(rating) AS avg_rating,
       RANK() OVER(ORDER BY AVG(rating) DESC) AS rnk
FROM restaurant_reviews
GROUP BY restaurant_id;
