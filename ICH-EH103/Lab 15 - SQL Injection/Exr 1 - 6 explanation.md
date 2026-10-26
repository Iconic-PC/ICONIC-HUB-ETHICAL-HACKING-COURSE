# SQL Injection Lab: Exercises 1–6 Explained

## Introduction

SQL Injection (SQLi) is a web application security vulnerability that occurs when an application handles user input unsafely and allows that input to interfere with a SQL query sent to a database.

SQL stands for **Structured Query Language**. It is used to create, retrieve, update, and delete information stored in relational databases.

Web applications commonly use databases to store information such as:

- Usernames and passwords
- Customer information
- Product details
- Payment records
- Student information
- Employee records
- Application settings

When an application communicates with a database, it often uses SQL queries to request specific information. If the application builds these queries unsafely, an attacker may be able to manipulate the intended query.

Depending on the vulnerability, database permissions, and application design, SQL injection can potentially allow unauthorized access to information, authentication bypass, or modification of database records.

This laboratory exercise uses **OWASP Mutillidae II**, a deliberately vulnerable web application designed for security education and testing.

> **Lab safety:** Perform these exercises only against your authorized Mutillidae environment. Do not test these techniques against websites, databases, or accounts without explicit permission.

---

# Understanding a Normal SQL Query

Before examining the exercises, it is important to understand how a normal login query works.

Suppose a user enters the following details:

- Username: `Abba`
- Password: `abba`

A simplified SQL query might look like this:

```sql
SELECT *
FROM accounts
WHERE username = 'Abba'
AND password = 'abba';
```

### Breaking down the query

| SQL component | Explanation |
|---|---|
| `SELECT` | Requests information from the database. |
| `*` | Requests all columns from the matching records. |
| `FROM accounts` | Specifies the database table being queried. |
| `WHERE` | Introduces the conditions records must satisfy. |
| `username = 'Abba'` | Checks whether the username matches Abba. |
| `AND` | Requires both conditions to be true. |
| `password = 'abba'` | Checks whether the password matches. |

The application processes the database response and decides whether to allow the user to continue.

A secure application should use parameterized queries, which keep user input separate from the SQL instructions. The example above illustrates the logic of a query; it does not recommend storing passwords as plain text.

## A Simple Real-Life Analogy

Imagine a security guard who allows someone into a building only when their name is on the approved visitor list and they have valid identification.

The guard should check the information supplied by the visitor without allowing the visitor to change the building's admission rules.

An SQL injection vulnerability is similar to allowing a visitor to alter the guard's instructions by writing unexpected commands on the visitor form.

---

# Exercise 1: Identifying Vulnerable Parameters

## Objective

Identify application inputs that may influence database queries and determine whether those inputs are handled safely.

## 1. What Is a Parameter?

A parameter is a value supplied to an application. Parameters may come from:

- Login forms
- Search boxes
- Registration forms
- URL query strings
- API requests
- Contact forms

For example, a login page might contain two parameters:

- Username
- Password

A search page might contain a parameter named `product`.

The presence of a parameter does not automatically mean that it is vulnerable. A tester must investigate how the application processes it.

## 2. Practical Explanation

In the Mutillidae lab, the User Lookup (SQL) page contains Name and Password fields.

The tester begins by entering ordinary values and observing the response. Next, the tester checks how the application handles unexpected input, such as an apostrophe.

An apostrophe is useful as an initial diagnostic character because SQL frequently uses single quotation marks to delimit text values.

If an application inserts user input directly into a SQL query without handling it safely, an unexpected apostrophe may disrupt the query's syntax.

### Lab observation

The following results were observed in the Mutillidae environment:

| Test | Observed result |
|---|---|
| Normal Name and Password values | Authentication error |
| Apostrophe in the Password field | MySQL syntax error |
| Apostrophe in the Name field | MySQL syntax error |
| Error response | Displayed SQL query and database error details |

The application displayed a query similar to:

```sql
SELECT * FROM accounts
WHERE username='Abba' AND password='''
```

The quotation marks are not balanced correctly, causing MySQL to report a syntax error.

### What does this prove?

The result is evidence that the application may be constructing SQL queries unsafely. The disclosure of the query and internal error details is also a security concern.

However, the syntax error alone does not prove that authentication can be bypassed or that database records can be extracted.

## 3. Why Is This Important?

Identifying a potentially vulnerable parameter helps the tester determine where further investigation is necessary.

The objective is to establish a clear relationship between the input and the application's database behavior, rather than assuming every unusual response represents a successful attack.

## 4. Real-Life Examples

### Example 1: Online Banking Login

Imagine an online banking application with username and password fields.

If the application handles these fields unsafely, a malicious user might be able to interfere with the database query used to authenticate customers.

Depending on the vulnerability, this could potentially lead to unauthorized access to customer accounts.

A security tester would investigate whether the inputs are safely parameterized, whether database errors are exposed, and whether authentication behaves correctly.

### Example 2: University Student Portal

Imagine a university portal that allows students to log in using their matriculation numbers and passwords.

If the application constructs database queries unsafely, an attacker may attempt to manipulate the login process or access records they are not authorized to view.

The potential impact could include exposure of student profiles, examination results, or registration information.

A tester would examine the login parameters and verify that the application separates user input from SQL instructions.

## Exercise 1: Key Lesson

**A parameter is a potential entry point, not proof of a vulnerability.** Test its behavior, document the evidence, and confirm the underlying security issue before reporting a successful exploit.

---

# Exercise 2: Understanding Basic SQL Injection

## Objective

Understand how unsafe user input can change the logical structure of a SQL query.

## 1. What Is Basic SQL Injection?

Basic SQL injection occurs when unexpected input changes the meaning of a SQL statement.

Consider this simplified query:

```sql
SELECT *
FROM accounts
WHERE username = 'Abba'
AND password = 'abba';
```

The database checks whether a record satisfies both conditions.

The word `AND` is important because both conditions must be true for a record to match.

An injection vulnerability may allow an attacker to interfere with those conditions.

## 2. Understanding the Boolean Logic

A Boolean expression is an expression that evaluates to either TRUE or FALSE.

For example:

```sql
1 = 1
```

This expression is TRUE because one equals one.

```sql
1 = 2
```

This expression is FALSE because one does not equal two.

Attackers may investigate whether an application incorrectly allows supplied input to influence the conditions of a query.

The key issue is not the special character itself. The key issue is whether the application allows user-controlled input to become part of the SQL instructions.

## 3. Understanding the UNION Keyword

SQL provides a keyword called `UNION`. It combines the results of two compatible `SELECT` statements.

For example, consider two queries that return the same type of information:

```sql
SELECT name FROM customers
UNION
SELECT name FROM suppliers;
```

This query combines the names returned by the two queries.

For a UNION-based SQL injection vulnerability to work, several requirements must be satisfied:

1. The application must be vulnerable to SQL injection.
2. The database must accept the resulting query.
3. Both queries must return the same number of columns.
4. Corresponding columns must have compatible data types.
5. The application must expose or otherwise use the combined results.

Therefore, a UNION statement does not automatically produce a successful injection.

## 4. What Should Students Observe?

During authorized testing, students should compare the application's behavior with normal input and controlled test input.

They should record:

- Whether the response changes
- Whether an SQL error appears
- Whether the number of displayed records changes
- Whether the application returns unexpected information
- Whether the result is repeatable

A changed response is evidence to investigate, not automatic proof of a successful attack.

## 5. Real-Life Examples

### Example 1: E-Commerce Product Search

Imagine an online store that lets customers search for products by name.

A secure application treats the search term as data and searches for matching products.

An application vulnerable to SQL injection might allow a specially crafted search term to alter the query. Depending on the database permissions and the query structure, an attacker could potentially access information beyond the intended product results.

The business impact could include exposure of customer information or confidential product records.

### Example 2: Employee Management System

Imagine an organization with an employee portal that retrieves employee details from a database.

If an employee search field is vulnerable, a malicious user might attempt to alter the query so that it returns records beyond the employee they are authorized to view.

This could expose names, contact details, salary information, or other confidential records if the database and application permit access.

## Exercise 2: Key Lesson

**SQL injection manipulates query logic, not merely the appearance of an input field.** Always distinguish a suspicious response from verified unauthorized access.

---

# Exercise 3: Understanding Error-Based SQL Injection

## Objective

Understand how database error messages can reveal information about an application's database processing.

## 1. What Is Error-Based SQL Injection?

Error-based SQL injection is a technique in which a tester investigates whether database errors can reveal useful information about the underlying application or database.

An application may display errors when a query contains invalid syntax, incompatible data types, or references to objects that do not exist.

If detailed errors are exposed to users, they may reveal information that should remain internal.

For example, an error might reveal:

- The database product
- SQL syntax
- Table or column names
- Query structure
- Internal file paths
- Database error codes

This information can help an attacker understand the application's implementation.

## 2. What Happened in Mutillidae?

In the lab, entering an apostrophe into a Name or Password field caused MySQL error 1064.

Error 1064 is associated with SQL syntax errors.

The application also displayed the query that had been generated.

This is significant because ordinary users generally should not receive detailed database diagnostics.

The observed error demonstrates that the application exposes internal database details and that the supplied character affects the query's syntax.

It does not, by itself, demonstrate that the database version, confidential records, or credentials have been successfully extracted.

## 3. Important Database Difference

The original exercise includes this expression:

```sql
CONVERT(int, (SELECT @@version))
```

This syntax is associated with Microsoft SQL Server. It is not an appropriate version-discovery expression for the MySQL-backed Mutillidae page used in this lab.

MySQL and Microsoft SQL Server use different functions and syntax in some areas. A test must be appropriate for the database being examined.

This is an important lesson in penetration testing: **never assume that a payload written for one database will work on another.**

## 4. Why Are Detailed Errors Dangerous?

Detailed errors can help developers diagnose problems during development, but exposing them in production may assist an attacker.

For example, an error that reveals a table name may help someone understand how the application stores information. A file path may reveal part of the application's server-side structure.

The recommended approach is to log technical details securely on the server and display a generic error to the user.

## 5. Real-Life Examples

### Example 1: Online Payment Application

Imagine an online payment application that displays raw database errors whenever a customer submits a transaction form.

An error might reveal the name of a payment table or details about the SQL statement used to retrieve transaction records.

An attacker could use that information to better understand the application and investigate other weaknesses.

The immediate issue is information disclosure. Whether it leads to a more serious compromise depends on the other security controls in place.

### Example 2: Hospital Management System

Imagine a hospital system that displays database error messages when staff search for patient records.

If the error reveals internal database names or server paths, an unauthorized user may learn details about the system's implementation.

That information could make further attacks easier and increase the risk to confidential patient information.

A secure system should show a simple message such as “The request could not be processed” while recording the technical error in protected server logs.

## Exercise 3: Key Lesson

**Database errors are useful evidence for a security tester, but they should not expose internal implementation details to ordinary users.**

---

# Exercise 4: Understanding Boolean-Based SQL Injection

## Objective

Understand how TRUE and FALSE conditions can be used to investigate whether user input influences database query behavior.

## 1. What Is Boolean-Based SQL Injection?

Boolean-based SQL injection is a technique that examines how an application responds when a database condition evaluates to TRUE compared with when it evaluates to FALSE.

The tester observes whether the application's response changes in a consistent way.

Consider these expressions:

```sql
1 = 1
```

This evaluates to TRUE.

```sql
1 = 2
```

This evaluates to FALSE.

If an application is vulnerable, changing the logic of the query may affect the records returned or the application's response.

The response difference can be visible or subtle. For example, the page might display different content, a different number of results, or a generic message.

## 2. How Does the Test Work Conceptually?

A tester establishes a baseline response using ordinary input.

Next, the tester compares controlled conditions expected to evaluate to TRUE and FALSE, using a method appropriate for the authorized lab and the database in use.

The results must be compared carefully because legitimate application behavior can also cause response differences.

For example, a session timeout, invalid account, or unrelated server error could affect the response.

## 3. How Should Results Be Interpreted?

| Observation | Possible interpretation |
|---|---|
| TRUE and FALSE tests produce consistently different responses | The input may influence query logic. Further validation is needed. |
| Both tests produce the same response | The test may not affect the query, or the application may hide the difference. |
| A syntax error appears | The query may be malformed; this is not proof of successful extraction. |
| The page sometimes changes and sometimes does not | Results may be affected by other factors; repeat the test and investigate. |

A tester should not report a confirmed Boolean-based SQL injection vulnerability based on one unexplained page change.

## 4. Real-Life Examples

### Example 1: Library Book Search

Imagine a public library website that searches for books using a database.

A normal search returns matching books. If the application is vulnerable, manipulated query logic might cause it to return a different set of records than the user requested.

A tester can compare expected results with observed results to determine whether the application is safely processing search terms.

The potential impact depends on what data the query can access and what the application displays.

### Example 2: Online Booking System

Imagine a travel website that retrieves available hotel rooms based on destination and date.

If the destination parameter is vulnerable, changing the query's logical conditions could potentially affect which records the database returns.

This could result in unintended information being displayed or incorrect application behavior.

A security assessment should verify that the input influences SQL logic rather than assuming every unusual booking result is caused by injection.

## Exercise 4: Key Lesson

**Boolean-based testing relies on comparing controlled TRUE and FALSE conditions.** Consistent, explainable differences provide evidence, but the underlying cause must still be verified.

---

# Exercise 5: Understanding UNION-Based SQL Injection

## Objective

Understand how the SQL `UNION` operator combines query results and why unsafe applications may expose information from unintended database queries.

## 1. What Is UNION-Based SQL Injection?

The SQL `UNION` operator combines the results of two or more compatible `SELECT` statements.

Consider a database containing a products table and a suppliers table.

A legitimate query might be:

```sql
SELECT name FROM products
UNION
SELECT name FROM suppliers;
```

The database returns the combined names from both queries.

In a vulnerable application, a tester may investigate whether an injected UNION query can cause the application to return information beyond its intended results.

## 2. Important Requirements

A UNION query has specific requirements:

### Requirement 1: Matching Column Count

Both `SELECT` statements must return the same number of columns.

If the original query returns four columns, the second query must also return four columns.

### Requirement 2: Compatible Data Types

Corresponding columns must have compatible data types. For example, combining text and numeric values may require appropriate handling depending on the database.

### Requirement 3: A Vulnerable Query Context

The application must allow the supplied input to alter the SQL statement in a way that the database accepts.

### Requirement 4: Results Must Be Observable

Even if a query executes successfully, the application may not display the resulting data. The tester must determine what the response actually demonstrates.

## 3. Understanding Database Information Functions

MySQL provides functions that return information about the database environment.

For example:

```sql
SELECT VERSION();
```

This returns the MySQL server version.

```sql
SELECT USER();
```

This returns the current MySQL session's user identity.

```sql
SELECT DATABASE();
```

This returns the current default database name, if one is selected.

These functions are useful for understanding a database during authorized testing. They do not automatically reveal passwords or grant additional permissions.

## 4. Applying This to Mutillidae

The observed Mutillidae query uses:

```sql
SELECT * FROM accounts
```

The `*` means that the query requests all columns from the `accounts` table.

The number and data types of those columns determine which UNION queries would be compatible. The number of columns must be established from authorized lab evidence rather than assumed.

A sample payload with four columns may fail if the original query returns a different number of columns.

A failure does not automatically mean that the application is secure. It may simply mean that the attempted query is incompatible with the original statement.

Students should document the actual response and avoid claiming successful data retrieval unless the evidence supports that conclusion.

## 5. Real-Life Examples

### Example 1: Online Store Administration Portal

Imagine an online store that displays product records from a database.

If a vulnerable product-search query can be manipulated using UNION, it may be possible for an attacker to cause the application to combine product information with records from another accessible table.

Depending on the database permissions and the application's query structure, this could expose internal business information.

The risk is greater if the application uses an overprivileged database account.

### Example 2: Employee Records Application

Imagine a company application that retrieves employee names and department details.

If its query is vulnerable and the database account can access additional tables, an attacker may attempt to combine the intended employee results with other records.

This could potentially expose confidential organizational information.

Proper parameterization and least-privilege database permissions help reduce this risk.

## Exercise 5: Key Lesson

**UNION combines compatible query results.** Successful exploitation depends on the query structure, column count, data types, database permissions, and what the application reveals.

---

# Exercise 6: Understanding Database Metadata Extraction

## Objective

Understand database metadata and how improper query handling could expose information about the structure of a database.

## 1. What Is Database Metadata?

Metadata is information that describes how a database is organized.

It can include:

- Table names
- Column names
- Data types
- Relationships between tables
- Database object definitions

For example, a database might contain tables named:

- `accounts`
- `products`
- `orders`
- `employees`

The `accounts` table might contain columns such as:

- `username`
- `password`
- `email`

These names describe the structure of the database.

Metadata is different from the actual records stored in those tables. Knowing that a table exists does not automatically mean a user can read its contents.

## 2. What Is `information_schema`?

MySQL provides a special database called `information_schema`. It contains metadata about databases, tables, columns, and other database objects that the current database user is permitted to see.

For example, this query retrieves table names visible through the metadata interface:

```sql
SELECT TABLE_NAME
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = DATABASE();
```

### Breaking down the query

| Component | Explanation |
|---|---|
| `information_schema.TABLES` | References metadata describing tables. |
| `TABLE_NAME` | Selects the table-name field. |
| `TABLE_SCHEMA = DATABASE()` | Filters metadata to the current default database. |
| `SELECT` | Requests the matching metadata values. |

This is an ordinary metadata query. It is not, by itself, an SQL injection attack.

The security concern arises when an application vulnerability allows an unauthorized user to manipulate a query and retrieve metadata they should not be able to obtain through the application.

## 3. Why Would an Attacker Want Table and Column Names?

Understanding a database's structure may help an attacker identify where sensitive information is stored.

For example, table names may suggest that the database contains customer records, transactions, employee accounts, or administrative settings.

Column names may reveal the purpose of individual fields.

However, table and column names alone do not prove that sensitive records are accessible. Database permissions and application controls still matter.

## 4. Applying This to Mutillidae

The observed application query references the `accounts` table:

```sql
SELECT * FROM accounts
```

Therefore, students should not assume that a table named `users` exists.

They should use the evidence available in the lab to identify the actual schema and interpret results carefully.

The original exercise includes a query intended to inspect metadata through an injected UNION statement. Whether that technique works depends on the original query's column count, data types, injection context, and database permissions.

If the application returns a syntax error, that is evidence of a failed query—not proof that metadata has been extracted.

## 5. Real-Life Examples

### Example 1: Hospital Database

Imagine a hospital database containing tables for patients, appointments, prescriptions, and staff accounts.

If an application vulnerability allows an unauthorized user to enumerate metadata, the user may learn how the database is organized.

That information could help them identify potentially sensitive areas of the application for further investigation.

The hospital should restrict database permissions, use parameterized queries, and avoid exposing unnecessary database details.

### Example 2: Financial Services Company

Imagine a financial services company with tables for customers, transfers, invoices, and account activity.

If a vulnerable application exposes database metadata, an attacker may discover names that reveal the system's data organization.

This discovery does not automatically provide access to customer transactions, but it can reduce uncertainty about the database structure and support further attacks.

The organization should ensure that the database account used by each application has only the permissions required for its tasks.

## Exercise 6: Key Lesson

**Metadata describes the structure of a database.** Exposing it can help an attacker understand the application, but metadata discovery and access to actual records are separate security findings.

---

# Comparing the Six Exercises

| Exercise | Main concept | What the tester investigates |
|---|---|---|
| 1. Vulnerable Parameters | Identifying potential entry points | Whether inputs influence SQL processing unsafely |
| 2. Basic SQL Injection | Query logic manipulation | Whether user input can alter the intended SQL statement |
| 3. Error-Based SQL Injection | Database error disclosure | Whether errors reveal query structure or internal details |
| 4. Boolean-Based SQL Injection | TRUE/FALSE conditions | Whether controlled conditions produce consistent response differences |
| 5. UNION-Based SQL Injection | Combining query results | Whether unintended query results can be exposed |
| 6. Metadata Extraction | Database structure discovery | Whether database metadata can be accessed through an application vulnerability |

# Important Findings From the Current Mutillidae Lab

Based on the tests performed so far, the evidence supports the following conclusions:

1. Normal login input produced an authentication error.
2. An apostrophe in the Name field produced a MySQL syntax error.
3. An apostrophe in the Password field also produced a MySQL syntax error.
4. The application exposed the generated SQL query and database error details.
5. The observed query references the `accounts` table.

These findings indicate unsafe SQL query handling and excessive error disclosure.

However, the tests described here do **not** yet establish that authentication was bypassed, UNION-based extraction succeeded, or database metadata was successfully enumerated.

A professional security report should distinguish confirmed findings from unverified possibilities.

# How to Prevent SQL Injection

Developers should apply several defensive measures.

## 1. Use Parameterized Queries

Parameterized queries keep SQL instructions separate from user-supplied values.

For example, in PHP using PDO:

```php
$stmt = $pdo->prepare(
    'SELECT * FROM accounts WHERE username = :username'
);

$stmt->execute([
    'username' => $username
]);
```

Here, the username is supplied as a parameter rather than inserted directly into the SQL instruction.

This is safer than constructing a query by concatenating user input into a SQL string.

For login systems, passwords should also be handled using appropriate password hashing and verification mechanisms rather than plain-text database comparisons.

## 2. Apply Least-Privilege Database Permissions

The application's database account should have only the permissions it needs.

For example, an application that only needs to retrieve product listings should not automatically have permission to modify employee records or administer the entire database.

Reducing privileges limits the potential impact if an application vulnerability is exploited.

## 3. Handle Errors Securely

Users should receive generic error messages rather than raw database diagnostics.

Detailed errors should be recorded in protected server-side logs for authorized developers and administrators.

## 4. Validate Input Appropriately

Applications should validate that input matches the expected format and length.

For example, a field intended to accept a numeric student ID should reject unexpected formats.

Input validation is useful, but it should complement parameterized queries rather than replace them.

## 5. Test and Review Applications Regularly

Organizations should conduct authorized security testing, review database access controls, update application components, and investigate unexpected database errors.

Security testing should verify both the existence of vulnerabilities and the effectiveness of remediation.

---

# Conclusion

SQL injection is a serious web application security risk because it can allow user-controlled input to interfere with database queries.

The six exercises introduce important concepts: identifying potentially vulnerable parameters, understanding query logic, interpreting database errors, comparing Boolean conditions, understanding UNION queries, and examining database metadata.

The Mutillidae lab demonstrates why database errors and unsafe query construction deserve investigation. The next step in a professional assessment is to collect repeatable evidence, determine the actual impact, and recommend appropriate remediation.

**The goal of ethical hacking is not simply to make an attack work. It is to understand the weakness, demonstrate its impact responsibly, document the evidence accurately, and help prevent it from happening again.**
