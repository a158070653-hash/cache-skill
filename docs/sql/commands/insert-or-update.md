# INSERT OR UPDATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_insertorupdate

---

 

Caché SQL Reference

INSERT OR UPDATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate "INSERT OR UPDATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Adds a new row or updates an existing row in a table.

Synopsis

INSERT OR UPDATE \[%NOFPLAN\] \[restriction\] \[INTO\] table
          SET column1 = scalar-expression1 {,column2 = scalar-expression2} ...  |
          \[ (column1{,column2} ...) \] VALUES (scalar-expression1 {,scalar-expression2} ...)  |
          VALUES :array()  |
          \[ (column1{,column2} ...) \] query  |
          DEFAULT VALUES

Arguments

%NOFPLAN

Optional — The %NOFPLAN keyword specifies that Caché will ignore the frozen plan (if any) for this operation and generate a new query plan. The frozen plan is retained, but not used. For further details, refer to [Frozen Plans](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_frozenplans) in Caché SQL Optimization Guide.

restriction

Optional — One or more of the following keywords, separated by spaces: %NOLOCK, %NOCHECK, %NOINDEX, %NOTRIGGER.

table

The name of the [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) or [view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) on which to perform the insert operation. This argument may be a subquery. The INTO keyword is optional.

column

Optional — A column name or comma-separated list of [column names](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fields) that correspond in sequence to the supplied list of values. If omitted, the list of values is applied to all columns in column-number order.

scalar-expression

A scalar expression or comma-separated list of scalar expressions that supplies the data values for the corresponding column fields.

:array()

Embedded SQL only — A dynamic local array of values specified as a [host variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars). The lowest subscript level of the array must be unspecified. Thus :myupdates(), :myupdates(5,), and :myupdates(1,1,) are all valid specifications.

query

A query’s result set that supplies the data values for the corresponding column fields for one or more rows.

Description

The INSERT OR UPDATE statement is an extension of the [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) statement (which it closely resembles):

*   If the specified record does not exist, INSERT OR UPDATE performs an INSERT.
    
*   If the specified record already exists, INSERT OR UPDATE performs an UPDATE. It updates the record with the specified field values. An update occurs even when the specified data is identical to the existing data.
    

INSERT OR UPDATE determines of a record exists by matching UNIQUE KEY field values to the existing data values. If a UNIQUE KEY violation occurs, INSERT OR UPDATE performs an update operation. Note that a UNIQUE KEY field value may not be a value explicitly specified in INSERT OR UPDATE; it may be the result of a column default value or a computed value.

INSERT OR UPDATE of a single record always sets the %ROWCOUNT variable to 1, and the %ROWID variable for the row that has been either inserted or updated.

An INSERT OR UPDATE statement combined with a SELECT statement can insert and/or update multiple table rows. For further details, refer to “[INSERT Query Results](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert#RSQL_insertselect)” in the INSERT reference page.

INSERT OR UPDATE uses the same syntax, and generally has the same features and restrictions as the [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) statement. Special considerations for INSERT OR UPDATE are described here. Unless otherwise stated here, refer to [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) for details.

Privileges

INSERT OR UPDATE requires both INSERT and UPDATE privileges, as well as SELECT privilege. You must have these privileges either as table-level privileges or as column-level privileges.

IDKEY Fields

You can insert an IDKEY field value, but you cannot update an IDKEY field value. If the table has an IDKEY index and another unique key constraint, INSERT OR UPDATE matches these fields to determine whether to perform an insert or an update. If the other key constraint fails, this forces INSERT OR UPDATE to perform an update rather than an insert. However, if the specified IDKEY field values do not match the existing IDKEY field values, this update fails and generates an SQLCODE -107 error, because the update is attempting to modify the IDKEY fields.

For example, the table MyTest is defined with four fields: A, B, C, D, with IDKEY (A,B) and UNIQUE (C,D) constraints. The table contains the following records:

Row 1: A=1, B=1, C=2, D=2
Row 2: A=1, B=2, C=3, D=4

You invoke INSERT OR UPDATE ABC (A,B,C,D) VALUES (2,2,3,4) Because the UNIQUE (C,D) constraint failed, this statement cannot perform an insert. Instead, it attempts to update Row 2. The IDKEY for Row 2 is (1,2), so the INSERT OR UPDATE statement would attempt to change the field A value from 1 to 2. But you cannot change an IDKEY value, so the update fails with an SQLCODE -107 error.

%Counter Type Fields

When an INSERT OR UPDATE is executed, Caché initially assumes the operation will be an insert. Therefore, it increments the CounterLocation each time it is invoked. An insert uses the CounterLocation value to assign the %Counter field value. If, however, Caché determines that the operation needs to be an update, INSERT OR UPDATE has already incremented the CounterLocation value, but does not assign it to a %Counter field. This is shown in the following example:

1.  The initial CounterLocation is 0. INSERT OR UPDATE increments CounterLocation then inserts Row 1: CounterLocation=1, Row 1 %Counter field=1.
    
2.  INSERT OR UPDATE increments CounterLocation then determines that it must performs an update on Row 1: CounterLocation=2, Row 1 %Counter field remains 1.
    
3.  INSERT OR UPDATE increments CounterLocation then inserts Row 2: CounterLocation=3, Row 2 %Counter field=3.
    

One example of a %Counter type field is an [IDENTITY field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield).

Examples

The following five examples create a new table (Sample.CaveDwellers), then use INSERT OR UPDATE to populate this table with data. A SELECT \* example is provided to display the Sample.CaveDwellers data, which can be run at any point. Another example is provided to delete Sample.CaveDwellers.

CreateTable
   ZNSPACE "Samples"
   &sql(CREATE TABLE Sample.CaveDwellers (
  Num          INT NOT NULL,
  TrogIdentity INTEGER IDENTITY,
  CaveCluster  CHAR(20) NOT NULL,
  Troglodyte   CHAR(30) NOT NULL,
  CONSTRAINT CaveDwellerPK PRIMARY KEY (Num))
  )
  IF SQLCODE\=0 {WRITE !,"Table created" }
  ELSEIF SQLCODE\=\-201 {WRITE !,"Table already exists"}
  ELSE {WRITE !,"CREATE TABLE failed. SQLCODE=",SQLCODE }

 

SELECT \* FROM Sample.CaveDwellers

 

Run the following two examples one or more times in any order. They will insert records 1 thorough 5. If record 4 already exists, INSERT OR UPDATE will update it. Use the SELECT \* example to display the table data. Note that the IDENTITY field values have a gap in their sequence; this gap occurred when INSERT OR UPDATE performed an update, rather than an insert:

InsertOrUpdateIndividualRecords
  ZNSPACE "Samples"
  &sql(INSERT OR UPDATE INTO Sample.CaveDwellers (Num,CaveCluster,Troglodyte) VALUES 
    (1,'Bedrock','Flintstone,Fred'))
  IF SQLCODE \= 0 { SET rcount\=%ROWCOUNT }
  &sql(INSERT OR UPDATE INTO Sample.CaveDwellers (Num,CaveCluster,Troglodyte) VALUES 
    (4,'Bedrock','Flintstone,Wilma'))
  IF SQLCODE \= 0 { SET rcount\=rcount+%ROWCOUNT 
                   WRITE !,rcount," records inserted" }
  ELSE { WRITE !,"Insert failed, SQLCODE=",SQLCODE }

 

InsertOrUpdateWithQueryResults
   NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(INSERT OR UPDATE Sample.CaveDwellers
      (Num,CaveCluster,Troglodyte)
       SELECT %ID,Home\_City,Name
       FROM Sample.Person
       WHERE %ID BETWEEN 2 AND 5)
    IF SQLCODE\=0 {
    WRITE !,"Insert/Update succeeded"
    WRITE !,%ROWCOUNT," records inserted"
    WRITE !,"Row ID=",%ROWID }
    ELSE {
    WRITE !,"Insert/Update failed, SQLCODE=",SQLCODE }

 

The following example deletes the table:

DeleteTable
  ZNSPACE "Samples"
  &sql(DROP TABLE Sample.CaveDwellers)
  IF SQLCODE\=0 {WRITE !,"Table deleted" }
  ELSEIF SQLCODE\=\-30 {WRITE !,"Table does not exist"}
  ELSE {WRITE !,"DROP TABLE failed. SQLCODE=",SQLCODE }

 

See Also

*   [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable) [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) [DROP TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable) [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) [VALUES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_values)
    
*   “[Modifying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify)” chapter in Using Caché SQL
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Defining Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views)” chapter in Using Caché SQL
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_insertorupdate.xml**