# Session 02 - Introduction to SQL II

### 1. Tuples from the CARMAKERS relation having 'barcelona' in the city attribute.
```sql
SELECT *
FROM carmakers
WHERE citycm='barcelona';
```

### 2. Tuples from the CUSTOMERS relation for 'madrid' customers with 'garcia' surname.
```sql
SELECT *
FROM customers
WHERE city='madrid' AND surname='garcia';
```

###  The same for customers having either one or the other condition.
```sql
SELECT *
FROM customers
WHERE city='madrid' OR surname='garcia';
```

### 3. Get a relation having the values of the surname and city attributes from the CUSTOMERS relation.
```sql
SELECT surname, city
FROM customers;
```

### 4. Get a relation showing the surnames of the CUSTOMERS from 'madrid'.
```sql
SELECT surname
FROM customers
WHERE city='madrid';
```

### 5. Names of car makers having 'gtd' models.
```sql
SELECT DISTINCT M.namecm
FROM carmakers M, cmcars CM, cars C
WHERE M.cifcm=CM.cifcm AND CM.codecar=C.codecar AND C.model='gtd';

SELECT DISTINCT M.namecm
FROM carmakers M INNER JOIN cmcars MC ON M.cifcm = MC.cifcm
    INNER JOIN cars C ON MC.codecar = C.codecar
WHERE C.model = 'gtd';
```

### 6. Names of car makers that have sold red cars.
```sql
SELECT DISTINCT M.namecm
FROM carmakers M, cmcars CM, sales S
WHERE M.cifcm=CM.cifcm AND CM.codecar=S.codecar AND S.color='red';

SELECT DISTINCT M.namecm
FROM carmakers M INNER JOIN cmcars CM ON M.cifcm = CM.cifcm
    INNER JOIN sales S ON CM.codecar = S.codecar
WHERE S.color = 'red';
```

### 7. Names of cars having the same models as the car named 'cordoba'.
```sql
SELECT DISTINCT C2.namec
FROM cars C1, cars C2
WHERE C1.model = C2.model AND C1.namec = 'cordoba';

SELECT DISTINCT C2.namec
FROM cars C1 INNER JOIN cars C2 ON C1.model = C2.model
WHERE C1.namec = 'cordoba';

SELECT namec
FROM cars
WHERE model IN (SELECT model FROM cars WHERE namec='cordoba');
```

### 8. Names of cars NOT having a 'gtd' model.
```sql
SELECT DISTINCT namec
FROM cars
WHERE namec NOT IN (SELECT namec FROM cars WHERE model='gtd');

(SELECT namec FROM cars)
MINUS
(SELECT namec FROM cars WHERE model='gtd');
```

### 9. Get all tuples from the DEALERS relation.
```sql
SELECT *
FROM dealers;
```

### 10. Pairs of values: CIFC from the CARMAKERS relation, and DNI from the CUSTOMERS relation belonging to the same city.
```sql
SELECT CM.cifcm, C.dni
FROM carmakers CM, customers C
WHERE CM.citycm = C.city;
```

### The same for the ones not belonging to the same city.
```sql
SELECT CM.cifcm, C.dni
FROM carmakers CM, customers C
WHERE CM.citycm <> C.city;
```

### 11. Codecar values for the cars stocked in any dealer in 'barcelona'.
```sql
SELECT DI.codecar
FROM distribution DI, dealers DE
WHERE DI.cifd = DE.cifd AND DE.cityd='barcelona';

SELECT DI.codecar
FROM distribution DI INNER JOIN dealers DE ON DI.cifd = DE.cifd
WHERE DE.cityd='barcelona';

SELECT DI.codecar
FROM distribution DI
WHERE DI.cifd IN (SELECT  DE.cifd FROM dealers DE WHERE DE.cityd='barcelona');

SELECT DI.codecar
FROM distribution DI
WHERE  EXISTS (SELECT *
    FROM dealers DE
    WHERE DE.cityd='barcelona' AND DE.cifd=DI.cifd);
```

### 12. Codecar values for cars bought by a 'madrid' customer in a 'madrid' dealer.
```sql
SELECT DISTINCT S.codecar
FROM sales S, customers C,dealers D
WHERE S.dni = C.dni AND S.cifd = D.cifd AND C.city='madrid' AND D.cityd='madrid';

SELECT DISTINCT S.codecar
FROM sales S INNER JOIN customers C ON S.dni = C.dni
    INNER JOIN dealers D ON S.cifd = D.cifd
WHERE C.city='madrid' AND D.cityd='madrid';

SELECT DISTINCT S.codecar
FROM sales S
WHERE S.dni IN (SELECT C.dni FROM customers C WHERE C.city='madrid')
    AND S.cifd IN (SELECT D.cifd FROM dealers D WHERE D.cityd='madrid');
```

### 13. Codecar values for cars sold in a dealer in the same city as the buying customer.
```sql
SELECT DISTINCT S.codecar
FROM sales S, customers C, dealers D
WHERE S.dni=C.dni AND S.cifd=D.cifd AND C.city=D.cityd;

SELECT DISTINCT S.codecar
FROM sales S INNER JOIN customers C ON S.dni=C.dni
    INNER JOIN dealers D ON S.cifd=D.cifd
WHERE C.city=D.cityd;
```

### 14. Pairs of CARMAKERS (names) from the same city.
```sql
SELECT  M1.namecm, M2.namecm
FROM carmakers M1, carmakers M2
WHERE M1.citycm=M2.citycm AND M1.namecm<>M2.namecm;

SELECT M1.namecm, M2.namecm
FROM carmakers M1 INNER JOIN carmakers M2 ON M1.citycm=M2.citycm AND M1.namecm<>M2.namecm;
```

### 15. Get all tuples from the CUSTOMERS relation belonging to 'madrid' customers.
```sql
SELECT *
FROM customers
WHERE city='madrid';
```

### 16. DNI of the customers that have bought a car in a 'madrid' dealer.
```sql
SELECT DISTINCT S.dni
FROM sales S, dealers D
WHERE S.cifd=D.cifd AND D.cityd='madrid';

SELECT DISTINCT S.dni
FROM sales S INNER JOIN dealers D ON S.cifd=D.cifd
WHERE D.cityd='madrid';

SELECT DISTINCT S.dni
fROM sales S
WHERE S.cifd IN (SELECT D.cifd FROM dealers D WHERE D.cityd='madrid');

SELECT DISTINCT S.dni
FROM sales S
WHERE EXISTS (SELECT * FROM dealers D WHERE D.cityd='madrid' AND S.cifd=D.cifd);
```

### 17. Colors of cars sold by the 'acar' dealer.
```sql
SELECT DISTINCT color
FROM sales S, dealers D
WHERE S.cifd= D.cifd AND D.named='acar';

SELECT DISTINCT color
FROM sales S INNER JOIN dealers D ON S.cifd= D.cifd
WHERE D.named='acar';

SELECT DISTINCT color
FROM sales S
WHERE S.cifd IN (SELECT D.cifd FROM dealers D WHERE D.named='acar');

SELECT DISTINCT color
FROM sales S
WHERE EXISTS (SELECT * FROM dealers D WHERE D.named='acar' AND S.cifd=D.cifd);
```

### 18. Codecar of the cars sold by any 'madrid' dealer.
```sql
SELECT DISTINCT codecar
FROM sales S, dealers D
WHERE S.cifd=D.cifd AND D.cityd='madrid';

SELECT DISTINCT codecar
FROM sales S INNER JOIN dealers D ON S.cifd=D.cifd
WHERE D.cityd='madrid';
```

### 19. Names of customers that have bought any car in the 'dcar' dealer.
```sql
SELECT C.name
FROM customers C, sales S, dealers D
WHERE C.dni=S.dni AND S.cifd=D.cifd AND D.named='dcar';

SELECT C.name
FROM customers C INNER JOIN sales S ON C.dni=S.dni
    INNER JOIN dealers D ON S.cifd=D.cifd
WHERE D.named='dcar';

SELECT C.name
FROM customers C
WHERE c.dni IN (SELECT dni
    FROM sales S, dealers D
    WHERE S.cifd=D.cifd AND D.named='dcar');

SELECT C.name
FROM customers C, sales S
WHERE C.dni=S.dni AND S.cifd IN (SELECT D.cifd FROM dealers D WHERE D.named='dcar');

SELECT C.name
FROM customers C
WHERE C.dni IN (SELECT dni
    FROM sales S
    WHERE S.cifd IN (SELECT D.cifd FROM dealers D WHERE D.named='dcar'));
```

### 20. Name and surname of the customers that have bought a 'gti' model with 'white' color.
```sql
SELECT CU.name, CU.surname
FROM customers CU, sales S, cars CA
WHERE CU.dni=S.dni AND S.codecar=CA.codecar AND CA.model='gti' AND S.color='white';

SELECT CU.name, CU.surname
FROM customers CU INNER JOIN sales S ON CU.dni=S.dni
    INNER JOIN cars CA ON S.codecar=CA.codecar
WHERE CA.model='gti' AND S.color='white';

SELECT C.name, C.surname
FROM customers C, sales S
WHERE C.dni=S.dni AND S.color='white'
    AND S.codecar IN (SELECT CA.codecar FROM cars CA WHERE CA.model='gti');

SELECT CU.name, CU.surname
FROM customers CU
WHERE CU.dni IN (SELECT S.dni
    FROM sales S
    WHERE S.color='white' AND S.codecar IN (SELECT CA.codecar FROM cars CA WHERE CA.model='gti'));
```

### 21. Name and surname of customers that have bought a car in a 'madrid' dealer that has 'gti' cars.
```sql
SELECT name, surname
FROM customers
WHERE dni IN (SELECT DISTINCT S.dni
    FROM sales S, cars C, dealers DE, distribution DI
    WHERE S.cifd=DE.cifd AND DE.cityd='madrid' AND DE.cifd=DI.cifd
            AND S.codecar=C.codecar AND C.model='gti');

SELECT name, surname
FROM customers
WHERE dni IN (SELECT DISTINCT S.dni
    FROM sales S INNER JOIN cars C ON S.codecar=C.codecar
        INNER JOIN dealers DE ON S.cifd=DE.cifd
        INNER JOIN distribution DI ON DE.cifd=DI.cifd
    WHERE DE.cityd='madrid' AND C.model='gti');
```

### 22. Name and surname of customers that have bought (at least) a 'white' and a 'red' car.
```sql
SELECT name,surname
FROM sales v1, sales v2, customers c
WHERE v1.color='white' AND v2.color='red' AND v1.dni=v2.dni AND v1.dni=c.dni;

SELECT name,surname
FROM sales v1 INNER JOIN sales v2 ON v1.dni = v2.dni
    INNER JOIN customers c ON v1.dni = c.dni
WHERE v1.color='white' AND v2.color='red';

SELECT c.name, c.surname
FROM customers c
WHERE dni IN ((SELECT dni FROM sales WHERE color='red')
    INTERSECT
    (SELECT dni FROM sales WHERE color='white'));

SELECT C.name, C.surname
FROM customers C, sales S
WHERE C.dni=S.dni AND S.color='white'
    AND C.dni IN (SELECT dni FROM sales WHERE color='red');

SELECT C.name, C.surname
FROM customers C
WHERE C.dni IN (SELECT dni FROM sales WHERE color='white')
    AND C.dni IN (SELECT dni FROM sales WHERE color='red');
```

### 23. DNI of customers that have only bought cars at the dealer with CIFD '1'.
```sql
(SELECT dni FROM sales)
MINUS
(SELECT dni FROM sales WHERE cifd<>'1');

SELECT dni
FROM sales
WHERE dni NOT IN (SELECT dni FROM sales WHERE cifd<>'1');
```

### 24. Names of the customers that have NOT bought 'red' cars at 'madrid' dealers.
```sql
SELECT DISTINCT C.name
FROM customers C, sales S
WHERE S.dni=C.dni AND C.dni NOT IN (SELECT S.dni
    FROM dealers D, sales S
    WHERE D.cifd=S.cifd AND D.cityd='madrid' AND S.color='red');

SELECT DISTINCT C.name
FROM customers C INNER JOIN sales S ON S.dni=C.dni
WHERE C.dni NOT IN (SELECT S.dni
    FROM dealers D INNER JOIN sales S ON D.cifd=S.cifd
    WHERE D.cityd='madrid' AND S.color='red');

SELECT name
FROM customers
WHERE dni IN ((SELECT dni FROM sales)
    MINUS
    (SELECT dni FROM dealers D, sales S WHERE D.cifd=S.cifd AND D.cityd='madrid' AND S.color='red'));
```

### 25. For each dealer (cifd), show the total amount of cars stocked.
```sql
SELECT cifd, sum(stockage) AS total_stockage
FROM distribution
GROUP BY cifd;
```

### 26. Show the cifc of dealers with an average stockage of more than 10 units (show that average as well).
```sql
SELECT cifd, avg(stockage) AS avg_stockage
FROM distribution
GROUP BY cifd
HAVING avg(stockage)>10;
```

### 27. CIFD of dealers with a stock between 10 and 18 units inclusive.
```sql
SELECT cifd, sum(stockage) AS total_stockage
FROM distribution
GROUP BY cifd
HAVING sum(stockage)>=10 AND sum(stockage)<=18;

SELECT cifd, sum(stockage) AS total_stockage
FROM distribution
GROUP BY cifd
HAVING sum(stockage) BETWEEN 10 AND 18;
```

### 28. Total amount of car makers.
```sql
SELECT count(*) AS total_cm
FROM carmakers;
```

###  Total amount of cities with car makers.
```sql
SELECT count(DISTINCT citycm) AS total_ci
FROM carmakers;
```

### 29. Name and surname of customers that have bought a car in a 'madrid' dealer, and have a name starting with j.
```sql
SELECT DISTINCT name, surname
FROM customers C, sales S, dealers D
WHERE C.dni=S.dni AND C.city=D.cityd AND D.cityd='madrid' AND C.name like 'j%';

SELECT DISTINCT name, surname
FROM customers C INNER JOIN sales S ON C.dni=S.dni
    INNER JOIN dealers D ON C.city=D.cityd
WHERE C.name like 'j%' AND D.cityd='madrid';
```

### 30. List customers, ordered by name (ascending).
```sql
SELECT dni, name, surname
FROM customers
ORDER BY name ASC;
```

### 31. List customers that have bought a car in the same dealer as customer with dni '2' (excluding the customer with dni '2').
```sql
SELECT DISTINCT name, surname
FROM customers C, sales S
WHERE C.dni=S.dni AND C.dni <> '2'
    AND S.cifd IN (SELECT cifd FROM sales WHERE dni='2');

SELECT DISTINCT name, surname
FROM customers C INNER JOIN sales S ON C.dni=S.dni
WHERE S.cifd IN (SELECT cifd FROM sales WHERE dni = '2') AND C.dni <> '2';
```

###  Same with dni '1'.
```sql
SELECT DISTINCT name, surname
FROM customers C, sales S
WHERE C.dni=S.dni AND C.dni <> '1'
    AND S.cifd IN (SELECT cifd FROM sales WHERE dni='1');
```

### 32. Return a list with the dealers which have a total number of car units in stock greater than the global unit average of all dealers together.
```sql
SELECT cifd,named,cityd
FROM dealers
WHERE cifd IN(SELECT  cifd
    FROM  distribution
    GROUP BY cifd
    HAVING SUM(stockage) > (SELECT  AVG(total) FROM (SELECT SUM(stockage)total FROM  distribution GROUP BY cifd)));

SELECT  distribution.cifd,named, cityd
FROM  distribution,dealers
WHERE distribution.cifd = dealers.cifd
GROUP BY distribution.cifd,named,cityd
HAVING SUM(stockage)> (SELECT  AVG(total) FROM (SELECT SUM(stockage)total FROM  distribution GROUP BY cifd));

SELECT distribution.cifd,named, cityd
FROM  distribution,dealers
WHERE distribution.cifd = dealers.cifd
GROUP BY distribution.cifd,named, cityd
HAVING SUM(stockage)> (SELECT SUM(stockage)/COUNT(DISTINCT cifd) FROM  distribution);
```

### 33. Dealer having the best average stockage of all dealers; that is, dealer having an average stockage greater than the average stockage of each one of the remaining dealers.
```sql
SELECT de.cifd, named, cityd
FROM dealers de, distribution di
WHERE de.cifd=di.cifd
GROUP BY de.cifd, named, cityd
HAVING AVG(stockage)>= ALL(SELECT AVG(stockage) FROM distribution GROUP BY cifd);

SELECT de.cifd, named, cityd
FROM dealers de INNER JOIN distribution di ON de.cifd=di.cifd
GROUP BY de.cifd, named, cityd
HAVING AVG(stockage) >= ALL (SELECT AVG(stockage) FROM distribution GROUP BY cifd);
```

### 34. List the two customers that have bought more cars in total, ordered by the number of cars bought.
```sql
SELECT dni, name, surname
FROM customers
WHERE dni IN (SELECT dni
    FROM (SELECT COUNT(codecar), dni
        FROM sales
        GROUP BY dni
        ORDER BY COUNT(codecar) DESC
        FETCH FIRST 2 ROWS ONLY));
```

### 35. Create a view from query 34. Using such view, list for each of the customers who have bought more cars in total, the code of the cars bought, the cifd of the dealers where they were bought, and the color.
```sql
CREATE VIEW customersmorecars AS
    SELECT dni, name, surname
    FROM customers
    WHERE dni IN (SELECT dni
        FROM (SELECT COUNT(codecar), dni
            FROM sales
            GROUP BY dni
            ORDER BY COUNT(codecar) DESC
            FETCH FIRST 2 ROWS ONLY));

SELECT s.dni, name, surname, codecar, cifd, color
FROM sales s, customersmorecars c
WHERE s.dni=c.dni;
```