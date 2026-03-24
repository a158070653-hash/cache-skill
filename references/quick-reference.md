# Cache Quick Reference

## ObjectScript Basics

### Variables
```objectscript
; Local variables (case-sensitive)
SET name = "John"
SET age = 25

; Global variables (persistent, start with ^)
SET ^Person(1, "name") = "张三"
SET ^Person(1, "age") = 30

; Safe access with default
SET name = $GET(^Person(1, "name"), "Unknown")

; Check if variable exists
IF $DATA(^Person(1)) {
    ; Variable exists
}
```

### String Operations
```objectscript
; $PIECE - Extract by delimiter
SET str = "apple,banana,cherry"
SET first = $PIECE(str, ",", 1)     ; "apple"
SET second = $PIECE(str, ",", 2)    ; "banana"
SET last = $PIECE(str, ",", 3, 99)  ; "cherry" (from 3 to end)

; $EXTRACT - Extract by position
SET sub = $EXTRACT(str, 1, 5)       ; "apple"
SET char = $EXTRACT(str, 1)         ; "a"

; $LENGTH - String length
SET len = $LENGTH(str)              ; 17

; $TRANSLATE - Character replacement
SET result = $TRANSLATE(str, "a", "A") ; "Apple,bonono,Cherry"

; $REPLACE - String replacement
SET result = $REPLACE(str, "apple", "orange") ; "orange,banana,cherry"
```

### List Operations
```objectscript
; Build a list
SET list = $LISTBUILD("apple", "banana", "cherry")

; Get element
SET item = $LIST(list, 2)           ; "banana"

; List length
SET count = $LISTLENGTH(list)       ; 3

; Find element
SET pos = $LISTFIND(list, "banana") ; 2

; Build list from string
SET list = $LISTFROMSTRING("a,b,c", ",")

; Convert list to string
SET str = $LISTTOSTRING(list, ",")  ; "a,b,c"
```

### Control Flow
```objectscript
; IF/ELSE
IF age > 18 {
    WRITE "Adult"
} ELSEIF age > 12 {
    WRITE "Teen"
} ELSE {
    WRITE "Child"
}

; FOR loop
FOR i=1:1:10 {
    WRITE i, !
}

; FOR with $ORDER (iterate globals)
SET id = ""
FOR {
    SET id = $ORDER(^Person(id))
    QUIT:id=""
    SET name = ^Person(id, "name")
    WRITE name, !
}

; WHILE
SET count = 0
WHILE count < 5 {
    SET count = count + 1
    WRITE count, !
}
```

### Error Handling
```objectscript
TRY {
    ; Code that might fail
    SET obj = ##class(MyApp.Person).%New()
    SET obj.Name = "Test"
    DO obj.%Save()
} CATCH ex {
    ; Handle error
    WRITE "Error: ", ex.DisplayString(), !
    ; Or: WRITE "Error: ", $ZERROR, !
}
```

### Class Usage
```objectscript
; Create object
SET person = ##class(MyApp.Person).%New()
SET person.Name = "张三"
SET person.Age = 30
DO person.%Save()

; Open object by ID
SET person = ##class(MyApp.Person).%OpenId(1)
IF $ISOBJECT(person) {
    WRITE person.Name
}

; Call class method
SET result = ##class(MyApp.Person).FindByName("张三")

; Delete object
DO ##class(MyApp.Person).%DeleteId(1)
```

## SQL Basics

### Query Data
```sql
-- Basic SELECT
SELECT * FROM MyApp.Person

-- With WHERE
SELECT Name, Age FROM MyApp.Person WHERE Age > 18

-- TOP (Cache uses TOP, not LIMIT)
SELECT TOP 10 * FROM MyApp.Person ORDER BY Name

-- Cache-specific predicates
SELECT * FROM MyApp.Person WHERE Name %STARTSWITH '张'
SELECT * FROM MyApp.Person WHERE Name %CONTAINS 'san'
SELECT * FROM MyApp.Person WHERE %EXACT(Name) = 'ZhangSan'
```

### Modify Data
```sql
-- INSERT
INSERT INTO MyApp.Person (Name, Age) VALUES ('张三', 30)

-- UPDATE
UPDATE MyApp.Person SET Age = 31 WHERE Name = '张三'

-- DELETE
DELETE FROM MyApp.Person WHERE Age < 18

-- INSERT OR UPDATE (upsert)
INSERT OR UPDATE INTO MyApp.Person (ID, Name, Age) VALUES (1, '张三', 31)
```

### Create Table
```sql
CREATE TABLE MyApp.Person (
    ID INTEGER IDENTITY,
    Name VARCHAR(100),
    Age INTEGER,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT PK_Person PRIMARY KEY (ID)
)
```

## Common Patterns

### Date/Time
```objectscript
; Current date/time
SET now = $HOROLOG                    ; Internal format: "66543,45678"
SET datePart = $PIECE(now, ",", 1)    ; Date portion
SET timePart = $PIECE(now, ",", 2)    ; Time portion

; Format date
SET formatted = $ZDATE(now, 3)        ; "2024-03-24" (YYYY-MM-DD)
SET formatted = $ZDATE(now, 1)        ; "03/24/2024" (MM/DD/YYYY)

; Format datetime
SET formatted = $ZDATETIME(now, 3)    ; "2024-03-24 12:34:56"

; Parse date
SET internal = $ZDATEH("2024-03-24", 3)
```

### Transactions
```objectscript
TSTART
TRY {
    ; Multiple operations
    SET ^Account(1, "balance") = ^Account(1, "balance") - 100
    SET ^Account(2, "balance") = ^Account(2, "balance") + 100
    TCOMMIT
} CATCH ex {
    TROLLBACK
    ; Handle error
}
```

### SQL in ObjectScript
```objectscript
; Embedded SQL
&sql(SELECT Name INTO :name FROM MyApp.Person WHERE ID = :id)
IF SQLCODE = 0 {
    WRITE "Name: ", name
} ELSE {
    WRITE "Error: ", SQLCODE
}

; Dynamic SQL
SET rs = ##class(%SQL.Statement).%ExecDirect(,
    "SELECT * FROM MyApp.Person WHERE Age > ?", age)
WHILE rs.%Next() {
    WRITE rs.%Get("Name"), !
}
```
