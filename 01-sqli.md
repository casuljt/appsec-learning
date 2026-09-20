# SQL Injection

## What is it

User-controlled input is inserted into SQL queries.

## Example

SELECT * FROM users
WHERE username='$user'

## Impact

- Authentication bypass
- Data leakage
- Data modification

## Prevention

- Prepared Statements
- Parameterized Queries
- ORM

## What I learned

I learnt how UNION attacks work and how attackers enumerate database structures.

##Labs

#### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- https://insecure-website.com/products?category=Gifts'--
#### SQL injection vulnerability allowing login bypass
#### SQL injection attack, querying the database type and version on Oracle
#### SQL injection attack, querying the database type and version on MySQL and Microsoft
#### SQL injection attack, listing the database contents on non-Oracle databases
#### SQL injection attack, listing the database contents on Oracle
#### SQL injection UNION attack, determining the number of columns returned by the query
#### SQL injection UNION attack, finding a column containing text
#### SQL injection UNION attack, retrieving data from other tables
#### SQL injection UNION attack, retrieving multiple values in a single column
#### Blind SQL injection with conditional responses
#### Blind SQL injection with conditional errors
#### Visible error-based SQL injection
