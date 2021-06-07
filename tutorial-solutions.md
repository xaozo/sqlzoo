# SQLZOO (WIP)
This file contains my solutions to all tutorials found in [SQLZOO](https://sqlzoo.net/wiki/SQL_Tutorial).

## Contents
0  [SELECT basics](#select-basics), [quiz](#quiz-1)           <br>
1  [SELECT name](#select-name)                                <br>
2  [SELECT from WORLD](#select-from-world), [quiz](#quiz-2)   <br>
3  [SELECT from NOBEL](#select-from-nobel)                    <br>
4  [SELECT within SELECT](#select-in-select)                  <br>
5  [SUM and COUNT](#sum-and-count)                            <br>
6  [JOIN](#join)                                              <br>
7  [More JOIN operations](#more-join)                         <br>
8  [Using NULL](#using-null)                                  <br>
8+ [Numeric Examples](#numeric-examples)                      <br>
9- [Window function](#window-function)                        <br>
9+ [COVID 19](#covid-19)                                      <br>
9  [Self JOIN](#self-join)                                    <br>

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

## SELECT name
1. You can use `WHERE name LIKE 'B%'` to find the countries that start with "B".
* The % is a wild-card it can match any characters
Find the country that start with Y
``` sql
SELECT name FROM world
  WHERE name LIKE 'Y%'
```
2. Find the countries that end with y
``` sql
SELECT name FROM world
  WHERE name LIKE '%y'
```
3. Luxembourg has an x - so does one other country. List them both.
Find the countries that contain the letter x
``` sql
SELECT name FROM world
  WHERE name LIKE '%x%'
```
4. Iceland, Switzerland end with land - but are there others?
Find the countries that end with land
``` sql
SELECT name FROM world
  WHERE name LIKE '%land'
```
5. Columbia starts with a C and ends with ia - there are two more like this.
Find the countries that start with C and end with ia
``` sql
SELECT name FROM world
  WHERE name LIKE 'C%ia'
```
6. Greece has a double e - who has a double o?
Find the country that has oo in the name
``` sql
SELECT name FROM world
  WHERE name LIKE '%oo%'
```
7. Bahamas has three a - who else?
Find the countries that have three or more a in the name
``` sql
SELECT name FROM world
  WHERE name LIKE '%a%a%a%'
```
8. India and Angola have an n as the second character. You can use the underscore as a single character wildcard.
Find the countries that have "t" as the second character.
``` sql
SELECT name FROM world
 WHERE name LIKE '_t%'
ORDER BY name
```
9. Lesotho and Moldova both have two o characters separated by two other characters.
Find the countries that have two "o" characters separated by two others.
``` sql
SELECT name FROM world
 WHERE name LIKE '%o__o%'
```
10. Cuba and Togo have four characters names.
Find the countries that have exactly four characters.
``` sql
SELECT name FROM world
 WHERE name LIKE '____'
```
11. The capital of Luxembourg is Luxembourg. Show all the countries where the capital is the same as the name of the country
Find the country where the name is the capital city.
``` sql
SELECT name
  FROM world
 WHERE name = capital
```
12. The capital of Mexico is Mexico City. Show all the countries where the capital has the country together with the word "City".
Find the country where the capital is the country plus "City".
``` sql
SELECT name
  FROM world
 WHERE capital = CONCAT(name, ' City')
```
13. Find the capital and the name where the capital includes the name of the country.
``` sql
SELECT capital, name FROM world
WHERE capital LIKE CONCAT('%', name, '%')
```
14. Find the capital and the name where the capital is an extension of name of the country.
You should include Mexico City as it is longer than Mexico. You should not include Luxembourg as the capital is the same as the country.
``` sql
SELECT capital, name FROM world
WHERE capital LIKE CONCAT('%', name, '%')
AND capital != name
```
15. For Monaco-Ville the name is Monaco and the extension is -Ville.
Show the name and the extension where the capital is an extension of name of the country.
You can use the SQL function REPLACE.
``` sql
SELECT name, REPLACE(capital, name, '')
FROM world
WHERE capital LIKE CONCAT('%', name, '%')
AND capital != name
```

## SELECT from world
1. Read the notes about this table. Observe the result of running this SQL command to show the name, continent and population of all countries.
``` sql
SELECT name, continent, population FROM world
```
2. How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
``` sql
SELECT name
  FROM world
 WHERE population > 200000000
```
3. Give the `name` and the per capita GDP for those countries with a `population` of at least 200 million.
``` sql
SELECT name, gdp/population
FROM world
WHERE population > 200000000
```
4. Show the `name` and `population` in millions for the countries of the `continent` 'South America'. Divide the population by 1000000 to get population in millions.
``` sql
SELECT name, population/1000000
FROM world
WHERE continent = 'South America'
```
5. Show the `name` and `population` for France, Germany, Italy
``` sql
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy')
```
6. Show the countries which have a `name` that includes the word 'United'
``` sql
SELECT name
FROM world
WHERE name LIKE '%United%'
```
7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
Show the countries that are big by area or big by population. Show name, population and area.
``` sql
SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000
```
8. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.
* Australia has a big area but a small population, it should be included.
* Indonesia has a big population but a small area, it should be included.
* China has a big population and big area, it should be excluded.
* United Kingdom has a small population and a small area, it should be excluded.
``` sql
SELECT name, population, area
FROM world
WHERE (area > 3000000 AND population < 250000000)
OR (area < 3000000 AND population > 250000000)
```
9. Show the `name` and `population` in millions and the GDP in billions for the countries of the `continent` 'South America'. Use the ROUND function to show the values to two decimal places.
For South America show population in millions and GDP in billions both to 2 decimal places.
``` sql
SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2)
FROM world
WHERE continent = 'South America'
```
10. Show the `name` and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.
Show per-capita GDP for the trillion dollar countries to the nearest $1000.
``` sql
SELECT name, ROUND(gdp/population, -3)
FROM world
WHERE gdp > 1000000000000
```
11. Greece has capital Athens.
Each of the strings 'Greece', and 'Athens' has 6 characters.
Show the name and capital where the name and the capital have the same number of characters.
* You can use the LENGTH function to find the number of characters in a string
``` sql
SELECT name, capital
FROM world
WHERE LENGTH(name) = LENGTH(capital)
```
12. The capital of Sweden is Stockholm. Both words start with the letter 'S'.
Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
* You can use the function LEFT to isolate the first character.
* You can use `<>` as the NOT EQUALS operator.
``` sql
SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1)
AND name <> capital
```
13. Equatorial Guinea and Dominican Republic have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name.
Find the country that has all the vowels and no spaces in its name.
* You can use the phrase `name NOT LIKE '%a%'` to exclude characters from your results.
* The query shown misses countries like Bahamas and Belarus because they contain at least one 'a'
``` sql
SELECT name
FROM world
WHERE name LIKE '%a%'
AND name LIKE '%e%'
AND name LIKE '%i%'
AND name LIKE '%o%'
AND name LIKE '%u%'
AND name NOT LIKE '% %'
```







## Quiz 1
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

## Quiz 2
1. Select the code which gives the name of countries beginning with U
``` sql
SELECT name
  FROM world
 WHERE name LIKE 'U%'
```

2. Select the code which shows just the population of United Kingdom?
``` sql
SELECT population
  FROM world
 WHERE name = 'United Kingdom'
```

3. Select the answer which shows the problem with this SQL code - the intended result should be the continent of France:
``` sql
SELECT continent 
   FROM world 
  WHERE 'name' = 'France'
```
'name' should be name<br>

4. Select the result that would be obtained from the following code:
``` sql
SELECT name, population / 10 
  FROM world 
 WHERE population < 10000
```
Nauru	990<br>

5. Select the code which would reveal the name and population of countries in Europe and Asia
``` sql
SELECT name, population
  FROM world
 WHERE continent IN ('Europe', 'Asia')
```

6. Select the code which would give two rows
``` sql
SELECT name FROM world
 WHERE name IN ('Cuba', 'Togo')
```

7. Select the result that would be obtained from this code:
``` sql
SELECT name FROM world
 WHERE continent = 'South America'
   AND population > 40000000
```
Brazil    <br>
Colombia  <br>
