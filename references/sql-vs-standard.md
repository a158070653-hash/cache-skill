# Cache SQL vs Standard SQL Differences

## Key Differences

### 1. TOP vs LIMIT

**Standard SQL:**
```sql
SELECT * FROM users LIMIT 10
```

**Cache SQL:**
```sql
SELECT TOP 10 * FROM users
```

### 2. Case Sensitivity

**Standard SQL:**
```sql
-- Usually case-insensitive for identifiers
SELECT name FROM Users WHERE name = 'john'
```

**Cache SQL:**
```sql
-- Case-insensitive by default, but use %EXACT for case-sensitive
SELECT * FROM Users WHERE %EXACT(Name) = 'John'

-- Collation functions
SELECT * FROM Users WHERE %SQLUPPER(Name) = 'JOHN'  -- Force uppercase comparison
```

### 3. String Matching

**Standard SQL:**
```sql
SELECT * FROM users WHERE name LIKE 'John%'
SELECT * FROM users WHERE name LIKE '%son'
```

**Cache SQL:**
```sql
-- %STARTSWITH is Cache-specific (more efficient than LIKE)
SELECT * FROM users WHERE Name %STARTSWITH 'John'

-- %CONTAINS for text search
SELECT * FROM users WHERE Name %CONTAINS 'son'

-- %MATCHES for pattern matching (Cache-specific)
SELECT * FROM users WHERE Name %MATCHES 'J*son'
```

### 4. List/Array Operations

**Standard SQL:**
```sql
-- Usually no native list support
```

**Cache SQL:**
```sql
-- %INLIST for %List fields
SELECT * FROM users WHERE 'John' %INLIST NameList

-- FOR SOME %ELEMENT for list conditions
SELECT * FROM users WHERE FOR SOME %ELEMENT(NameList) (%Value = 'John')
```

### 5. Date/Time Functions

**Standard SQL:**
```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 DAY)
SELECT DATEDIFF(end_date, start_date)
```

**Cache SQL:**
```sql
-- DATEADD
SELECT DATEADD('day', 1, CURRENT_TIMESTAMP)

-- DATEDIFF
SELECT DATEDIFF('day', start_date, end_date)

-- Cache-specific date functions
SELECT TO_DATE('2024-03-24', 'YYYY-MM-DD')
SELECT TO_CHAR(date_column, 'YYYY-MM-DD')
```

### 6. NULL Handling

**Standard SQL:**
```sql
SELECT COALESCE(name, 'Unknown') FROM users
SELECT IFNULL(name, 'Unknown') FROM users  -- MySQL
```

**Cache SQL:**
```sql
-- Both COALESCE and IFNULL are supported
SELECT COALESCE(Name, 'Unknown') FROM users
SELECT IFNULL(Name, 'Unknown') FROM users
SELECT NVL(Name, 'Unknown') FROM users  -- Also supported
```

### 7. Identity/Auto-increment

**Standard SQL:**
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
```

**Cache SQL:**
```sql
CREATE TABLE users (
    ID INTEGER IDENTITY,
    Name VARCHAR(100),
    CONSTRAINT PK_users PRIMARY KEY (ID)
)
```

### 8. Upsert (INSERT OR UPDATE)

**Standard SQL:**
```sql
INSERT INTO users (id, name) VALUES (1, 'John')
ON DUPLICATE KEY UPDATE name = 'John';  -- MySQL

MERGE INTO users USING ... ON ... WHEN MATCHED ...;  -- SQL Server
```

**Cache SQL:**
```sql
-- Cache has native upsert
INSERT OR UPDATE INTO users (ID, Name) VALUES (1, 'John')
```

### 9. Schema Qualification

**Standard SQL:**
```sql
SELECT * FROM database.schema.table
SELECT * FROM schema.table
```

**Cache SQL:**
```sql
-- Cache uses package names (class names)
SELECT * FROM MyApp.Person
SELECT * FROM SQLUser.Person
```

### 10. Stored Procedures

**Standard SQL:**
```sql
CREATE PROCEDURE get_user(IN user_id INT)
BEGIN
    SELECT * FROM users WHERE id = user_id;
END
```

**Cache SQL:**
```sql
-- Stored procedures are class methods
CREATE PROCEDURE GetUser(user_id INTEGER)
    RETURNS TABLE (Name VARCHAR(100), Age INTEGER)
    LANGUAGE OBJECTSCRIPT
{
    // ObjectScript implementation
}
```

## Common Pitfalls

### 1. String Concatenation
```sql
-- Standard SQL
SELECT first_name || ' ' || last_name FROM users

-- Cache SQL (|| is supported, but also:)
SELECT first_name _ ' ' _ last_name FROM users
```

### 2. Boolean Values
```sql
-- Standard SQL
WHERE is_active = TRUE

-- Cache SQL
WHERE IsActive = 1
```

### 3. Date Literals
```sql
-- Standard SQL
WHERE date = '2024-03-24'

-- Cache SQL (date literals work, but for internal format)
WHERE date = {d '2024-03-24'}
WHERE timestamp = {ts '2024-03-24 12:00:00'}
```

### 4. Outer Join Syntax
```sql
-- Standard SQL (both work in Cache)
SELECT * FROM orders o LEFT JOIN customers c ON o.customer_id = c.id

-- Old style (also supported)
SELECT * FROM orders o, customers c WHERE o.customer_id =* c.id
```

## Performance Tips

1. **Use %STARTSWITH** instead of `LIKE 'prefix%'` - it's optimized
2. **Use %EXACT** when you need case-sensitive comparison
3. **Avoid SELECT *** in production - specify columns
4. **Use TOP** to limit result sets
5. **Cache SQL Optimizer** automatically optimizes queries, but you can use `%NOINDEX` to force table scan when needed

## Reference

For detailed information, see:
- `D:/cache-docs/sql/commands/` - SQL command reference
- `D:/cache-docs/sql/functions/` - SQL function reference
- `D:/cache-docs/sql/predicates/` - SQL predicate reference
