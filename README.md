# SQL for Data Analysis
## Reading Tables:

* **SELECT and FROM**
The most basic SELECT query follows the pattern SELECT…FROM <table_name>;. This query is the way to pull data from a single table. For example, if you want to pull all the data from the products table in our sample database, simply use this query:
```sql 
SELECT * FROM products
```
If we want to return only specific columns from a query, we can simply replace the asteriskwith the names of the columns we want to be separated in the order we want them tobe returned in. For example, if we wanted to return the product_id column followed bythe model column of the products table, we would write the following query:
```sql 
SELECT product_id, model FROM products;
```
If we wanted to return the model column first and the product_id column second, we would write this:
```sql 
SELECT model, product_id,  FROM products;
```
* **WHERE** The WHERE clause is a piece of conditional logic that limits the amount of data returned. FROM filters the columns & WHERE filters the rows. Let's say we wanted to see the model names of our products with the model year of 2014 from our sample dataset. We would write the following query:
```sql 
SELECT model FROM products WHERE year=2014;
```
* **AND/OR** The previous query had only one condition. We are often interested in multiple conditions being met at once. For this, we put multiple statements together using the AND or OR clause. Now, we will illustrate this with an example. Let's say we wanted to return models
that not only were built in 2014, but also have a manufacturer's suggested retail price (MSRP) of less than $1,000. We can write:
```sql 
SELECT model FROM products WHERE year=2014 AND msrp<=1000;
```
Now, let's say we wanted to return models that were released in the year 2014 or had a product type of automobile. We would then write the following query:
```sql 
SELECT model FROM products WHERE year=2014 OR product_type='automobile';
```
When using more than one AND/OR condition, use parentheses to separate and position pieces of logic together. This will make sure that your query works as expected and that it is as readable as possible. For example, if we wanted to get all products with models in the years between 2014 and 2016, as well as any products that are scooters, we could write:
```sql 
SELECT * FROM products WHERE year>2014 AND year<2016 OR product_type='scooter';
```
However, to clarify the WHERE clause, it would be preferable to write:
```sql 
SELECT * FROM products WHERE (year>2014 AND year<2016) OR product_type='scooter';
```
* **IN/NOT IN** For instance, let's sayyou were interested in returning all models with the year 2014, 2016, or 2019. You could write a query such as this:
```sql 
SELECT * FROM products WHERE WHERE year = 2014 OR year = 2016 OR year = 2019;
```
However, this is long and tedious to write. Using IN, you can instead write:
```sql 
SELECT model FROM products WHERE year IN (2014, 2016, 2019);
```
Conversely, you can also use the NOT IN clause to return all values that are not in a list of values. For instance, if you wanted all products that were not produced in the years 2014, 2016, and 2019, you could write:
```sql 
SELECT model FROM products WHERE year NOT IN (2014, 2016, 2019);
```
* **ORDER BY** Let's say you want to see all of the products listed by the date when they were first produced, from earliest to latest. The method for doing this in SQL would be as follows:
```sql 
SELECT model FROM products ORDER BY production_start_date;
```
If an order sequence is not explicitly mentioned, the rows will be returned in ascending order. Ascending order simply means the rows will be ordered from the smallest value to the highest value of the chosen column or columns. In the case of things such as text,
this means alphabetical order. You can make the ascending order explicit by using the ASC keyword. For our last query, this would be achieved by writing:
```sql 
SELECT model FROM products ORDER BY production_start_date ASC;
```
If you would like to extract data in greatest-to-least order, you can use the DESC keyword. If we wanted to fetch manufactured models ordered from newest to oldest, we would write:
```sql 
SELECT model FROM products ORDER BY production_start_date DESC;
```
Also, instead of writing the name of the column you want to order by, you can instead refer to what number column it is in the natural order of the table. For instance, say you wanted to return all the models in the products table ordered by product ID. You couldwrite:
```sql
SELECT modelFROM products ORDER BY product_id;
```
However, because product_id is the first column in the table, you could instead write:
```sql
SELECT model FROM products ORDER BY 1;
```
Finally, you can order by multiple columns by adding additional columns after ORDER BY separated with commas. For instance, let's say we wanted to order all of the rows in the table first by the year of the model, from newest to oldest, and then by the MSRP from least to greatest. We would then write:
```sql
SELECT * FROM products ORDER BY year DESC, base_msrp ASC;
```
* **LIMIT** Sometimes, you may want only the first few rows. For this scenario, the LIMIT keyword comes in handy. Let's imagine that you wanted to only get the first five products that were produced by the company. You could get this by using the following query:
```sql
SELECT model FROM products ORDER BY production_start_date LIMIT 5;
```
* **IS NULL/IS NOT NULL** Whatever the reason, we are often interested in finding rows where the data is not filled in for a certain value. In SQL, blank values are often represented by the NULL value. For instance, in the products table,the production_end_date column having a NULL value indicates that the product is still being made. In this case, if we want to list all products that are still being made, we canuse the following query:
```sql
SELECT model FROM products WHERE production_end_date IS NULL;
```
If we are only interested in products that are not being produced, we can use the IS NOT NULL clause, as in the following query:
```sql
SELECT model FROM products WHERE production_end_date IS NOT NULL;
```
* **Exercise:** Create a list of the online usernames of the first 10 female salespeople hired, ordered from the first hired to the latest hired.
```sql
SELECT username FROM salespeople WHERE gender= 'Female' ORDER BY hire_date LIMIT 10
```
* **Exercise:** Write a query that pulls all emails for ZoomZoom customers in the state of Florida in alphabetical order.
```sql
SELECT email FROM customers WHERE city= 'Florida' ORDER BY email
```
* **Exercise:** Write a query that pulls all the first names, last names and email details for ZoomZoom customers in New York City in the state of New York. They should be ordered alphabetically by the last name followed by the first name.
```sql
SELECT first name, last name, email FROM customers WHERE city= 'New York' ORDER BY last name, first name;
```
* **Exercise:** Write a query that returns all customers with a phone number ordered by the date the customer was added to the database.
```sql
SELECT * FROM customers WHERE phone IS NOT NULL ORDER BY date_added;
```
## Creating Tables
There are fundamentally two ways to create tables: creating blank tables or using SELECT queries.
```sql
CREATE TABLE {table_name} (
{column_name_1} {data_type_1} {column_constraint_1},
{column_name_2} {data_type_2} {column_constraint_2},
{column_name_3} {data_type_3} {column_constraint_3},
…
{column_name_last} {data_type_last} {column_constraint_last},
);
```
Here {table_name} is the name of the table, {column_name} is the name of the column, {data_type} is the data type of the column, and {column_constraint} is one or more optional keywords giving special properties to the column. Before we discuss how to use the CREATE TABLE query, we will first discuss column constraints.

**Column constraints** are keywords that give special properties to a column. Some major column constraints are:
* **NOT NULL**: This constraint guarantees that no value in a column can be null.
* **UNIQUE**: This constraint guarantees that every single row for a column has a unique value and that no value is repeated.
* **PRIMARY KEY**: This is a special constraint that is unique for each row and helps to find the row quicker. Only one column in a table can be a primary key.

Suppose we want to create a table called state_populations, and it has columns with states' initials and populations. The query would look like this:
```sql
CREATE TABLE state_populations (^state VARCHAR(2) PRIMARY KEY, population NUMERIC);
```
* **Exercise:** The marketing team at ZoomZoom would like to create a table called countries to analyze the data of different countries. It should have four columns: an integer key column, a unique name column, a founding year column, and a capital column.
```sql
CREATE TABLE countries (key INT PRIMARY KEY, name text UNIQUE, founding_year INT, capital text);
```
* **Creating Tables with SELECT**
```sql
CREATE TABLE {table_name} AS ({select_query});
```
Here, {select_query} is any SELECT query that can be run in your database. For instance, say you wanted to create a table based on the products table that only had products from the year 2014. Let's call this table products_2014. You could then write the following query:
```sql
CREATE TABLE products_2014 AS (SELECT * FROM products WHERE year=2014);
```
* **Updating Tables**
* Adding and Removing Columns To add new columns to an existing table, we use the ADD COLUMN statement as in the following query:
```sql
ALTER TABLE {table_name}
ADD COLUMN {column_name} {data_type};
```
Let's say, for example, that we wanted to add a new column to the products table that we will use to store the products' weight in kilograms called weight. We could do this by using the following query:
```sql
ALTER TABLE products ADD COLUMN weight INT;
```
If you want to remove a column from a table, you can use the DROP column statement:
```sql
ALTER TABLE {table_name}
DROP COLUMN {column_name};
```
Let's imagine that you decide to delete the weight column you just created. You could get rid of it using the following query:
```sql
ALTER TABLE products
DROP COLUMN weight;
```
* Adding New Data: You can add new data in a table using several methods in SQL. One method is to simply insert values straight into a table using the INSERT INTO…VALUES statement. It has the following structure:
```sql
INSERT INTO {table_name} ({column_1], {column_2}, …{column_last}) VALUES ({column_value_1}, {column_value_2}, … {column_value_last});
```
Here, {table_name} is the name of the table you want to insert your data into, {column_1}, {column_2}, … {column_last} is a list of the columns whose values you want to insert, and {column_value_1}, {column_value_2}, … {column_value_last} is the values of the rows you want to insert into the table. If a column in the table is not put into the INSERT statement, the column is assumed to have a NULL value. As an example, let's say you wanted to insert a new scooter into the products table. This could be done with the following query:
```sql
INSERT INTO products (product_id, model, year, product_type, base_msrp, production_start_date, production_end_date)
VALUES (13, "Nimbus 5000", 2019, 'scooter', 500.00, '2019-03-03', '2020-03-03');
```
Another way to insert data into a table is to use the INSERT statement with a SELECT query using the following syntax:
```sql
INSERT INTO {table_name} ({column_1], {column_2}, …{column_last}){select_query};
```
Here, {table_name} is the name of the table into which you want to insert the data,{column_1}, {column_2}, … {column_last} is a list of the columns whose values you want to insert, and {select query} is a query with the same structure as the values you want to insert into the table. Take the example of the products_2014 table we discussed earlier. Imagine that instead of creating it with a SELECT query, we created it as a blank table with the same structure as the products table. If we wanted to insert the same data as we did earlier, we could use the following query:
```sql
INSERT INTO products (product_id, model, year, product_type, base_msrp, production_start_date, production_end_date)
SELECT * FROM products WHERE year=2014;
```
* **Updating Existing Rows** Sometimes, you may need to update the values of the data present in a table. To do this, you can use the UPDATE statement:
```sql
UPDATE {table_name}
SET {column_1} = {column_value_1},
{column_2} = {column_value_2},
...
{column_last} = {{column_value_last}}
WHERE
{conditional};
```
Here, {table_name} is the name of the table with data that will be changed, {column_1},{column_2},… {column_last} is the columns whose values you want to change, {column_value_1}, {column_value_2},… {column_value_last} is the new values you want to insertinto those columns, and {WHERE} is a conditional statement like one you would find in aSQL query. To illustrate its use of the update statement, let's say that for the rest of the year, the company has decided to sell all scooter models before 2018 for $299.99. We could change the data in the products table using the following query:
```sql
UPDATE productsSET base_msrp = 299.99, WHERE product_type = 'scooter'AND year<2018;
```
* **Exercise:** Our goal in this exercise is to update the data in a table using the UPDATE statement. Dueto the higher cost of rare metals needed to manufacture an electric vehicle, the new2019 Model Chi will need to undergo a price hike of 10%. Update the products table to increase the price of this product.
```sql
UPDATE productsSET base_msrp = base_msrp*1.10 WHERE model='Model Chi'and year=2019;
```
* **Deleting Values from a Row** Often, we will be interested in deleting a value in a row. The easiest way to accomplish this task is to use the UPDATE structure we already discussed and to set the column value to NULL like so:
```sql
UPDATE {table_name}
SET {column_1} = NULL,
{column_2} = NULL,
...
{column_last} = NULL
WHERE
{conditional};
```
Here, {table_name} is the name of the table with the data that needs to be changed, {column_1}, {column_2},… {column_last} is the columns whose values you want to delete, and {WHERE} is a conditional statement like one you would find in a SQL query. Let's say, for instance, that we have the wrong email on file for the customer with the customer ID equal to 3. To fix that, we can use the following query:
```sql
UPDATE customers SET email = NULL WHERE customer_id=3;
```
Deleting a row from a table can be done using the DELETE statement, which looks like this:
```sql
DELETE FROM {table_name} WHERE {conditional}; DELETE FROM customers WHERE email='bjordan2@geocities.com';
```
If we wanted to delete all the data in the customers table without deleting the table, we could write the following query:
```sql
DELETE FROM customers;
```
Alternatively, if you want to delete all the data in a query without deleting the table, you could use the TRUNCATE keyword as follows:
```sql
TRUNCATE TABLE customers;
```
To delete the table along with the data completely, you can just use the DROP TABLE statement with the following syntax:
```sql
DROP TABLE {table_name};
```
Here, {table_name} is the name of the table you want to delete. If we wanted to delete all the data in the customers table along with the table itself, we would write:
```sql
DROP TABLE customers;
```
## Assembling Data
* **Connecting Tables Using JOIN** Majority of the time, the data you are interested in is spread across multiple tables. Fortunately, SQL has methods for bringing related tables together using the JOIN keyword.
* **INNER JOIN**
```sql
SELECT * FROM salespeople INNER JOIN dealerships ON salespeople.dealership_id = dealerships.dealership_id ORDER BY 1;
```
