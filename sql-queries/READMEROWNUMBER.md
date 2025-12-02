# SQL Query Collection

This repository contains 60 SQL queries grouped into 6 sections. Each query solves a specific problem related to deduplication, ranking, analytics, or complex FAANG-level challenges using SQL window functions and other techniques.

---

## Index

### ⭐ Section 1 — Remove Duplicates (10 Queries)
- [Q1: Remove duplicate customers, keep earliest signup (Amazon)](#q1-remove-duplicate-customers-keep-earliest-signup-amazon)
- [Q2: Deduplicate product catalog, keeping cheapest version (Walmart, Amazon Retail)](#q2-deduplicate-product-catalog-keeping-cheapest-version-walmart-amazon-retail)
- [Q3: Deduplicate payment attempts, keep most recent success (Stripe, PayPal)](#q3-deduplicate-payment-attempts-keep-most-recent-success-stripe-paypal)
- [Q4: Deduplicate transactions where same amount appears twice (Uber)](#q4-deduplicate-transactions-where-same-amount-appears-twice-uber)
- [Q5: Deduplicate rows in a clickstream log (Meta)](#q5-deduplicate-rows-in-a-clickstream-log-meta)
- [Q6: Keep latest price record for each product (Amazon Pricing)](#q6-keep-latest-price-record-for-each-product-amazon-pricing)
- [Q7: Deduplicate addresses, keep first verified (Google Maps)](#q7-deduplicate-addresses-keep-first-verified-google-maps)
- [Q8: Keep last job title of each employee before termination (LinkedIn)](#q8-keep-last-job-title-of-each-employee-before-termination-linkedin)
- [Q9: Deduplicate survey responses, keep the latest per user (Google UX Research)](#q9-deduplicate-survey-responses-keep-the-latest-per-user-google-ux-research)
- [Q10: Deduplicate device logs, keep newest record (Apple)](#q10-deduplicate-device-logs-keep-newest-record-apple)

### ⭐ Section 2 — Top-N Per Group (10 Queries)
- [Q11: Top 3 orders per customer by amount (Amazon)](#q11-top-3-orders-per-customer-by-amount-amazon)
- [Q12: Top 2 highest-rated movies per genre (Netflix)](#q12-top-2-highest-rated-movies-per-genre-netflix)
- [Q13: Top 5 most viewed posts per creator (Instagram)](#q13-top-5-most-viewed-posts-per-creator-instagram)
- [Q14: Top 1 earning driver per city (Uber)](#q14-top-1-earning-driver-per-city-uber)
- [Q15: Top 10 bestselling products per category (Amazon Marketplace)](#q15-top-10-bestselling-products-per-category-amazon-marketplace)
- [Q16: Top 3 busiest stations per city (Lyft)](#q16-top-3-busiest-stations-per-city-lyft)
- [Q17: Find the most active volunteer per event (Airbnb Social Impact)](#q17-find-the-most-active-volunteer-per-event-airbnb-social-impact)
- [Q18: Top 2 highest scoring students per class (Google Education)](#q18-top-2-highest-scoring-students-per-class-google-education)
- [Q19: Return only the longest call per user (Zoom)](#q19-return-only-the-longest-call-per-user-zoom)
- [Q20: Find top restaurant per cuisine by rating (Yelp)](#q20-find-top-restaurant-per-cuisine-by-rating-yelp)

### ⭐ Section 3 — First & Last Records (10 Queries)
- [Q21: Get first purchase of each customer (Amazon)](#q21-get-first-purchase-of-each-customer-amazon)
- [Q22: Get last login per user (Meta)](#q22-get-last-login-per-user-meta)
- [Q23: First version of each document (Google Docs)](#q23-first-version-of-each-document-google-docs)
- [Q24: Latest salary record per employee (LinkedIn)](#q24-latest-salary-record-per-employee-linkedin)
- [Q25: Most recent subscription plan per user (Spotify)](#q25-most-recent-subscription-plan-per-user-spotify)
- [Q26: First country visited per traveler (Airbnb)](#q26-first-country-visited-per-traveler-airbnb)
- [Q27: Last comment before closing a ticket (Zendesk)](#q27-last-comment-before-closing-a-ticket-zendesk)
- [Q28: Get first job title of each employee (Workday)](#q28-get-first-job-title-of-each-employee-workday)
- [Q29: Latest product price (Amazon Retail)](#q29-latest-product-price-amazon-retail)
- [Q30: First failed login attempt per user (Security)](#q30-first-failed-login-attempt-per-user-security)

### ⭐ Section 4 — Ranking Problems (10 Queries)
- [Q31: Rank employees by salary per department (Microsoft)](#q31-rank-employees-by-salary-per-department-microsoft)
- [Q32: Rank customers by their lifetime value (Amazon Prime)](#q32-rank-customers-by-their-lifetime-value-amazon-prime)
- [Q33: Rank stocks by weekly return (Google Finance)](#q33-rank-stocks-by-weekly-return-google-finance)
- [Q34: Rank users by activity score per country (Meta)](#q34-rank-users-by-activity-score-per-country-meta)
- [Q35: Rank athletes by performance per event (Olympics)](#q35-rank-athletes-by-performance-per-event-olympics)
- [Q36: Rank items by popularity per category (Pinterest)](#q36-rank-items-by-popularity-per-category-pinterest)
- [Q37: Rank cities by pollution index (Google Earth)](#q37-rank-cities-by-pollution-index-google-earth)
- [Q38: Rank videos by watch time per creator (YouTube)](#q38-rank-videos-by-watch-time-per-creator-youtube)
- [Q39: Rank employees by tenure (Workday)](#q39-rank-employees-by-tenure-workday)
- [Q40: Rank articles by number of shares (LinkedIn)](#q40-rank-articles-by-number-of-shares-linkedin)

### ⭐ Section 5 — Product / Behavior Analytics (10 Queries)
- [Q41: Identify users with 7-day login streak (Meta)](#q41-identify-users-with-7-day-login-streak-meta)
- [Q42: Find the first search after opening the app (Google)](#q42-find-the-first-search-after-opening-the-app-google)
- [Q43: Detect first subscription cancellation per user (Spotify)](#q43-detect-first-subscription-cancellation-per-user-spotify)
- [Q44: Find user's first watched movie in each year (Netflix)](#q44-find-users-first-watched-movie-in-each-year-netflix)
- [Q45: Get each shopper’s first cart abandon event (Amazon)](#q45-get-each-shoppers-first-cart-abandon-event-amazon)
- [Q46: Detect first unusual spike in search volume per user (Google Search)](#q46-detect-first-unusual-spike-in-search-volume-per-user-google-search)
- [Q47: Identify first time a user changed device (Apple iCloud)](#q47-identify-first-time-a-user-changed-device-apple-icloud)
- [Q48: Detect the first rage-click event per user (UX Analytics)](#q48-detect-the-first-rage-click-event-per-user-ux-analytics)
- [Q49: First subscription upgrade per user (Amazon Prime)](#q49-first-subscription-upgrade-per-user-amazon-prime)
- [Q50: First driver cancellation per trip (Uber)](#q50-first-driver-cancellation-per-trip-uber)

### ⭐ Section 6 — Hard FAANG Questions (10 Queries)
- [Q51: Detect overlapping bookings per host, return first overlap (Airbnb)](#q51-detect-overlapping-bookings-per-host-return-first-overlap-airbnb)
- [Q52: First fraudulent transaction per user (Stripe)](#q52-first-fraudulent-transaction-per-user-stripe)
- [Q53: Detect first time a stock hits a new all-time high (Bloomberg, Google)](#q53-detect-first-time-a-stock-hits-a-new-all-time-high-bloomberg-google)
- [Q54: Identify the 3 most recent promotions per employee (Workday)](#q54-identify-the-3-most-recent-promotions-per-employee-workday)
- [Q55: Detect daily anomalies where traffic increased > 200% (Cloudflare)](#q55-detect-daily-anomalies-where-traffic-increased--200-cloudflare)
- [Q56: Identify product with greatest day-over-day price increase per category (Amazon Pricing)](#q56-identify-product-with-greatest-day-over-day-price-increase-per-category-amazon-pricing)
- [Q57: Detect first repeating user behavior pattern (Meta ML)](#q57-detect-first-repeating-user-behavior-pattern-meta-ml)
- [Q58: Identify users who returned within 7 days after first visit (Google Ads)](#q58-identify-users-who-returned-within-7-days-after-first-visit-google-ads)
- [Q59: First cart add after viewing a product (Amazon Retail)](#q59-first-cart-add-after-viewing-a-product-amazon-retail)
- [Q60: Identify employees who got demoted (Netflix)](#q60-identify-employees-who-got-demoted-netflix)

---

## Section 1 — Remove Duplicates

### Q1: Remove duplicate customers, keep earliest signup (Amazon)
```sql
WITH c AS (
    SELECT *,
           ROW_NUMBER() OVER(
               PARTITION BY email
               ORDER BY signup_date
           ) AS rn
    FROM customers
)
SELECT *
FROM c
WHERE rn = 1;

