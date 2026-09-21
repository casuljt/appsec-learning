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


