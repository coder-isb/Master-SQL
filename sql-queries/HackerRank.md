
# HackerRank SQL Queries Cheat Sheet (1-40)

## Index

1. [Get Employee Name](#1-get-employee-name)  
2. [Get Employee Salary](#2-get-employee-salary)  
3. [Query Name of Students with Marks > 75](#3-query-name-of-students-with-marks--75)  
4. [WEATHER-3: List CITY names with even ID](#4-weather-3-list-city-names-with-even-id)  
5. [WEATHER-4: Difference between total and distinct CITY entries](#5-weather-4-difference-between-total-and-distinct-city-entries)  
6. [WEATHER-5: Two cities with shortest and longest names](#6-weather-5-two-cities-with-shortest-and-longest-names)  
7. [WEATHER-6: CITY names starting with vowels](#7-weather-6-city-names-starting-with-vowels)  
8. [WEATHER-7: CITY names ending with vowels](#8-weather-7-city-names-ending-with-vowels)  
9. [BINARY TREE: Root, Inner, Leaf](#9-binary-tree-root-inner-leaf)  
10. [Founder > LM > SM > M > Emp: Count employees per company](#10-founder--lm--sm--m--emp-count-employees-per-company)  
11. [TRIANGLE TYPES](#11-triangle-types)  
12. [PAD CONCAT: Name + Occupation abbreviation](#12-pad-concat-name--occupation-abbreviation)  
13. [OCCUPATION Pivot](#13-occupation-pivot)  
14. [COUNT: Number of cities with population > 100000](#14-count-number-of-cities-with-population--100000)  
15. [SUM: Total population in California](#15-sum-total-population-in-california)  
16. [AVG: Average population in California](#16-avg-average-population-in-california)  
17. [Round average population](#17-round-average-population)  
18. [Population Density Difference](#18-population-density-difference)  
19. [Blunder-Miscalculation Ceil](#19-blunder-miscalculation-ceil)  
20. [Max earnings (salary * months)](#20-max-earnings-salary--months)  
21. [Round LAT_N in STATION](#21-round-lat_n-in-station)  
22. [Manhattan Distance](#22-manhattan-distance)  
23. [Euclidean Distance](#23-euclidean-distance)  
24. [Asian Population JOIN](#24-asian-population-join)  
25. [African Cities JOIN](#25-african-cities-join)  
26. [Average population per continent](#26-average-population-per-continent)  
27. [Student Report](#27-student-report)  
28. [Top Competitors](#28-top-competitors)  
29. [Challenges](#29-challenges)  
30. [Contest Leaderboard](#30-contest-leaderboard)  
31. [SQL Project Planning](#31-sql-project-planning)  
32. [Symmetric Pairs](#32-symmetric-pairs)  
33. [Print *](#33-print-)  

---

## Solutions

### 1. Get Employee Name
```sql
SELECT name FROM employee ORDER BY name;

2. Get Employee Salary
SELECT name FROM employee WHERE salary > 2000 AND months < 10 ORDER BY employee_id;

3. Query Name of Students with Marks > 75
SELECT name FROM students WHERE marks > 75 ORDER BY RIGHT(name,3), id ASC;

4. WEATHER-3: List CITY names with even ID
SELECT DISTINCT city FROM station WHERE ID % 2 = 0;

5. WEATHER-4: Difference between total and distinct CITY entries
SELECT COUNT(city) - COUNT(DISTINCT city) FROM station;

6. WEATHER-5: Two cities with shortest and longest names
SELECT city, LENGTH(city) 
FROM station 
ORDER BY LENGTH(city) ASC, city ASC 
LIMIT 1;

SELECT DISTINCT city, LENGTH(city) 
FROM station 
ORDER BY LENGTH(city) DESC, city ASC 
LIMIT 1;

7. WEATHER-6: CITY names starting with vowels
SELECT DISTINCT city 
FROM station 
WHERE LEFT(city,1) IN ('a','e','i','o','u');

8. WEATHER-7: CITY names ending with vowels
SELECT DISTINCT city 
FROM station 
WHERE RIGHT(city,1) IN ('a','e','i','o','u');

9. BINARY TREE: Root, Inner, Leaf
SELECT BT.N,
CASE
    WHEN BT.P IS NULL THEN 'Root'
    WHEN EXISTS (SELECT B.P FROM BST B WHERE B.P = BT.N) THEN 'Inner'
    ELSE 'Leaf'
END
FROM BST AS BT 
ORDER BY BT.N;

10. Founder > LM > SM > M > Emp: Count employees per company
SELECT e.company_code,
       MAX(founder),
       COUNT(DISTINCT lead_manager_code),
       COUNT(DISTINCT senior_manager_code),
       COUNT(DISTINCT manager_code),
       COUNT(DISTINCT employee_code)
FROM Employee e
LEFT JOIN Company c ON e.company_code = c.company_code
GROUP BY e.company_code
ORDER BY e.company_code;

11. TRIANGLE TYPES
SELECT CASE
    WHEN A + B <= C OR A + C <= B OR B + C <= A THEN 'Not A Triangle'
    WHEN A = B AND B = C THEN 'Equilateral'
    WHEN A = B OR A = C OR B = C THEN 'Isosceles'
    ELSE 'Scalene'
END AS triangle_sides
FROM TRIANGLES;

12. PAD CONCAT: Name + Occupation abbreviation
SELECT CONCAT(name,'(',SUBSTRING(Occupation,1,1),')') AS Name 
FROM occupations 
ORDER BY Name;

SELECT CONCAT('There are a total of ', COUNT(occupation),' ', LOWER(occupation),'s.') AS totals
FROM occupations
GROUP BY occupation
ORDER BY totals;

13. OCCUPATION Pivot
WITH doctor AS (
    SELECT name, ROW_NUMBER() OVER(ORDER BY name) AS rn
    FROM OCCUPATIONS
    WHERE occupation LIKE 'Doctor'
),
professor AS (
    SELECT name, ROW_NUMBER() OVER(ORDER BY name) AS rn
    FROM OCCUPATIONS
    WHERE occupation LIKE 'Professor'
)
...
SELECT d.name, p.name, s.name, a.name
FROM professor p
LEFT JOIN doctor d ON d.rn = p.rn
LEFT JOIN singer s ON s.rn = p.rn
LEFT JOIN actor a ON a.rn = p.rn;


Alternate:

USING PIVOT SELECT Doctor, Professor, Singer, Actor FROM (
    SELECT ROW_NUMBER() OVER (PARTITION BY occupation ORDER BY name) as rn, name, occupation 
    FROM occupations
) 
PIVOT (MAX(name) FOR occupation IN ('Doctor' as Doctor,'Professor' as Professor,'Singer' as Singer,'Actor' as Actor))
ORDER BY rn;

14. COUNT: Number of cities with population > 100000
SELECT COUNT(name) FROM city WHERE population > 100000;

15. SUM: Total population in California
SELECT SUM(population) FROM city WHERE district ='California';

16. AVG: Average population in California
SELECT AVG(population) FROM city WHERE district = 'California';

17. Round average population
SELECT ROUND(AVG(population)) FROM city;

18. Population Density Difference
SELECT MAX(population)-MIN(population) AS diff FROM city;

19. Blunder-Miscalculation Ceil
SELECT CEIL(AVG(salary) - AVG(REPLACE(salary, '0', ''))) FROM employees;

20. Max earnings (salary * months)
SELECT salary*months, COUNT(*) 
FROM employee 
WHERE salary*months=(SELECT MAX(salary*months) FROM employee)
GROUP BY salary*months;

21. Round LAT_N in STATION
SELECT ROUND(SUM(LAT_N),4)
FROM STATION
WHERE LAT_N > 38.7880 AND LAT_N < 137.2345;

22. Manhattan Distance
SELECT ROUND(
    ABS(MIN(LAT_N) - MAX(LAT_N)) + ABS(MIN(LONG_W) - MAX(LONG_W)),
    4
) AS manhattan_distance
FROM STATION;

23. Euclidean Distance
SELECT ROUND(
    SQRT(POWER(MAX(LAT_N)-MIN(LAT_N),2) + POWER(MAX(LONG_W)-MIN(LONG_W),2)),
    4
) AS euclidean_distance
FROM STATION;

24. Asian Population JOIN
SELECT SUM(city.population)
FROM country
LEFT JOIN city ON country.code = city.countrycode
WHERE country.continent = 'Asia';

25. African Cities JOIN
SELECT city.name
FROM city
JOIN country ON city.countrycode = country.code
WHERE country.continent = 'Africa';

26. Average population per continent
SELECT country.continent, FLOOR(AVG(city.population))
FROM country
INNER JOIN city ON city.countrycode = country.code
GROUP BY country.continent;

27. Student Report
SELECT CASE
         WHEN G.grade > 7 THEN S.name
         ELSE NULL
       END AS names,
       G.grade,
       S.marks
FROM students S
JOIN grades G ON S.marks BETWEEN G.min_mark AND G.max_mark
ORDER BY G.grade DESC, names ASC, S.marks ASC;


Alternate:

SELECT IF(GRADE < 8, NULL, NAME), GRADE, MARKS
FROM STUDENTS 
JOIN GRADES ON MARKS BETWEEN MIN_MARK AND MAX_MARK
ORDER BY GRADE DESC, NAME;

28. Top Competitors
SELECT H.hacker_id, H.name
FROM submissions S
JOIN challenges C ON S.challenge_id = C.challenge_id
JOIN difficulty D ON C.difficulty_level = D.difficulty_level
JOIN hackers H ON S.hacker_id = H.hacker_id
    AND S.score = D.score
GROUP BY H.hacker_id, H.name
HAVING COUNT(S.hacker_id) > 1
ORDER BY COUNT(S.hacker_id) DESC, S.hacker_id ASC;

29. Challenges
SELECT H.hacker_id, H.name, COUNT(C.challenge_id) AS total_count
FROM Hackers H
JOIN Challenges C ON H.hacker_id = C.hacker_id
GROUP BY H.hacker_id, H.name
HAVING total_count = (
    SELECT COUNT(temp1.challenge_id) AS max_count
    FROM challenges temp1
    GROUP BY temp1.hacker_id
    ORDER BY max_count DESC
    LIMIT 1
)
OR total_count IN (
    SELECT DISTINCT other_counts FROM (
        SELECT H2.hacker_id, H2.name, COUNT(C2.challenge_id) AS other_counts
        FROM Hackers H2
        JOIN Challenges C2 ON H2.hacker_id = C2.hacker_id
        GROUP BY H2.hacker_id, H2.name
    ) temp2
    GROUP BY other_counts
    HAVING COUNT(other_counts) = 1
)
ORDER BY total_count DESC, H.hacker_id;

30. Contest Leaderboard
SELECT h.hacker_id, h.name, SUM(MAX_SCORE.t1) AS total_score
FROM Hackers h
INNER JOIN (
    SELECT MAX(s.score) AS t1, s.hacker_id
    FROM Submissions s
    GROUP BY s.challenge_id, s.hacker_id
    HAVING t1 > 0
) AS MAX_SCORE
ON h.hacker_id = MAX_SCORE.hacker_id
GROUP BY h.hacker_id, h.name
ORDER BY total_score DESC, hacker_id ASC;

31. SQL Project Planning
SELECT s.Proj_Start_Date, MIN(e.Proj_End_Date) AS Real_Proj_End_Date
FROM (
    SELECT Start_Date AS Proj_Start_Date FROM Projects
    WHERE Start_Date NOT IN (SELECT End_Date FROM Projects)
) s,
(
    SELECT End_Date AS Proj_End_Date FROM Projects
    WHERE End_Date NOT IN (SELECT Start_Date FROM Projects)
) e
WHERE s.Proj_Start_Date < e.Proj_End_Date
GROUP BY s.Proj_Start_Date
ORDER BY DATEDIFF(MIN(e.Proj_End_Date), s.Proj_Start_Date) ASC, s.Proj_Start_Date ASC;

32. Symmetric Pairs
SELECT f1.x, f1.y
FROM functions f1
JOIN functions f2
ON f1.x = f2.y AND f1.y = f2.x
WHERE f1.x != f1.y AND f1.x < f1.y
UNION
SELECT x, y
FROM functions
WHERE x = y
GROUP BY x, y
HAVING COUNT(1) > 1
ORDER BY x ASC;

33. Print *
SET @TEMP := 21;
SELECT REPEAT('* ', @TEMP := @TEMP - 1)
FROM INFORMATION_SCHEMA.TABLES;
