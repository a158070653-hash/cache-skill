# Cache Common Patterns

## Data Access Patterns

### Global Variable Access
```objectscript
; Basic global access
SET ^Person(id, "name") = "张三"
SET name = ^Person(id, "name")

; Safe access with default
SET name = $GET(^Person(id, "name"), "Unknown")

; Check existence
IF $DATA(^Person(id)) {
    ; Record exists
}

; Delete single node
KILL ^Person(id, "name")

; Delete entire subtree
KILL ^Person(id)
```

### Iterate Global Subscripts
```objectscript
; Forward iteration
SET id = ""
FOR {
    SET id = $ORDER(^Person(id))
    QUIT:id=""
    ; Process record
    WRITE ^Person(id, "name"), !
}

; Reverse iteration
SET id = ""
FOR {
    SET id = $ORDER(^Person(id), -1)  ; -1 for reverse
    QUIT:id=""
    ; Process record
}

; With starting point
SET id = 100
FOR {
    SET id = $ORDER(^Person(id))
    QUIT:id=""
    ; Process from id=100 onwards
}
```

### Count Records
```objectscript
; Count by iterating
SET count = 0
SET id = ""
FOR {
    SET id = $ORDER(^Person(id))
    QUIT:id=""
    SET count = count + 1
}
WRITE "Total: ", count

; Or use SQL
&sql(SELECT COUNT(*) INTO :count FROM MyApp.Person)
```

## Class Definition Patterns

### Persistent Class
```objectscript
Class MyApp.Person Extends %Persistent [ ClassType = persistent ]
{
    /// Person name
    Property Name As %String(MAXLEN = 100);
    
    /// Person age
    Property Age As %Integer;
    
    /// Creation timestamp
    Property CreatedAt As %TimeStamp [ InitialExpression = {$ZDATETIME($HOROLOG, 3)} ];
    
    /// Index on name
    Index NameIndex On Name;
    
    /// Class method to create person
    ClassMethod Create(name As %String, age As %Integer) As Person
    {
        SET obj = ..%New()
        SET obj.Name = name
        SET obj.Age = age
        SET status = obj.%Save()
        IF $$$ISERR(status) {
            THROW ##class(%Exception.StatusException).CreateFromStatus(status)
        }
        RETURN obj
    }
    
    /// Instance method to format display
    Method ToString() As %String
    {
        RETURN ..Name _ " (Age: " _ ..Age _ ")"
    }
}
```

### Serial Class (Embedded Object)
```objectscript
Class MyApp.Address Extends %SerialObject
{
    Property Street As %String(MAXLEN = 200);
    Property City As %String(MAXLEN = 100);
    Property ZipCode As %String(MAXLEN = 20);
}
```

### Relationship
```objectscript
Class MyApp.Order Extends %Persistent
{
    Property Customer As MyApp.Customer;
    Property Items As list Of MyApp.OrderItem;
}

Class MyApp.Customer Extends %Persistent
{
    Property Name As %String;
    Relationship Orders As MyApp.Order [ Cardinality = many, Inverse = Customer ];
}
```

## CRUD Operations

### Create
```objectscript
; Using %New()
SET person = ##class(MyApp.Person).%New()
SET person.Name = "张三"
SET person.Age = 30
SET status = person.%Save()
IF $$$ISERR(status) {
    ; Handle error
}

; Using class method
SET person = ##class(MyApp.Person).Create("李四", 25)
```

### Read
```objectscript
; Open by ID
SET person = ##class(MyApp.Person).%OpenId(1)
IF $ISOBJECT(person) {
    WRITE person.Name
}

; Query by SQL
&sql(SELECT Name, Age INTO :name, :age FROM MyApp.Person WHERE ID = :id)
IF SQLCODE = 0 {
    WRITE name, " is ", age
}

; Using dynamic SQL
SET rs = ##class(%SQL.Statement).%ExecDirect(,
    "SELECT * FROM MyApp.Person WHERE Age > ?", 18)
WHILE rs.%Next() {
    WRITE rs.%Get("Name"), !
}
```

### Update
```objectscript
; Open and modify
SET person = ##class(MyApp.Person).%OpenId(1)
IF $ISOBJECT(person) {
    SET person.Age = 31
    SET status = person.%Save()
}

; Using SQL
&sql(UPDATE MyApp.Person SET Age = 31 WHERE ID = 1)
```

### Delete
```objectscript
; Delete by ID
SET status = ##class(MyApp.Person).%DeleteId(1)

; Delete object
SET person = ##class(MyApp.Person).%OpenId(1)
IF $ISOBJECT(person) {
    SET status = person.%Delete(1)
}

; Using SQL
&sql(DELETE FROM MyApp.Person WHERE ID = 1)
```

## Transaction Pattern
```objectscript
TSTART
TRY {
    ; Transfer money between accounts
    SET ^Account(fromId, "balance") = ^Account(fromId, "balance") - amount
    SET ^Account(toId, "balance") = ^Account(toId, "balance") + amount
    
    ; Check for negative balance
    IF ^Account(fromId, "balance") < 0 {
        THROW ##class(%Exception.General).%New("Insufficient funds")
    }
    
    TCOMMIT
} CATCH ex {
    TROLLBACK
    WRITE "Transaction failed: ", ex.DisplayString(), !
}
```

## Error Handling Patterns

### Basic TRY/CATCH
```objectscript
TRY {
    ; Code that might fail
    SET obj = ##class(MyApp.Person).%New()
    DO obj.%Save()
} CATCH ex {
    WRITE "Error: ", ex.DisplayString(), !
    WRITE "Code: ", ex.Code, !
    WRITE "Location: ", ex.Location, !
}
```

### Status Code Handling
```objectscript
; Many Cache methods return %Status
SET status = obj.%Save()
IF $$$ISERR(status) {
    ; Convert to human-readable
    DO $SYSTEM.Status.DecomposeStatus(status, .errors)
    SET errMsg = ""
    SET i = ""
    FOR {
        SET i = $ORDER(errors(i))
        QUIT:i=""
        SET errMsg = errMsg _ errors(i) _ "; "
    }
    WRITE "Save failed: ", errMsg
}
```

### Error Trap
```objectscript
; Set error trap
SET $ZTRAP = "ErrorHandler"

; Code that might generate errors
SET x = 1/0  ; Division by zero

QUIT

ErrorHandler
WRITE "Error occurred: ", $ZERROR, !
WRITE "Location: ", $ZPOS, !
QUIT
```

## SQL Patterns

### Dynamic SQL
```objectscript
; Build query dynamically
SET query = "SELECT * FROM MyApp.Person WHERE Age > ?"
SET rs = ##class(%SQL.Statement).%ExecDirect(, query, age)
WHILE rs.%Next() {
    WRITE rs.%Get("Name"), !
}
```

### Embedded SQL
```objectscript
; Embedded SQL with host variables
SET minAge = 18
&sql(SELECT Name INTO :name FROM MyApp.Person WHERE Age > :minAge)
IF SQLCODE = 0 {
    WRITE "Found: ", name
} ELSEIF SQLCODE = 100 {
    WRITE "No results"
} ELSE {
    WRITE "Error: ", SQLCODE
}
```

### Pagination
```sql
-- Using TOP and NOT IN
SELECT TOP 10 * FROM MyApp.Person
WHERE ID NOT IN (SELECT TOP 20 ID FROM MyApp.Person ORDER BY ID)
ORDER BY ID

-- Or using ROW_NUMBER (if supported)
SELECT * FROM (
    SELECT *, ROW_NUMBER() OVER (ORDER BY ID) as RowNum
    FROM MyApp.Person
) WHERE RowNum BETWEEN 21 AND 30
```

## Performance Patterns

### Index Usage
```objectscript
; Ensure index is used
; Good: indexed column
SELECT * FROM MyApp.Person WHERE Name %STARTSWITH '张'

; Bad: function on indexed column
SELECT * FROM MyApp.Person WHERE $EXTRACT(Name, 1) = '张'
```

### Batch Processing
```objectscript
; Process in batches
SET batchSize = 1000
SET id = ""
FOR {
    TSTART
    FOR i=1:1:batchSize {
        SET id = $ORDER(^Person(id))
        QUIT:id=""
        ; Process record
        SET ^Person(id, "processed") = 1
    }
    TCOMMIT
    QUIT:id=""
}
```

### Efficient Iteration
```objectscript
; Use $ORDER for efficient iteration
SET id = ""
FOR {
    SET id = $ORDER(^Person(id))
    QUIT:id=""
    ; Direct global access is faster than object access
    SET name = ^Person(id, "name")
    SET age = ^Person(id, "age")
    ; Process...
}
```

## Web/REST Patterns

### REST API Handler
```objectscript
Class MyApp.REST.Person Extends %CSP.REST
{
    XData UrlMap
    {
        <Routes>
            <Route Url="/person/:id" Method="GET" Call="GetPerson"/>
            <Route Url="/person" Method="POST" Call="CreatePerson"/>
        </Routes>
    }
    
    ClassMethod GetPerson(id As %String) As %Status
    {
        SET person = ##class(MyApp.Person).%OpenId(id)
        IF '$ISOBJECT(person) {
            RETURN ..ReportHttpStatusCode(..#HTTP404NOTFOUND)
        }
        WRITE person.%JSONExport()
        RETURN $$$OK
    }
    
    ClassMethod CreatePerson() As %Status
    {
        SET data = ##class(%DynamicObject).%FromJSON(%request.Content)
        SET person = ##class(MyApp.Person).Create(data.name, data.age)
        WRITE person.%JSONExport()
        RETURN $$$OK
    }
}
```

## See Also

- `quick-reference.md` - Common syntax at a glance
- `sql-vs-standard.md` - Cache SQL differences
- `D:/cache-docs/objectscript/` - ObjectScript reference
- `D:/cache-docs/sql/` - SQL reference
