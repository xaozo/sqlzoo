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

## SELECT from Nobel
1. Change the query shown so that it displays Nobel prizes for 1950.
``` sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```
2. Show who won the 1962 prize for Literature.
``` sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
```
3. Show the year and subject that won 'Albert Einstein' his prize.
``` sql
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'
```
4. Give the name of the 'Peace' winners since the year 2000, including 2000.
``` sql
SELECT winner
FROM nobel
WHERE yr >= 2000
AND subject = 'Peace'
```
5. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
``` sql
SELECT * 
FROM nobel
WHERE subject = 'Literature'
AND yr BETWEEN 1980 AND 1989
```
6. Show all details of the presidential winners:
* Theodore Roosevelt
* Woodrow Wilson
* Jimmy Carter
* Barack Obama
``` sql
SELECT name
FROM world
WHERE name LIKE '%United%'
```
7. Show the winners with first name John
``` sql
SELECT winner FROM nobel
WHERE winner LIKE 'John%'
```
8. Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.
``` sql
SELECT * FROM nobel
WHERE (subject = 'Physics' AND yr = 1980)
OR (subject = 'Chemistry' AND yr = 1984)
```
9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine
``` sql
SELECT * FROM nobel
WHERE yr = 1980
AND subject NOT IN ('Chemistry', 'Medicine')
```
10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
``` sql
SELECT * FROM nobel
WHERE (subject = 'Medicine' AND yr < 1910)
OR (subject = 'Literature' AND yr >= 2004)
```
11. Find all details of the prize won by PETER GRÜNBERG
``` sql
SELECT * FROM nobel
WHERE winner = 'Peter Grünberg'
```
12. Find all details of the prize won by EUGENE O'NEILL
``` sql
SELECT * FROM nobel
WHERE winner = 'Eugene O''Neill'
```
13. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
``` sql
SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner
```
14. The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.
Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
``` sql
SELECT winner, subject FROM nobel
WHERE yr = 1984
ORDER BY subject IN ('Chemistry', 'Physics'), subject, winner
```

## SELECT within SELECT
1. List each country name where the population is larger than that of 'Russia'.
``` sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```
2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
``` sql
SELECT name FROM world
WHERE gdp/population > 
(SELECT gdp/population FROM world
WHERE name = 'United Kingdom')
AND continent = 'Europe'
```
3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
``` sql
SELECT name, continent FROM world
WHERE continent IN
(SELECT continent FROM world
WHERE name = 'Argentina' OR name = 'Australia')
ORDER BY name
```
4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
``` sql
SELECT name, population FROM world
WHERE population >
(SELECT population FROM world WHERE name = 'Canada')
AND population <
(SELECT population FROM world WHERE name = 'Poland')
```
5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
The format should be Name, Percentage for example:
name	percentage<br>
Albania	3%<br>
Andorra	0%<br>
Austria	11%<br>
...	...<br>
``` sql
SELECT * 
FROM nobel
WHERE subject = 'Literature'
AND yr BETWEEN 1980 AND 1989
```
6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
``` sql
SELECT name FROM world
WHERE gdp > ALL(SELECT gdp FROM world WHERE continent = 'Europe' AND gdp > 0)
```
7. Find the largest country (by area) in each continent, show the continent, the name and the area:
``` sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```
8. List each continent and the name of the country that comes first alphabetically.
``` sql
SELECT continent, name FROM world x
  WHERE name <= ALL
    (SELECT name FROM world y
        WHERE y.continent=x.continent)
```
9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
``` sql
SELECT name, continent, population FROM world x
WHERE 25000000 >= ALL(
SELECT population FROM world y WHERE x.continent = y.continent AND population > 0)
```
10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
``` sql
SELECT name, continent FROM world x
WHERE population >= ALL(
    SELECT 3*population FROM world y
    WHERE x.continent = y.continent
    AND x.name <> y.name
    AND population > 0)
```

## SUM and COUNT
1. Show the total population of the world.
``` sql
SELECT SUM(population)
FROM world
```
2. List all the continents - just once each.
``` sql
SELECT DISTINCT continent FROM world
```
3. Give the total GDP of Africa
``` sql
SELECT SUM(gdp) FROM world
WHERE continent = 'Africa'
```
4. How many countries have an area of at least 1000000
``` sql
SELECT COUNT(*) FROM world
WHERE area >= 1000000
```
5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')
``` sql
SELECT SUM(population) FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```
6. For each continent show the continent and number of countries.
``` sql
SELECT continent, COUNT(*)
FROM world
GROUP BY continent
```
7. For each continent show the continent and number of countries with populations of at least 10 million.
``` sql
SELECT continent, COUNT(*)
FROM world
WHERE population >= 10000000
GROUP BY continent
```
8. List the continents that have a total population of at least 100 million.
``` sql
SELECT continent FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```

## JOIN
1. The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime
Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
``` sql
SELECT matchid, player FROM goal 
  WHERE teamid = 'GER'
```
2. From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.
Notice in the that the column matchid in the goal table corresponds to the id column in the game table. We can look up information about game 1012 by finding that row in the game table.

Show id, stadium, team1, team2 for just game 1012
``` sql
SELECT id,stadium,team1,team2
  FROM game
WHERE id = 1012
```
3. You can combine the two steps into a single query with a `JOIN`.
``` sql
SELECT *
  FROM game JOIN goal ON (id=matchid)
```
The FROM clause says to merge data from the goal table with that from the game table. The ON says how to figure out which rows in game go with which rows in goal - the matchid from goal must match id from game. (If we wanted to be more clear/specific we could say
ON (game.id=goal.matchid)
The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.
Modify it to show the player, teamid, stadium and mdate for every German goal.
``` sql
SELECT player, teamid, stadium, mdate
  FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
```
4. Use the same JOIN as in the previous question.
Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
``` sql
SELECT team1, team2, player 
FROM game JOIN goal ON id=matchid
WHERE player LIKE 'Mario%'
```
5. The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id
Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
``` sql
SELECT player, teamid, coach, gtime
  FROM goal JOIN eteam ON teamid=id
 WHERE gtime<=10
```
6. To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)
Notice that because id is a column name in both game and eteam you must specify eteam.id instead of just id
List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
``` sql
SELECT mdate, teamname
FROM game JOIN eteam ON (team1=eteam.id)
WHERE coach = 'Fernando Santos'
```
7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
``` sql
SELECT player
FROM goal JOIN game ON (matchid=id)
WHERE stadium = 'National Stadium, Warsaw'
```
8. The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.
``` sql
SELECT DISTINCT player
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER')
AND teamid != 'GER'
```
9. Show teamname and the total number of goals scored.
``` sql
SELECT teamname, COUNT(*)
  FROM eteam JOIN goal ON id=teamid
 GROUP BY teamname
```
10. Show the stadium and the number of goals scored in each stadium.
``` sql
SELECT stadium, COUNT(*)
FROM goal JOIN game ON matchid = id
GROUP BY stadium
```
11. For every match involving 'POL', show the matchid, date and the number of goals scored.
``` sql
SELECT matchid, mdate, COUNT(*)
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid, mdate
```
12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
``` sql
SELECT matchid, mdate, COUNT(*)
FROM goal JOIN game ON matchid = id 
WHERE teamid = 'GER'
GROUP BY matchid, mdate
```
13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.
``` sql
SELECT mdate,
  team1,
 SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) AS score1,
  team2,
 SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) AS score2
  FROM game LEFT JOIN goal ON matchid = id
GROUP BY mdate, matchid, team1, team2
```

## More JOIN Operations
1. List the films where the yr is 1962 \[Show id, title\]
``` sql
SELECT id, title
 FROM movie
 WHERE yr=1962
```
2. Give year of 'Citizen Kane'.
``` sql
SELECT yr FROM movie 
WHERE title = 'Citizen Kane'
```
3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
``` sql
SELECT id, title, yr FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr
```
4. What id number does the actor 'Glenn Close' have?
``` sql
SELECT id FROM actor
WHERE name = 'Glenn Close'
```
5. What is the id of the film 'Casablanca'
``` sql
SELECT id FROM movie
WHERE title = 'Casablanca'
```
6. Obtain the cast list for 'Casablanca'.
Use movieid=11768, (or whatever value you got from the previous question)
``` sql
SELECT name FROM movie
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE movieid=11768
```
7. Obtain the cast list for the film 'Alien'
``` sql
SELECT name FROM movie
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE title = 'Alien'
```
8. List the films in which 'Harrison Ford' has appeared
``` sql
SELECT title FROM movie
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE name = 'Harrison Ford'
```
9. List the films where 'Harrison Ford' has appeared - but not in the starring role. \[Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role\]
``` sql
SELECT title FROM movie
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE name = 'Harrison Ford'
AND ord != 1
```
10. List the films together with the leading star for all 1962 films.
``` sql
SELECT title, name FROM movie
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE yr = 1962 AND ord = 1
```
11. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
``` sql
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
        JOIN actor   ON actorid=actor.id
WHERE name='Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2
```
12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
``` sql
SELECT title, name FROM movie
JOIN casting ON id=movieid
JOIN actor ON actorid=actor.id
WHERE ord = 1 
AND movieid IN (
  SELECT movieid FROM actor
  JOIN casting ON actor.id=casting.actorid
  WHERE name='Julie Andrews')
```
13. Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles.
``` sql
SELECT name FROM actor
JOIN casting ON id=actorid
WHERE ord=1
GROUP BY name
HAVING COUNT(*) >= 15
```
14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
``` sql
SELECT title, COUNT(*) FROM movie
JOIN casting ON id=movieid
where yr = 1978
GROUP BY title
ORDER BY COUNT(*) DESC, title
```
15. List all the people who have worked with 'Art Garfunkel'.
``` sql
SELECT name FROM actor
JOIN casting ON actor.id=casting.actorid
JOIN movie ON casting.movieid=movie.id
WHERE movieid IN
(SELECT movieid FROM actor
JOIN casting ON id=actorid
WHERE name = 'Art Garfunkel')
AND name != 'Art Garfunkel'
```

## Using NULL
1. List the teachers who have NULL for their department.
``` sql
SELECT name FROM teacher
WHERE dept IS NULL
```
2. Note the INNER JOIN misses the teachers with no department and the departments with no teacher.
``` sql
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
```
3. Use a different JOIN so that all teachers are listed.
``` sql
SELECT teacher.name, dept.name
 FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)
```
4. Use a different JOIN so that all departments are listed.
``` sql
SELECT teacher.name, dept.name
 FROM teacher RIGHT JOIN dept
           ON (teacher.dept=dept.id)
```
5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher name and mobile number or '07986 444 2266'
``` sql
SELECT name, COALESCE(mobile, '07986 444 2266')
FROM teacher
```
6. Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.
``` sql
SELECT teacher.name, COALESCE(dept.name, 'None')
FROM teacher
LEFT JOIN dept ON teacher.dept=dept.id
```
7. Use COUNT to show the number of teachers and the number of mobile phones.
``` sql
SELECT COUNT(name), COUNT(mobile) FROM teacher
```
8. Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.
``` sql
SELECT dept.name, COUNT(teacher.name) FROM teacher
RIGHT JOIN dept ON teacher.dept=dept.id
GROUP BY dept.name
```
9. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.
``` sql
SELECT name,
CASE WHEN dept IN (1,2) THEN 'Sci' ELSE 'Art' END
FROM teacher
```
10. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.
``` sql
SELECT name,
CASE WHEN dept IN (1,2) THEN 'Sci'
    WHEN dept=3 THEN 'Art'
    ELSE 'None' END
FROM teacher
```

## Numeric examples
1. The example shows the number who responded for:
* question 1
* at 'Edinburgh Napier University'
* studying '(8) Computer Science'
Show the the percentage who STRONGLY AGREE
``` sql
SELECT A_STRONGLY_AGREE
  FROM nss
 WHERE question='Q01'
   AND institution='Edinburgh Napier University'
   AND subject='(8) Computer Science'
```
2. Show the institution and subject where the score is at least 100 for question 15.
``` sql
SELECT institution, subject
  FROM nss
 WHERE question='Q15'
   AND score >= 100
```
3. Show the institution and score where the score for '(8) Computer Science' is less than 50 for question 'Q15'
``` sql
SELECT institution,score
  FROM nss
 WHERE question='Q15'
   AND subject='(8) Computer Science'
   AND score < 50
```
4. Show the subject and total number of students who responded to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.
``` sql
SELECT subject, SUM(response)
  FROM nss
 WHERE question='Q22'
GROUP BY subject
HAVING subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
```
5. Show the subject and total number of students who A_STRONGLY_AGREE to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.
``` sql
SELECT subject, SUM(A_STRONGLY_AGREE/100*response)
  FROM nss
 WHERE question='Q22'
GROUP BY subject 
HAVING subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
```
6. Show the percentage of students who A_STRONGLY_AGREE to question 22 for the subject '(8) Computer Science' show the same figure for the subject '(H) Creative Arts and Design'.
Use the ROUND function to show the percentage without decimal places.
``` sql
SELECT subject,
ROUND(SUM(A_STRONGLY_AGREE/100*response)/SUM(response)*100, 0)
  FROM nss
 WHERE question='Q22'
GROUP BY subject 
HAVING subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
```
7. Show the average scores for question 'Q22' for each institution that include 'Manchester' in the name.
The column score is a percentage - you must use the method outlined above to multiply the percentage by the response and divide by the total response. Give your answer rounded to the nearest whole number.
``` sql
SELECT institution, 
ROUND(SUM(score/100*response)/SUM(response)*100)
  FROM nss
 WHERE question='Q22'
   AND (institution LIKE '%Manchester%')
GROUP BY institution
ORDER BY institution
```
8. Show the institution, the total sample size and the number of computing students for institutions in Manchester for 'Q01'.
``` sql
SELECT institution, SUM(sample),
SUM(CASE WHEN subject = '(8) Computer Science' THEN sample ELSE 0 END)
AS comp
FROM nss
WHERE question='Q22' AND institution LIKE '%Manchester%'
GROUP BY institution
```

## Window functions
1. Show the lastName, party and votes for the constituency 'S14000024' in 2017.
``` sql
SELECT lastName, party, votes
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY votes DESC
```
2. You can use the RANK function to see the order of the candidates. If you RANK using (ORDER BY votes DESC) then the candidate with the most votes has rank 1.
Show the party and RANK for constituency S14000024 in 2017. List the output by party
``` sql
SELECT party, votes,
       RANK() OVER (ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY party
```
3. The 2015 election is a different PARTITION to the 2017 election. We only care about the order of votes for each year.
Use PARTITION to show the ranking of each party in S14000021 in each year. Include yr, party, votes and ranking (the party with the most votes is 1).
``` sql
SELECT yr,party, votes,
      RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000021'
ORDER BY party,yr
```
4. Edinburgh constituencies are numbered S14000021 to S14000026.
Use PARTITION BY constituency to show the ranking of each party in Edinburgh in 2017. Order your results so the winners are shown first, then ordered by constituency.
``` sql
SELECT constituency,party, votes,
RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) AS rank
  FROM ge
 WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
   AND yr  = 2017
ORDER BY rank, constituency
```
5. You can use SELECT within SELECT to pick out only the winners in Edinburgh.
Show the parties that won for each Edinburgh constituency in 2017.
``` sql
SELECT constituency, party
  FROM ge AS a
 WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
   AND yr = 2017
   AND votes >= ALL(
SELECT votes FROM ge AS b
WHERE a.constituency=b.constituency
AND yr = 2017 AND votes > 0)
ORDER BY constituency,votes DESC
```
6. You can use COUNT and GROUP BY to see how each party did in Scotland. Scottish constituencies start with 'S'
Show how many seats for each party in Scotland in 2017.
``` sql
SELECT party, COUNT(party)
  FROM ge AS a
 WHERE constituency LIKE 'S%'
   AND yr = 2017
   AND votes >= ALL(
SELECT votes FROM ge AS b
WHERE a.constituency=b.constituency
AND yr = 2017 AND votes > 0)
GROUP BY party
```

## COVID 19
1. The example uses a WHERE clause to show the cases in 'Italy' in March.
Modify the query to show data from Spain
``` sql
SELECT name, DAY(whn),
 confirmed, deaths, recovered
 FROM covid
WHERE name = 'Spain'
AND MONTH(whn) = 3
ORDER BY whn
```
2. The LAG function is used to show data from the preceding row or the table. When lining up rows the data is partitioned by country name and ordered by the data whn. That means that only data from Italy is considered.
Modify the query to show confirmed for the day before.
``` sql
SELECT name, DAY(whn), confirmed,
   LAG(confirmed, 1) OVER (PARTITION BY name ORDER BY whn) AS lag
 FROM covid
WHERE name = 'Italy'
AND MONTH(whn) = 3
ORDER BY whn
```
3. The number of confirmed case is cumulative - but we can use LAG to recover the number of new cases reported for each day.
Show the number of new cases for each day, for Italy, for March.
``` sql
SELECT name, DAY(whn),
   (confirmed - LAG(confirmed, 1) OVER (PARTITION BY name ORDER BY whn)) AS new
 FROM covid
WHERE name = 'Italy'
AND MONTH(whn) = 3
ORDER BY whn
```
4. The data gathered are necessarily estimates and are inaccurate. However by taking a longer time span we can mitigate some of the effects.
You can filter the data to view only Monday's figures WHERE WEEKDAY(whn) = 0.
Show the number of new cases in Italy for each week - show Monday only.
``` sql
SELECT name, DATE_FORMAT(whn,'%Y-%m-%d'),
 (confirmed - LAG(confirmed, 1) OVER (PARTITION BY name ORDER BY whn)) AS "new this week"
 FROM covid
WHERE name = 'Italy'
AND WEEKDAY(whn) = 0
ORDER BY whn
```
5. You can JOIN a table using DATE arithmetic. This will give different results if data is missing.
Show the number of new cases in Italy for each week - show Monday only.
In the sample query we JOIN this week tw with last week lw using the DATE_ADD function.
``` sql
SELECT tw.name, DATE_FORMAT(tw.whn,'%Y-%m-%d'), 
 (tw.confirmed - lw.confirmed)
 FROM covid tw LEFT JOIN covid lw ON 
  DATE_ADD(lw.whn, INTERVAL 1 WEEK) = tw.whn
   AND tw.name=lw.name
WHERE tw.name = 'Italy'
AND WEEKDAY(tw.whn) = 0
ORDER BY tw.whn
```
6. The query shown shows the number of confirmed cases together with the world ranking for cases.
United States has the highest number, Spain is number 2...
Notice that while Spain has the second highest confirmed cases, Italy has the second highest number of deaths due to the virus.
Include the ranking for the number of deaths in the table.
``` sql
SELECT 
   name,
   confirmed,
   RANK() OVER (ORDER BY confirmed DESC) rc,
   deaths,
   RANK() OVER (ORDER BY deaths DESC) rc
  FROM covid
WHERE whn = '2020-04-20'
ORDER BY confirmed DESC
```
7. The query shown includes a JOIN t the world table so we can access the total population of each country and calculate infection rates (in cases per 100,000).
Show the infect rate ranking for each country. Only include countries with a population of at least 10 million.
``` sql
SELECT 
   world.name,
   ROUND(100000*confirmed/population,0),
   RANK() OVER (ORDER BY confirmed/population) AS rank
  FROM covid JOIN world ON covid.name=world.name
WHERE whn = '2020-04-20' AND population > 10000000
ORDER BY population DESC
```
8. For each country that has had at last 1000 new cases in a single day, show the date of the peak number of new cases. **Note: gives the wrong order of rows**
``` sql
WITH a AS
(SELECT name,
   DATE_FORMAT(whn,'%Y-%m-%d') AS date,
   (confirmed - LAG(confirmed, 1) OVER (PARTITION BY 
   name ORDER BY whn)) AS new
   FROM covid)
SELECT * FROM a
WHERE (name, new) IN
(SELECT name, MAX(new) FROM a
WHERE new >= 1000 GROUP BY name)
ORDER BY date
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
