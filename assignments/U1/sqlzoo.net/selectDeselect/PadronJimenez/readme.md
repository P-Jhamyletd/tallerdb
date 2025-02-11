# ALUMNA: PADRON JIMENEZ NICOL JHAMYLETD

## 📍​Bigger than Russia
### 📍​📍​PROBLEMA 1: List each country name where the population is larger than that of 'Russia'.
world(name, continent, area, population, gdp)
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```

## 📍​Richer than UK
### 📍​📍​PROBLEMA 2: Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
SELECT name
FROM world
WHERE continent = 'Europe'
AND gdp / population > (
    SELECT gdp / population
    FROM world
    WHERE name = 'United Kingdom'
);
```

## 📍​Neighbours of Argentina and Australia
### 📍​📍​PROBLEMA 3: List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
SELECT name, continent
FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    WHERE name IN ('Argentina', 'Australia')
)
ORDER BY name;
```

## 📍​Between Canada and Poland
### 📍​📍​PROBLEMA 4: Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.
```sql
SELECT name, population
FROM world
WHERE population > (
    SELECT population 
    FROM world 
    WHERE name = 'United Kingdom'
) 
AND population < (
    SELECT population 
    FROM world 
    WHERE name = 'Germany'
);
```

## 📍​Percentages of Germany
### 📍​📍​PROBLEMA 5: Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT name, 
       CONCAT(ROUND((population * 100.0) / 
       (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage
FROM world
WHERE continent = 'Europe';
```
To gain an absurdly detailed view of one insignificant feature of the language, read on.
We can use the word ALL to allow >= or > or < or <=to act over a list. For example, you can find the largest country in the world, by population with this query:
```sql
SELECT name
  FROM world
 WHERE population >= ALL(SELECT population
                           FROM world
                          WHERE population>0)
```
You need the condition population>0 in the sub-query as some countries have null for population.

## 📍​Bigger than every country in Europe
### 📍​📍​PROBLEMA 6: Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```sql
  SELECT name
FROM world
WHERE gdp > (
    SELECT MAX(gdp) 
    FROM world 
    WHERE continent = 'Europe'
)
AND gdp IS NOT NULL;
```

## 📍​Largest in each continent
### 📍​📍​PROBLEMA 7: Find the largest country (by area) in each continent, show the continent, the name and the area:
The above example is known as a correlated or synchronized sub-query.
```sql
SELECT continent, name, area
FROM world w1
WHERE area = (
    SELECT MAX(area) 
    FROM world w2 
    WHERE w1.continent = w2.continent
);
```

## 📍​ First country of each continent (alphabetically)
### 📍​📍​PROBLEMA 8:
```sql
SELECT continent, name
FROM world w1
WHERE name = (
    SELECT MIN(name)
    FROM world w2
    WHERE w1.continent = w2.continent
);
```

## 📍​Difficult Questions That Utilize Techniques Not Covered In Prior Sections
### 📍​📍​PROBLEMA 9: Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000
);
```

## 📍​Three time bigger
### 📍​📍​PROBLEMA 10: Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continents.
```sql
SELECT name, continent
FROM world w1
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world w2
    WHERE w1.continent = w2.continent
    AND w1.name != w2.name
);
```



