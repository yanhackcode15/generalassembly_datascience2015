# Stuctured Query Language, Pt. 1

## Learning Objectives:
- What is SQL?
- Why do we need to know it?
- How do we use it?

## Setup

##### 1. in terminal, go to the directory containing world.sql

##### 2. open mysql as root
``mysql -u root``

if you have a password set, use:
``mysql -u <username> -p <password>``

##### 3. create a world database
``CREATE DATABASE world;``

##### 4. switch so the world database is active
``USE DATABASE world``

##### 4. import our sample database -- the cities, countries, and languages of the world
``source world.sql``

## Selecting

    Notes: Only single quotes work. If you get an error, make sure you're using single quotes.

###### select everything from this table
```SQL
SELECT * FROM Country;
```

###### select just certain columns
```SQL
SELECT Code,Name,Continent,Population FROM Country;
```

###### find one row, based on a country code
```SQL
SELECT Code,Name,Continent,Population FROM Country WHERE Code='ARG';
```

###### find any rows that are similar
```SQL
SELECT Code,Name,Continent,Population FROM Country WHERE Code LIKE 'AR%';
```

###### find all countries where there's a population of at least 100 million people
```SQL
SELECT Code,Name,Continent,Population FROM Country WHERE Population >= 100000000;
```


###### find only European countries whose population is more than 40 million
```SQL
SELECT Code,Name,Continent,Population FROM Country WHERE Continent='Europe' AND Population > 40000000;
```

###### sort those countries by population, from most to least
```SQL
SELECT Code,Name,Continent,Population FROM Country WHERE Continent='Europe' AND Population > 40000000 ORDER BY Population DESC;
```
