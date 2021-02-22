# VU Duomenų bazių valdymo sistemos

## SQL užklausų sprendimai iš [sql-ex.ru](https://sql-ex.ru/exercises/index.php?act=learn) svetainės

1\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=1). Score: 1. Find the model number, speed and hard drive capacity for all the PCs with prices below $500.
Result set: model, speed, hd.

```sql
SELECT model, speed, hd
FROM PC
WHERE price < 500
```

2\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=2). Score: 1. List all printer makers. Result set: maker.

```sql
SELECT DISTINCT maker
FROM product
WHERE type = 'printer'
```

3\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=3). Score: 1. Find the model number, RAM and screen size of the laptops with prices over $1000.

```sql
SELECT model, ram, screen
FROM laptop
WHERE price > 1000
```

4\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=4). Score: 1. Find all records from the Printer table containing data about color printers.

```sql
SELECT *
FROM printer
WHERE color = 'y'
```

5\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=5). Score: 1. Find the model number, speed and hard drive capacity of PCs cheaper than $600 having a 12x or a 24x CD drive.

```sql
SELECT model, speed, hd
FROM PC
WHERE cd IN ('12x', '24x')
  AND price < 600
```

6\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=6). Score: 2. For each maker producing laptops with a hard drive capacity of 10 Gb or higher, find the speed of such laptops. Result set: maker, speed.

```sql
SELECT DISTINCT product.maker, laptop.speed
FROM laptop
  INNER JOIN product
  ON product.model = laptop.model
WHERE laptop.hd >= 10
```

arba:

```sql
SELECT DISTINCT product.maker, laptop.speed
FROM laptop, product
WHERE product.model = laptop.model
  AND laptop.hd >= 10
```

7\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=7). Score: 2. Get the models and prices for all commercially available products (of any type) produced by maker B.

```sql
SELECT DISTINCT product.model, price
FROM product
  INNER JOIN pc
  ON product.model = pc.model
  AND product.maker = 'B'

UNION

SELECT DISTINCT product.model, price
FROM product
  INNER JOIN laptop
  ON product.model = laptop.model
  AND product.maker = 'B'

UNION

SELECT DISTINCT product.model, price
FROM product
  INNER JOIN printer
  ON product.model = printer.model
  AND product.maker = 'B'
```

8\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=8). Score: 2. Find the makers producing PCs but not laptops.

```sql
SELECT maker
FROM product
WHERE type = 'PC'

EXCEPT

SELECT maker
FROM product
WHERE type = 'laptop'
```

9\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=9). Score: 1. Find the makers of PCs with a processor speed of 450 MHz or more. Result set: maker.

```sql
SELECT DISTINCT maker
FROM product
  INNER JOIN pc
  ON product.model = pc.model
WHERE pc.speed >= 450
```

arba:

```sql
SELECT DISTINCT maker
FROM product, pc
WHERE product.model = pc.model
  AND pc.speed >= 450
```

10\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=10). Score: 1. Find the printer models having the highest price. Result set: model, price.

```sql
SELECT model, price
FROM printer
WHERE price = (
  SELECT MAX(price) FROM printer
)
```

11\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=11). Score: 1. Find out the average speed of PCs.

```sql
SELECT AVG(speed) AS avg_speed
FROM pc
```

arba:

```sql
SELECT AVG(speed)
FROM pc
```

12\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=12). Score: 1. Find out the average speed of the laptops priced over $1000.

```sql
SELECT AVG(speed) AS avg_speed
FROM laptop
WHERE price > 1000
```

13\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=13). Score: 1. Find out the average speed of the PCs produced by maker A.

```sql
SELECT AVG(speed) AS avg_speed
FROM product
  INNER JOIN pc
  ON product.model = pc.model
WHERE product.maker = 'A'
```

arba:

```sql
SELECT AVG(speed) AS avg_speed
FROM product, pc
WHERE product.model = pc.model
  AND product.maker = 'A'
```

14\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=14). Score: 1. For the ships in the Ships table that have at least 10 guns, get the class, name, and country.

```sql
SELECT classes.class, ships.name, classes.country
FROM ships
  INNER JOIN classes
  ON ships.class = classes.class
WHERE classes.numGuns >= 10
```

arba:

```sql
SELECT DISTINCT classes.class, ships.name, classes.country
FROM ships, classes
WHERE ships.class = classes.class
  AND classes.numGuns >= 10
```

15\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=15). Score 1. Get hard drive capacities that are identical for two or more PCs. Result set: hd.

```sql
SELECT hd
FROM pc
GROUP BY hd
HAVING COUNT(hd) >= 2
```

16\.

17\.

18\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=18). Score 2. Find the makers of the cheapest color printers.
Result set: maker, price.

```sql
SELECT DISTINCT maker, price
FROM printer
  INNER JOIN product
  ON printer.model = product.model
  WHERE color = 'y'
  AND price = (
    SELECT MIN(price) FROM printer
    WHERE color = 'y'
  )
```

19\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=19). Score 1. For each maker having models in the Laptop table, find out the average screen size of the laptops he produces.
Result set: maker, average screen size.

```sql
SELECT maker, AVG(screen) AS avg_screen
FROM product
  INNER JOIN laptop
  ON laptop.model = product.model
GROUP BY maker
```

20\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=20). Score 1. Find the makers producing at least three distinct models of PCs.
Result set: maker, number of PC models.

```sql
SELECT maker, COUNT(model) AS model_count
FROM product
  WHERE type = 'PC'
GROUP BY maker
HAVING COUNT(model) >= 3;
```

21\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=21). Score 1. Find out the maximum PC price for each maker having models in the PC table. Result set: maker, maximum price.

```sql
SELECT maker, MAX(price)
FROM pc
  INNER JOIN product
  ON product.model = pc.model
GROUP BY maker
```

22\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=22). Score 1. For each value of PC speed that exceeds 600 MHz, find out the average price of PCs with identical speeds. Result set: speed, average price.

```sql
SELECT speed, AVG(price) AS avg_price
FROM pc
  WHERE speed > 600
GROUP BY speed
```

23\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=23). Score 1. Get the makers producing both PCs having a speed of 750 MHz or higher and laptops with a speed of 750 MHz or higher. Result set: maker.

```sql
SELECT maker
FROM pc
  INNER JOIN product
  ON product.model = pc.model
WHERE speed >= 750

INTERSECT

SELECT maker
FROM laptop
  INNER JOIN product
  ON product.model = laptop.model
WHERE speed >= 750
```

24\.

25\.

26\.

27\.

28\.

29\.

30\.

31\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=31). Score 1. For ship classes with a gun caliber of 16 in. or more, display the class and the country.

```sql
SELECT class, country
FROM classes
WHERE bore >= 16
```

32\.

33\.

34\.

35\.

36\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=36). Score 1. List the names of lead ships in the database (including the Outcomes table).

```sql
SELECT ships.name
FROM ships
  INNER JOIN classes
  ON ships.name = classes.class

UNION

SELECT outcomes.ship
FROM outcomes
  INNER JOIN classes
  ON outcomes.ship = classes.class
```

37\.

38\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=38). Score 1. Find countries that ever had classes of both battleships (‘bb’) and cruisers (‘bc’).

```sql
SELECT country
FROM classes
WHERE type = 'bc'

INTERSECT

SELECT country
FROM classes
WHERE type = 'bb'
```

39\.

40\.

41\.

42\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=42). Score 1. Find the names of ships sunk at battles, along with the names of the corresponding battles.

```sql
SELECT ship, battle
FROM outcomes
WHERE result = 'sunk'
```

43\.

44\. [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=44). Score 1. Find all ship names beginning with the letter R.

```sql
SELECT name
FROM ships
WHERE name LIKE 'R%'

UNION

SELECT ship
FROM outcomes
WHERE ship LIKE 'R%'
```
