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
# Question 4
A transaction is a compound operation that must be performed in its entirety as though it was atomic. Transactions are needed for operations that must either happen or not happen -- in order to maintain consistency between databases. They also allow concurrent access and modification of the same database -- errors may arise when accessing a database that is in the middle of the operation. They also prevent corruption for if the communication between the interface and database is lost in the middle of a transaction -- the transaction won't go through at all and therefore won't be partially executed.

### Example
Say Alice presses a button to tell her bank to send £10 to Bob. The bank begins the operation and takes £10 out of Alice's account, but midway through the bank falls victim to a power cut failure. If a transaction is not implemented, Alice will lose £10 and Bob will not receive £10. Implementing a transaction will mean that the database is not updated with the inconsistent state.
