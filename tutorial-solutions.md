# SQLZOO
This file contains my solutions to all tutorials found in [SQLZOO](https://sqlzoo.net/wiki/SQL_Tutorial).

## Contents
1. [SELECT basics](#select-basics), [quiz](#quiz-1)
2. [SELECT from WORLD](#select-from-world), [quiz](#quiz-2)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT in SELECT](#select-in-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

## SELECT basics
1. The example uses a `WHERE` clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes'; Modify it to show the population of Germany
``` sql
SELECT population FROM world
  WHERE name = 'Germany'
```
2. The word `IN` allows us to check if an item is in a list. The example shows the name and population for the countries 'Brazil', 'Russia', 'India' and 'China'. Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
``` sql
SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark');
```
3. Which countries are not too small and not too big? `BETWEEN` allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
``` sql
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
```
### Quiz 1
1. Select the code which produces this table
``` sql
SELECT name, population
  FROM world
 WHERE population BETWEEN 1000000 AND 1250000
```

2. Pick the result you would obtain from this code:
Table-E         <br>
Albania	3200000 <br>
Algeria	32900000<br>

3. Select the code which shows the countries that end in A or L
``` sql
SELECT name FROM world
 WHERE name LIKE '%a' OR name LIKE '%l'
```

4. Pick the result from the query
``` sql
SELECT name,length(name)
FROM world
WHERE length(name)=5 and continent='Europe'
```
name	length(name)<br>
Italy	5           <br>
Malta	5           <br>
Spain	5           <br>

5. Here are the first few rows of the world table: Pick the result you would obtain from this code:
``` sql
SELECT name, area*2 FROM world WHERE population = 64000
```
Andorra	936<br>

6. Select the code that would show the countries with an area larger than 50000 and a population smaller than 10000000
``` sql
SELECT name, area, population
  FROM world
 WHERE area > 50000 AND population < 10000000
```

7. Select the code that shows the population density of China, Australia, Nigeria and France
``` sql
SELECT name, population/area
  FROM world
 WHERE name IN ('China', 'Nigeria', 'France', 'Australia')
```
## SELECT from world
