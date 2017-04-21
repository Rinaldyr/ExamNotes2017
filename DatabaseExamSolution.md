# Question 1
### a
``` SQL
SELECT city, SUM(duration) "duration of calls received"
FROM Calls
JOIN Subscribers
ON receiver == phoneno
GROUP BY city;
```
### b
``` SQL
SELECT DISTINCT name
FROM Calls
JOIN Subscribers
ON receiver == phoneno OR caller == phoneno
WHERE duration >= 600;
```
### c
``` SQL
SELECT DISTINCT name
FROM Subscribers
WHERE phoneno NOT IN (
    SELECT DISTINCT caller 
    FROM Calls
    JOIN Subscribers
    ON caller == phoneno
    WHERE receiver IN (SELECT phoneno
    FROM Subscribers
    WHERE city LIKE "London")
)
ORDER BY name;
```
### d
``` SQL
SELECT name, SUM(Duration) FROM Subscribers
JOIN Calls
ON phoneno == caller
GROUP BY caller
HAVING SUM(Duration) > 500
```
# Question 2
```
E1(A1, A2)
E2(A4, A5)
E3(A6, A4, A7)
R1(A1, A4, A6, A3)
```
# Question 3
![](https://i.gyazo.com/f9ba03c1791c66134aba827296070afc.png)
