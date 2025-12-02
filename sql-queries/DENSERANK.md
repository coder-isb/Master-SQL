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
