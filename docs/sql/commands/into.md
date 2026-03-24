# INTO

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_into

---

 

Caché SQL Reference

INTO

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_intransaction "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [INTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into "INTO") \]

Go to: Description Host Variable List Examples Host Variable Array Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that specifies the storing of selected values in host variables.

Synopsis

INTO :hostvar1 \[,:hostvar2\]...

Arguments

:hostvar1

A [host variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) that has been declared in the host language. When specified in an INTO clause, the variable name is preceded by a colon (:). A host variable can be a local variable (unsubscripted or subscripted) or an object property. You can specify multiple variables as a comma-separated list, as a single subscripted array variable, or a combination of a comma-separated list and a single subscripted array variable.

Description

The INTO clause and host variables are only used in [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql). They are not used in [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql). In Dynamic SQL, similar functionality for output variables is provided by the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class.

An INTO clause can be used in a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select), [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare), or [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch) statement. The INTO clause is identical for all three statements; examples on this page all refer to the SELECT statement. For usage with DECLARE and FETCH, refer to “[SQL Cursors](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor)” in the “Using Embedded SQL” chapter of Using Caché SQL.

The INTO clause uses the values retrieved (or calculated) in the SELECT select-item list to set corresponding host variables, making these returned data values available to Caché ObjectScript. In a SELECT the optional INTO clause appears after the select-item list and before the FROM clause.

Host Variables

A host variable can contain only a single value. Therefore, a SELECT in embedded SQL only retrieves one row of data. This defaults to the first row of the table. You can, of course, retrieve data from some other row of the table by limiting the eligible rows using a WHERE condition.

In embedded SQL you can return data from multiple rows by declaring a cursor and then issuing a FETCH for each successive row. The INTO clause host variables can be specified in the DECLARE query or specified in the FETCH.

INTO clause host variables can be specified in either of two ways (or a combination of both):

*   [A host variable list](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into#RSQL_into_hvlist), consisting of a comma-separated list of host variables, one for each select-item.
    
*   [A host variable array](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into#RSQL_into_hvarray), consisting of a single subscripted host variable.
    

For important restrictions on the use of host variable values in the containing program, refer to the [Host Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) section of the “Embedded SQL” chapter of Using Caché SQL.

Note:

If the host language declares data types for variables, all host variables must be declared in the host language before invoking the SELECT statement. The data types of the retrieved field values must match the host variable declarations. (Caché ObjectScript does not declare data types for variables.)

Using a Host Variable List

The following rules apply when you specify a host variable list in the INTO clause:

*   The number of host variables in the INTO clause must match the number of fields specified in the select-item list. If the number of selected fields and host variables differs, SQL returns a “cardinality mismatch” error.
    
*   Selected fields and host variables are matched by relative position. Therefore, the corresponding items in these two lists must appear in the same sequence.
    
*   The listed host variables may be any combination of unsubscripted or subscripted variables.
    
*   A listed host variable can return an aggregate value (such as a count, sum, or average) or a function value.
    
*   A listed host variable can return %CLASSNAME and %TABLENAME values.
    
*   Listed host variables can return field values from a SELECT involving multiple tables, or return values from a SELECT with no FROM clause.
    

The following example selects four fields into a list of four host variables. The host variables in this example are subscripted:

  &sql(SELECT %ID,Home\_City,Name,SSN 
        INTO :mydata(1),:mydata(2),:mydata(3),:mydata(4)
        FROM Sample.Person
        WHERE Home\_State\='MA' )
  IF SQLCODE\=0 {
     FOR i\=1:1:15 { 
       IF $DATA(mydata(i)) {
       WRITE "field ",i," = ",mydata(i),! }
     } }
   ELSE {WRITE "SQLCODE=",SQLCODE,! }

 

For further examples refer to [Host Variable List Examples](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into#RSQL_into_exmplehvlist), below.

Using a Host Variable Array

A host variable array uses a single subscripted variable to contain all of the selected field values. This array is populated according to the order of field definition in the table, not the order of fields in the select-item list.

The following rules apply when using a host variable array in the INTO clause:

*   The fields specified in the select-item list are selected into subscripts of a single host variable. Therefore, you do not have to match the number of items in the select-item list with the host variable count.
    
*   The host variable subscripts are populated by the corresponding field position in the table definition. For example, the 6th field, as defined in the table definition, corresponds to mydata(6). All subscripts that do not correspond to a specified select-item remain undefined. The order of the items in the select-item has no effect on how subscripts are populated.
    
*   A host variable array can only return field values from a single table.
    
*   A host variable array can only return field values. It cannot return an aggregate value (such as a count, sum, or average), a function value, or a %CLASSNAME or %TABLENAME value. (You can return these by specifying a host variable argument that combines host variable list items with the host variable array.)
    

The following example selects four fields into a host variable array:

  &sql(SELECT %ID,Home\_City,Name,SSN
        INTO :mydata()   
        FROM Sample.Person
        WHERE Home\_State\='MA' )
   IF SQLCODE\=0 {
     FOR i\=0:1:15 { 
       IF $DATA(mydata(i)) {
       WRITE "field ",i," = ",mydata(i),! }
     } }
   ELSE {WRITE "SQLCODE=",SQLCODE,! }

 

For further examples refer to [Host Variable Array Examples](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into#RSQL_into_exmplehvarray), below.

For further details, refer to “[Host Variable as a Subscripted Array](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvarray)” in the “Using Embedded SQL” chapter of Using Caché SQL.

Host Variable List Examples

The following Embedded SQL example selects three fields from the first record in the table (Embedded SQL always retrieves a single record), and uses INTO to set three corresponding unsubscripted host variables. These variables are then used by the Caché ObjectScript WRITE commands. It is considered good program practice to immediately test the SQLCODE variable upon returning from Embedded SQL. If SQLCODE is not equal to 0, the values of output host variables are indeterminate.

   WRITE !,"Going to get the first record"
   &sql(SELECT Home\_State, Name, Age 
        INTO :state, :name, :age   
        FROM Sample.Person)
   IF SQLCODE\=0 {
     WRITE !,"  Name=",name
     WRITE !,"  Age=",age
     WRITE !,"  Home State=",state }
   ELSE {
     WRITE !,"SQL error ",SQLCODE  }

 

The following Embedded SQL example passes a host variable (today) into the SELECT statement, where a calculation results in the INTO clause variable value (:tomorrow). This host variable is passed out to the containing program. This SQL query does not require a FROM clause.

   SET today\=$HOROLOG
   &sql(SELECT :today+1
        INTO :tomorrow )
   IF SQLCODE\=0 {
        WRITE !,"Tomorrow is: ",$ZDATE(tomorrow) }
   ELSE {
        WRITE !,"SQL error ",SQLCODE  }

 

For restrictions on the use of input and output host variable values, refer to the [Host Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) section of the “Embedded SQL” chapter of Using Caché SQL.

The following Embedded SQL example returns aggregate values. It uses the COUNT aggregate function to count the records in a table and AVG to average the Salary field values. The INTO clause returns these values to Caché ObjectScript as two subscripted host variables:

   WRITE !,"Counting the records"
   &sql(SELECT COUNT(\*),AVG(Salary)
        INTO :agg(1),:agg(2)
        FROM Sample.Employee)
   IF SQLCODE\=0 {
        WRITE !,"Total Eymployee records= ",agg(1)
        WRITE !,"Average Employee salary= ",agg(2) }
   ELSE {
        WRITE !,"SQL error ",SQLCODE  }   

 

The following Embedded SQL example returns field values from a row resulting from the join of two tables. You must use a host variable list when returning fields from more than one table:

    &sql(SELECT P.Name,E.Title,E.Name,P.%TABLENAME,E.%TABLENAME 
        INTO :name(1),:title,:name(2),:ptname,:etname
        FROM Sample.Person AS P LEFT JOIN
             Sample.Employee AS E ON E.Name %STARTSWITH 'B'
        WHERE P.Name %STARTSWITH 'A')
   IF SQLCODE\=0 {
        WRITE ptname," = ",name(1),!
        WRITE etname," = ",title,!
        WRITE etname," = ",name(2) }
   ELSE {
        WRITE !,"SQL error ",SQLCODE  }   

 

Host Variable Array Examples

The following two Embedded SQL examples uses a host variable array to return the non-hidden data field values from a row. In these examples [%ID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_idfield) is specified in the select-item list, because, by default, SELECT \* does not return the RowId (though it does for Sample.Person); the RowId is always field 1. Note in Sample.Person fields 4 and 9 can take NULL, field 5 is not a data field (it references Sample.Address), and field 10 is hidden.

The first example returns a specified number of fields (firstflds); hidden and non-data fields are included in this count, though not displayed. Using firstflds would be appropriate when returning a row from a table with many fields. Note that this example can return Field 0, which is the parent reference. Sample.Person is not a child table, so tflds(0) is undefined:

  &sql(SELECT \*,%ID INTO :tflds()   
        FROM Sample.Person )
   IF SQLCODE\=0 {
     SET firstflds\=14
     FOR i\=0:1:firstflds { 
       IF $DATA(tflds(i)) {
       WRITE "field ",i," = ",tflds(i),! }
     } }
   ELSE {WRITE "SQLCODE error=",SQLCODE,! }

 

The second example returns all the non-hidden data fields in Sample.Person. Note that this example does not attempt to return Field 0, the parent reference, because in Sample.Person tflds(0) is undefined, and would therefore generate an <UNDEFINED> error:

  &sql(SELECT \*,%ID INTO :tflds()   
        FROM Sample.Person )
  IF SQLCODE\=0 {
      SET x\=1
      WHILE x '="" {
      WRITE "field ",x," = ",tflds(x),!
      SET x\=$ORDER(tflds(x)) }
      }
  ELSE { WRITE "SQLCODE error=",SQLCODE,! }

 

The following Embedded SQL example combines a comma-separated host variable list (for non-field values) and a host variable array (for field values):

  &sql(SELECT %TABLENAME,Name,Age,AVG(Age)
        INTO :tname,:tflds(),:ageavg
        FROM Sample.Person
        WHERE Age \> 50 )
   IF SQLCODE\=0 {
     WRITE "Table name is = ",tname,!
     FOR i\=0:1:25 { 
       IF $DATA(tflds(i)) {
       WRITE "field ",i," = ",tflds(i),! }
     } 
       WRITE "Average age is = ",ageavg,! }
   ELSE {WRITE "SQLCODE=",SQLCODE,! }

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select), [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare), [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch) statements
    
*   [VALUES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_values) clause
    
*   “[Host Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars)” in the “Using Embedded SQL” chapter of Using Caché SQL
    
*   Caché ObjectScript: [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_intransaction "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_into.xml**