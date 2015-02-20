# Stuctured Query Language, Pt. 2


## SQL Command Review

- Start PSQL with a database pre-loaded
	- ``mysql -u <username> -p <password> <database name>``
- List all databases
	- ``SHOW DATABASES;``
- Change to a database
	- ``USE DATABASE <database name>;`` 
- Display all tables (and only tables)
	- ``SHOW TABLES;``
- Describe a table's structure
	- ``DESCRIBE <table name>`` 
- Quit PSQL
	- ``\q`` 

## Setup

We will use the world database from Part One.

##### 1. Use psql to open up our database
``mysql -u <username> -p <password> world``

##### 2. View all of the available tables
``SHOW TABLES``

##### 3. View the structure of our country table
``DESCRIBE Country``


## SQL Part II

Note: **All strings must have single quotes.** If you get an error, make sure you're using single quotes.

###SELECT ... IN

SELECT ... IN is very useful for website development, because users frequently choose a group of items that are not related by a single column. For example, suppose on your website the user selects which cities he has visited. We would then store this list of cities in the database via the city ids. We can then analyze this unique subset of all cities by using the IN operator:

```SQL
SELECT Name FROM City WHERE ID IN (3, 5, 7);
```

Another frequent use of IN is to exclude certain results. For example, when matching a user with recommended cities to visit, we will want to exclude those marked as previously visited. For example:

```SQL
SELECT Name FROM City WHERE ID NOT IN (3, 5, 7);
```

Instead of hard-coding the ids here, we would normally JOIN this table with the user's table, or (less optimally) place a **subquery** inside the IN( ... ). Here is an example of using a subquery (in this case, the primary key of the country table is "Code"):

###### select the names of all cities in Africa
```SQL
SELECT name
FROM city
WHERE countrycode IN (
	SELECT code FROM country WHERE continent='Africa'
);
```

### Making Generated Columns & Aliases

New columns can be created using math, but the resulting column will be lacking a name. By using an **alias**, we can give any column a different name. Note that multiple columns can be used in the column calculation; for example: (YearClosed - YearOpened) AS YearsOpen.

###### select the most recently created country
```SQL
SELECT Name, (2015 - Indepyear) AS YearsAgo
FROM Country
ORDER BY YearsAgo;
```

### Aggregate Functions

Aggregate functions take a group of rows and compute a single value.

#### COUNT(*)

Counts the number of rows that a query returns. 

Below, we count the number of rows in 'city' which have a countrycode of 'CHN' (China). Because we aggreggated all of the results, it does not make sense to also display a different column.
```SQL
SELECT COUNT(*) FROM City WHERE CountryCode='CHN';
```
#### MAX(column) 

Finds the value of the largest column value in all rows returned. Again, this aggregate value is an aggregated result and so it does not make sense to display additional columns. 
```SQL
SELECT MAX(Population) FROM City;
```

#### SUM(column) 

Sums a particular column in all rows returned.
```SQL
SELECT SUM(Population) FROM Country WHERE Continent='Africa';
```

####Multiple Aggregates / Aliases

You can use multiple aggregates at once, as long as you are aggregating over the same group of rows. Here, we return the number of countries in Africa, the largest population of a country in Africa, and the sum of all country populations in Africa. The names of aggregate columns are not always very descriptive, so we can **alias** each column to give it a more friendly name.
```SQL
SELECT COUNT(*) AS NumCountries,
       MAX(Population) AS LargestCountryPopulation, 
       SUM(Population) AS TotalPop 
FROM Country 
WHERE Continent='Africa';
```

####Subqueries
Subqueries allow us to use a queried table to make a new table! For example:

###### find how many cities have a population greater than Los Angeles (id=3794)
```SQL
SELECT COUNT(*) 
FROM City 
WHERE Population > (
	SELECT Population FROM City WHERE ID=3794
);
```


### GROUP BY - group fields that have the same value

GROUP BY is used to group rows together before feeding them into an aggregate function. For example, what if we want to find not just the total population of all countries, but instead find **for each continent** the total population (GROUP BY continent)? 

**_GROUP BY groups data by distinct values of a column, allowing an aggregate function to be applied to each._**

###### for each continent, find its total population
```SQL
SELECT SUM(Population) 
FROM Country 
GROUP BY Continent;
```

Here, each group of rows has a continent value, since the rows are grouped together by a unique continent. So, we can also add the continent name to the SELECT:
```SQL
SELECT Continent, SUM(Population) 
FROM Country 
GROUP BY Continent;
```

###### HAVING
What if we only want continents that have a population greater than zero? Normally you may think we would use WHERE; however, **WHERE** is used to select data before it is grouped and aggregated. (Hence, it cannot have aggregates in it!) The **HAVING** operator is used to select data after it is grouped and aggregated. Note that HAVING will operate on aggregate values, and you must repeat the specific aggregate function in the HAVING. For example:

```SQL
SELECT Continent, SUM(Population) 
FROM Country 
GROUP BY Continent
HAVING SUM(Population) > 0;
```


### JOINing Tables

For each city, let's display its country and population:

```SQL
SELECT Name, CountryCode, Population
FROM City
ORDER BY Population DESC;
```

This is great, but the countrycodes are hard to read. Instead of a countrycode, let's replace the countrycode column with the country name. But the country name - country code pairing is in the country table, not the city table.

To do this, we will JOIN the two tables together. The INNER JOIN creates a single extended table -- the new table only contains rows where the primary key and foreign key match. However, you now have access to all of the columns from both tables. If two columns are the same name, you must put the name of the table in front, for example "city.name"

```SQL
SELECT City.Name AS CityName, Country.Name AS CountryName, City.Population
FROM City
INNER JOIN Country
	ON City.CountryCode = Country.Code
ORDER BY Population DESC;
```

We can INNER JOIN more than one table together as well! For example, suppose we want to list each city, the country it's in, and all of its languages. Note that here, multiple countrylanguage entries exist for each country, so each city/country pair will have multiple entries in the final table:

```SQL
SELECT City.Name AS CityName, Country.Name AS CountryName, Language
FROM City
INNER JOIN Country
	ON City.CountryCode = Country.Code
INNER JOIN CountryLanguage
	ON City.CountryCode = CountryLanguage.CountryCode;
```


