# VU Duomenų bazių valdymo sistemos

## SQL užklausų sprendimai iš [sql-ex.ru](https://sql-ex.ru/exercises/index.php?act=learn) svetainės

1\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=1). Find the model number, speed and hard drive capacity for all the PCs with prices below $500.
Result set: model, speed, hd.

```sql
SELECT model, speed, hd
FROM PC
WHERE price < 500
```

2\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=2). List all printer makers. Result set: maker.

```sql
SELECT DISTINCT maker
FROM product
WHERE type = 'printer'
```

3\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=3). Find the model number, RAM and screen size of the laptops with prices over $1000.

```sql
SELECT model, ram, screen
FROM laptop
WHERE price > 1000
```

4\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=4). Find all records from the Printer table containing data about color printers.

```sql
SELECT *
FROM printer
WHERE color = 'y'
```

5\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=5). Find the model number, speed and hard drive capacity of PCs cheaper than $600 having a 12x or a 24x CD drive.

```sql
SELECT model, speed, hd
FROM PC
WHERE cd IN ('12x', '24x')
  AND price < 600
```

6\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=6). For each maker producing laptops with a hard drive capacity of 10 Gb or higher, find the speed of such laptops. Result set: maker, speed.

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

7\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=7). Get the models and prices for all commercially available products (of any type) produced by maker B.

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

8\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=8). Find the makers producing PCs but not laptops.

```sql
SELECT maker
FROM product
WHERE type = 'PC'

EXCEPT

SELECT maker
FROM product
WHERE type = 'laptop'
```

9\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=9). Find the makers of PCs with a processor speed of 450 MHz or more. Result set: maker.

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

10\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=10). Find the printer models having the highest price. Result set: model, price.

```sql
SELECT model, price
FROM printer
WHERE price = (
  SELECT MAX(price) FROM printer
)
```

11\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=11). Find out the average speed of PCs.

```sql
SELECT AVG(speed) AS avg_speed
FROM pc
```

arba:

```sql
SELECT AVG(speed)
FROM pc
```

12\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=12). Find out the average speed of the laptops priced over $1000.

```sql
SELECT AVG(speed) AS avg_speed
FROM laptop
WHERE price > 1000
```

13\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=13). Find out the average speed of the PCs produced by maker A.

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

14\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=14). For the ships in the Ships table that have at least 10 guns, get the class, name, and country.

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

15\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=15). Get hard drive capacities that are identical for two or more PCs. Result set: hd.

```sql
SELECT hd
FROM pc
GROUP BY hd
HAVING COUNT(hd) >= 2
```

16\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=16). Get pairs of PC models with identical speeds and the same RAM capacity. Each resulting pair should be displayed only once, i.e. (i, j) but not (j, i).
Result set: model with the bigger number, model with the smaller number, speed, and RAM.

```sql
SELECT DISTINCT a.model, b.model, b.speed, b.ram
FROM PC a, PC b
WHERE a.speed = b.speed
  AND a.ram = b.ram
  AND a.model <> b.model
  AND a.model > b.model

```

17\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=17). Get the laptop models that have a speed smaller than the speed of any PC.
Result set: type, model, speed.

```sql
SELECT DISTINCT type, product.model, speed
FROM laptop, product
WHERE laptop.model = product.model
AND speed < ALL (
  SELECT MIN(speed)
  FROM PC
)
```

18\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=18). Find the makers of the cheapest color printers.
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

19\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=19). For each maker having models in the Laptop table, find out the average screen size of the laptops he produces. Result set: maker, average screen size.

```sql
SELECT maker, AVG(screen) AS avg_screen
FROM product
  INNER JOIN laptop
  ON laptop.model = product.model
GROUP BY maker
```

20\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=20). Find the makers producing at least three distinct models of PCs. Result set: maker, number of PC models.

```sql
SELECT maker, COUNT(model) AS model_count
FROM product
  WHERE type = 'PC'
GROUP BY maker
HAVING COUNT(model) >= 3;
```

21\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=21). Find out the maximum PC price for each maker having models in the PC table. Result set: maker, maximum price.

```sql
SELECT maker, MAX(price)
FROM pc
  INNER JOIN product
  ON product.model = pc.model
GROUP BY maker
```

22\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=22). For each value of PC speed that exceeds 600 MHz, find out the average price of PCs with identical speeds. Result set: speed, average price.

```sql
SELECT speed, AVG(price) AS avg_price
FROM pc
  WHERE speed > 600
GROUP BY speed
```

23\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=23). Get the makers producing both PCs having a speed of 750 MHz or higher and laptops with a speed of 750 MHz or higher. Result set: maker.

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

24\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=24). List the models of any type having the highest price of all products present in the database.

```sql
WITH all_shit AS (
  SELECT model, price
  FROM laptop

  UNION

  SELECT model, price
  FROM pc

  UNION

  SELECT model, price
  FROM printer
)

SELECT model
FROM all_shit
WHERE price IN (
  SELECT MAX(price)
  FROM all_shit
)
```

25\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=25). Find the printer makers also producing PCs with the lowest RAM capacity and the highest processor speed of all PCs having the lowest RAM capacity. Result set: maker.

```sql
SELECT DISTINCT maker
FROM Product
WHERE type = 'printer'

INTERSECT

SELECT DISTINCT maker
FROM product
WHERE maker IN (
  SELECT DISTINCT maker
  FROM product, PC
  WHERE product.model = PC.model
  AND speed = (
    SELECT DISTINCT MAX(speed)
    FROM PC
    WHERE ram IN (
      SELECT DISTINCT MIN(ram)
      FROM PC
    )
  )
  AND ram = (
    SELECT DISTINCT MIN(ram)
    FROM PC
  )
)
```

26\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=26). Find out the average price of PCs and laptops produced by maker A. Result set: one overall average price for all items.

```sql
WITH prices AS(
  SELECT price
  FROM PC, product
  WHERE PC.model = product.model
  AND maker = 'A'

  UNION ALL

  SELECT price
  FROM laptop, product
  WHERE laptop.model = product.model
  AND maker = 'A'
)

SELECT AVG(price)
FROM prices
```

27\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=27). Find out the average hard disk drive capacity of PCs produced by makers who also manufacture printers. Result set: maker, average HDD capacity.

```sql
WITH stuff AS (
  SELECT hd, maker
  FROM PC, product
  WHERE PC.model = product.model
  AND maker IN (
    SELECT maker
    FROM product
    WHERE type = 'printer'
  )
)

SELECT maker, AVG(hd) AS averageHD
FROM stuff
GROUP BY maker
```

28\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=28). Using Product table, find out the number of makers who produce only one model.

```sql
WITH all_makers AS (
  SELECT maker
  FROM product
  GROUP BY maker
  HAVING COUNT(type) = 1
)
SELECT COUNT(maker)
FROM all_makers
```

29\.

30\.

31\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=31). For ship classes with a gun caliber of 16 in. or more, display the class and the country.

```sql
SELECT class, country
FROM classes
WHERE bore >= 16
```

32\.

33\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=56). Get the ships sunk in the North Atlantic battle.Result set: ship.

```sql
SELECT ship
FROM outcomes
WHERE result = 'sunk'
AND battle = 'North Atlantic'
```

34\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=32). In accordance with the Washington Naval Treaty concluded in the beginning of 1922, it was prohibited to build battle ships with a displacement of more than 35 thousand tons.
Get the ships violating this treaty (only consider ships for which the year of launch is known). List the names of the ships.

```sql
FROM ships, classes
WHERE classes.class = ships.class
AND type = 'bb'
AND launched >= 1922
AND displacement > 35000
```

35\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=35). Find models in the Product table consisting either of digits only or Latin letters (A-Z, case insensitive) only. Result set: model, type.

```sql
SELECT model, type
FROM product
  WHERE model NOT LIKE '%[^a-zA-Z]%'
  OR model NOT LIKE '%[^0-9]%'
```

36\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=36). List the names of lead ships in the database (including the Outcomes table).

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

37\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=37). Find classes for which only one ship exists in the database (including the Outcomes table).

```sql
SELECT class
FROM (
  SELECT classes.class, ships.name AS _name_
  FROM classes, ships
  WHERE classes.class = ships.class

  UNION

  SELECT classes.class, ship AS _name_
  FROM classes, outcomes
  WHERE classes.class = outcomes.ship
) AS whatever
GROUP BY class
HAVING COUNT(_name_) = 1
```

38\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=38). Find countries that ever had classes of both battleships (‘bb’) and cruisers (‘bc’).

```sql
SELECT country
FROM classes
WHERE type = 'bc'

INTERSECT

SELECT country
FROM classes
WHERE type = 'bb'
```

39\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=39). Find the ships that `survived for future battles`; that is, after being damaged in a battle, they participated in another one, which occurred later.

```sql
WITH table1 AS (
  SELECT ship, result, battle, date
  FROM outcomes, battles
  WHERE outcomes.battle = battles.name
)

SELECT DISTINCT ship
FROM table1
WHERE table1.ship IN (
  SELECT ship
  FROM table1 AS table2
  WHERE table2.date < table1.date
    AND table2.result = 'damaged'
)
```

40\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=40). Get the makers who produce only one product type and more than one model. Output: maker, type.

```sql
SELECT maker, MAX(type)
FROM Product
GROUP BY maker
HAVING COUNT(DISTINCT type) = 1
AND COUNT(model) > 1
```

41\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=41). For each maker who has models at least in one of the tables PC, Laptop, or Printer, determine the maximum price for his products. Output: maker; if there are NULL values among the prices for the products of a given maker, display NULL for this maker, otherwise, the maximum price.

```sql
WITH good_stuff AS(
  SELECT maker, price
  FROM PC, product
  WHERE PC.model = product.model

  UNION ALL

  SELECT maker, price
  FROM laptop, product
  WHERE laptop.model = product.model

  UNION ALL

  SELECT maker, price
  FROM printer, product
  WHERE printer.model = product.model
)

SELECT maker,
(CASE
  WHEN maker IN (
    SELECT maker
    FROM PC, product
    WHERE PC.model = product.model
    AND price IS NULL

    UNION

    SELECT maker
    FROM laptop, product
    WHERE laptop.model = product.model
    AND price IS NULL

    UNION

    SELECT maker
    FROM printer, product
    WHERE printer.model = product.model
    AND price IS NULL
  )
  THEN NULL
  ELSE MAX(price)
END)

FROM good_stuff
GROUP BY maker
```

42\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=42). Find the names of ships sunk at battles, along with the names of the corresponding battles.

```sql
SELECT ship, battle
FROM outcomes
WHERE result = 'sunk'
```

43\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=43). Get the battles that occurred in years when no ships were launched into water.

```sql
SELECT name
FROM battles
WHERE DATEPART(yy, date) NOT IN (
  SELECT launched
  FROM ships
  WHERE launched IS NOT NULL
)
```

44\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=44). Find all ship names beginning with the letter R.

```sql
SELECT name
FROM ships
WHERE name LIKE 'R%'

UNION

SELECT ship
FROM outcomes
WHERE ship LIKE 'R%'
```

45\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=45). Find all ship names consisting of three or more words (e.g., King George V). Consider the words in ship names to be separated by single spaces, and the ship names to have no leading or trailing spaces.

```sql
SELECT name
FROM ships
WHERE name LIKE '% % %'

UNION

SELECT ship
FROM outcomes
WHERE ship LIKE '% % %'
```

46\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=46). For each ship that participated in the Battle of Guadalcanal, get its name, displacement, and the number of guns.

```sql
WITH table1 AS (
  SELECT DISTINCT name, displacement, numGuns
  FROM classes, ships
  WHERE classes.class = ships.class
  AND name IN (
    SELECT ship
    FROM outcomes
    WHERE battle = 'Guadalcanal'
  )

  UNION

  SELECT DISTINCT ship AS name, displacement, numGuns
  FROM classes, outcomes
  WHERE classes.class = outcomes.ship
    AND battle = 'Guadalcanal'
)

SELECT *
FROM table1

UNION

SELECT ship, NULL AS displacement, NULL AS numguns
FROM outcomes
WHERE battle = 'Guadalcanal'
  AND ship NOT IN (
    SELECT name
    FROM ships
  )
  AND ship NOT IN (
    SELECT name FROM table1
  )
```

47\.

48\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=48). Find the ship classes having at least one ship sunk in battles.

```sql
SELECT class
FROM classes
  INNER JOIN outcomes
  ON outcomes.ship = classes.class
WHERE result = 'sunk'

UNION

SELECT class
FROM ships
  INNER JOIN outcomes
  ON ships.name = outcomes.ship
WHERE result = 'sunk'
```

49\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=49). Find the names of the ships having a gun caliber of 16 inches (including ships in the Outcomes table).

```sql
SELECT name
FROM ships
WHERE class IN (
  SELECT class
  FROM classes
  WHERE bore = 16
)

UNION

SELECT ship
FROM outcomes
WHERE ship IN (
  SELECT class
  FROM classes
  WHERE bore = 16
)
```

50\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=50). Find the battles in which Kongo-class ships from the Ships table were engaged.

```sql
SELECT DISTINCT battle
FROM outcomes
  INNER JOIN ships
  ON ships.name = outcomes.ship
WHERE ships.class = 'kongo'
```

arba

```sql
SELECT DISTINCT battle
FROM outcomes
WHERE ship IN (
  SELECT name
  FROM ships
  WHERE class = 'kongo'
)
```

51\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=51). Find the names of the ships with the largest number of guns among all ships having the same displacement (including ships in the Outcomes table).

```sql
WITH all_data AS (
  SELECT name, displacement, numGuns
  FROM ships, classes
  WHERE ships.class = classes.class

  UNION

  SELECT ship, displacement, numGuns
  FROM outcomes, classes
  WHERE outcomes.ship = classes.class
), filtered_data AS (
  SELECT displacement, MAX(numGuns) AS maxGuns
  FROM all_data
  GROUP BY displacement
)

SELECT name
FROM all_data, filtered_data
  WHERE all_data.displacement = filtered_data.displacement
  AND all_data.numGuns = filtered_data.maxGuns
```

52\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=52). Determine the names of all ships in the Ships table that can be a Japanese battleship having at least nine main guns with a caliber of less than 19 inches and a displacement of not more than 65000 tons.

```sql
SELECT name
FROM ships, classes
WHERE ships.class = classes.class
AND (country = 'Japan' OR country IS NULL)
AND (type = 'bb' OR type IS NULL)
AND (numguns >= 9 OR numguns IS NULL)
AND (bore < 19 OR bore IS NULL)
AND (displacement <= 65000 OR displacement IS NULL)
```

53\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=53). With a precision of two decimal places, determine the average number of guns for the battleship classes.

```sql
SELECT CONVERT (
  NUMERIC (10, 2), AVG(numGuns * 1.0)
)  AS avg
FROM classes
WHERE type = 'bb'
```

54\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=54). With a precision of two decimal places, determine the average number of guns for all battleships (including the ones in the Outcomes table).

```sql
WITH all_ships AS (
  SELECT ship, numguns
  FROM outcomes, classes
  WHERE outcomes.ship = classes.class
    AND classes.type = 'bb'

  UNION

  SELECT name, numguns
  FROM ships, classes
  WHERE ships.class = classes.class
    AND classes.type = 'bb'
)

SELECT CONVERT (
  NUMERIC(10, 2), AVG(numguns * 1.0)
) AS average
FROM all_ships
```

55\. 🚢 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=55). For each class, determine the year the first ship of this class was launched. If the lead ship’s year of launch is not known, get the minimum year of launch for the ships of this class. Result set: class, year.

```sql
SELECT classes.class, MIN(ships.launched)
FROM classes, ships
WHERE classes.class = ships.class
GROUP BY classes.class

UNION

SELECT classes.class, NULL
FROM classes
WHERE (
  SELECT COUNT(1)
  FROM ships
  WHERE classes.class = ships.class
) = 0
```

56\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=56). For each class, find out the number of ships of this class that were sunk in battles. Result set: class, number of ships sunk.

```sql
WITH a AS (
  SELECT class, ship
  FROM ships, outcomes
  WHERE ships.name = outcomes.ship
  AND result = 'sunk'

  UNION

  SELECT ship AS class, ship
  FROM outcomes
  WHERE result = 'sunk'
)

SELECT classes.class, COUNT(ship)
FROM classes
LEFT JOIN a
  ON classes.class = a.class
GROUP BY classes.class
```

57\.

58\.

59\.

60\.

61\.

62\.

63\. ✈️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=63). Find the names of different passengers that ever travelled more than once occupying seats with the same number.

```sql
WITH fuckers AS (
  SELECT DISTINCT id_psg
  FROM pass_in_trip
  GROUP BY id_psg, place
  HAVING COUNT(*) > 1
)

SELECT name
FROM fuckers, passenger
WHERE fuckers.id_psg = passenger.id_psg
```

arba

```sql
SELECT name
FROM passenger
WHERE id_psg IN (
  SELECT id_psg
  FROM pass_in_trip
  GROUP BY place, id_psg
  HAVING COUNT(*) > 1
)
```

64\.

65\.

66\. ✈️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=66). For all days between 2003-04-01 and 2003-04-07 find the number of trips from Rostov. Result set: date, number of trips.

```sql
WITH dates AS (
  SELECT '2003-04-01 00:00:00.000' date UNION
  SELECT '2003-04-02 00:00:00.000' date UNION
  SELECT '2003-04-03 00:00:00.000' date UNION
  SELECT '2003-04-04 00:00:00.000' date UNION
  SELECT '2003-04-05 00:00:00.000' date UNION
  SELECT '2003-04-06 00:00:00.000' date UNION
  SELECT '2003-04-07 00:00:00.000' date
)

SELECT dates.date, (
  SELECT COUNT(*)
  FROM (
    SELECT DISTINCT trip.trip_no
    FROM pass_in_trip, trip
    WHERE pass_in_trip.trip_no = trip.trip_no
    AND trip.town_from = 'rostov'
    AND dates.date = pass_in_trip.date
  ) trips
) AS count
FROM dates
```

67\.

68\.

69\.

70\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=70). Get the battles in which at least three ships from the same country took part.

```sql
WITH grouped_ships AS (
  SELECT name AS ship, country
  FROM ships, classes
  WHERE ships.class = classes.class

  UNION

  SELECT ship, country
  FROM outcomes, classes
  WHERE outcomes.ship = classes.class
)

SELECT DISTINCT battle
FROM outcomes, grouped_ships
WHERE outcomes.ship = grouped_ships.ship
GROUP BY battle, country
HAVING COUNT(battle) >=3

```

71\.

72\. ✈️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=72). Among the customers using a single airline, find distinct passengers who have flown most frequently. Result set: passenger name, number of trips.

```sql
WITH devotedP AS (
  SELECT ID_psg
  FROM pass_in_trip, trip
  WHERE pass_in_trip.trip_no = trip.trip_no
  GROUP BY ID_psg
  HAVING COUNT(DISTINCT ID_comp) = 1
), maxFlies AS (
  SELECT devotedP.ID_psg, COUNT(trip_no) AS count
  FROM devotedP, pass_in_trip
  WHERE devotedP.ID_psg = pass_in_trip.ID_psg
  GROUP BY devotedP.ID_psg
)

SELECT name, count
FROM maxFlies, passenger
WHERE maxFlies.ID_psg = passenger.ID_psg
AND count IN (
  select MAX(count)
  from maxFlies
)
```

arba

```sql
WITH crap AS (
  SELECT id_psg, COUNT(*) count
  FROM pass_in_trip AS PIT1
  WHERE (
    SELECT COUNT(id_comp)
    FROM (
      SELECT id_comp
      FROM trip, pass_in_trip AS PIT2
      WHERE PIT1.id_psg = PIT2.id_psg
      AND trip.trip_no = PIT2.trip_no
      GROUP BY trip.id_comp
    ) companies
  ) = 1
  GROUP BY id_psg
)

SELECT name, count
FROM crap, passenger
WHERE crap.id_psg = passenger.id_psg
AND crap.count = (
  SELECT MAX(count)
  FROM crap
)
```

73\.

74\.

75\.

76\.

77\.

78\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=78). For each battle, get the first and the last day of the month when the battle occurred.
Result set: battle name, first day of the month, last day of the month. Note: output dates in yyyy-mm-dd format.

```sql
SELECT name, DATEFROMPARTS (
  DATEPART (year, date),
  DATEPART (month, date), 1
) AS 'first day', DATEADD (
  day, -1, DATEADD (
    month, 1, DATEFROMPARTS (
      DATEPART (year, date),
      DATEPART (month, date), 1
    )
  )
) AS 'last day'

FROM Battles
```

79\.

80\. 🖥️ 1️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=80). Find the computer equipment makers not producing any PC models absent in the PC table.

```sql
SELECT maker
FROM product

EXCEPT

SELECT maker
FROM product
WHERE type = 'PC' AND model NOT IN (
  SELECT model
  FROM pc
)
```

81\.

82\.

83\. 🚢 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=83). Find out the names of the ships in the Ships table that meet at least four criteria from the following list: numGuns = 8, bore = 15, displacement = 32000, type = bb, launched = 1915, class = Kongo, country = USA.

```sql
WITH asd AS (
  SELECT name, (
    CASE numGuns WHEN 8 THEN 1 ELSE 0 END+
    CASE bore WHEN 15 THEN 1 ELSE 0 END +
    CASE displacement WHEN 32000 THEN 1 ELSE 0 END+
    CASE type WHEN 'bb' THEN 1 ELSE 0 END+
    CASE launched WHEN 1915 THEN 1 ELSE 0 END+
    CASE ships.class WHEN 'Kongo' THEN 1 ELSE 0 END+
    CASE country WHEN 'USA' THEN 1 ELSE 0 END
  ) AS criteriaCount
  FROM ships, classes
  WHERE ships.class = classes.class
)

SELECT name
FROM asd
WHERE criteriaCount >= 4
```

84\.

85\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=85). Get makers producing either printers only or personal computers only; in case of PC manufacturers they should produce at least 3 models.

```sql
WITH devotedDeveloper AS (
  SELECT maker, count(type) as numUniqueProducts
  FROM product
  GROUP BY maker
  HAVING COUNT(DISTINCT type) = 1
)

SELECT DISTINCT product.maker
FROM devotedDeveloper, product
WHERE devotedDeveloper.maker = product.maker
AND (
  type = 'printer' OR
  (type = 'PC' AND numUniqueProducts >=3)
)
```

86\.

87\.

88\.

89\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=89). Get makers having most models in the Product table, as well as those having least. Output: maker, number of models.

```sql
WITH stats AS (
  SELECT maker, COUNT(type) as count
  FROM product
  GROUP BY maker
)

SELECT maker, count
FROM stats
WHERE count = (
  SELECT MAX(count)
  FROM stats
)

UNION

SELECT maker, count
FROM stats
WHERE count = (
  SELECT MIN(count)
  FROM stats
)
```

90\. 🖥️ 2️⃣ [Link](https://sql-ex.ru/exercises/index.php?act=learn&LN=90). Display all records from the Product table except for three rows with the smallest model numbers and three ones with the greatest model numbers.

```sql
SELECT *
FROM product
WHERE model NOT IN (
  SELECT TOP 3 model
  FROM product
  ORDER BY model DESC
)
AND model NOT IN (
  SELECT TOP 3 model
  FROM product
  ORDER BY model ASC
)
```
