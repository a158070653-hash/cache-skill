# VALUES

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_values

---

 

Caché SQL Reference

VALUES

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_usedatabase "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [VALUES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_values "VALUES") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An INSERT/UPDATE clause that specifies data values for use in fields.

Synopsis

(field1{,fieldn})
     VALUES (value1{,valuen})

Arguments

field

A field name or a comma-separated list of field names.

value

A value or comma-separated list of values. Each value is assigned to the corresponding field.

Description

The VALUES clause is used in an [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) or [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) statement to specify the data values to insert into the fields. Typically:

*   INSERT queries use the following syntax:
    
    INSERT INTO tablename (fieldname,fieldname,...)
         VALUES (value,...)
    
*   UPDATE queries use the following syntax:
    
    UPDATE tablename (fieldname,fieldname,...)
         VALUES (value,...)
    

The elements in the VALUES clause correspond in sequence to the fields specified after the table name. Note if there is only one value element specified in the VALUES clause, it is not necessary to enclose the element in parentheses.

The following embedded SQL example shows an INSERT statement that adds a single row to the "Employee" table:

   &sql(INSERT INTO Employee (Name,SocSec,Telephone)
        VALUES("Boswell",333448888,"546-7989"))

   &sql(INSERT INTO Employee (Name,SocSec,Telephone)
        VALUES ('Boswell',333448888,'546-7989'))

[INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) and [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) queries can use a VALUES clause without requiring you to explicitly specify a list of field names after the table name. In order to omit the list of field names after the table name, your query must meet the following two criteria:

*   The number of values specified in the VALUES clause is the same as the number of fields in the table (exclusive of the ID field).
    
*   The values in the VALUES clause are listed in order of the internal column numbers of the fields, beginning with column 2. Column 1 is always reserved for the system-generated ID field, and is not specified in a VALUES clause.
    

For example, the query:

INSERT INTO Sample.Person VALUES (5,'John')

is equivalent to the query:

INSERT INTO Sample.Person (Age,Name) VALUES (5,'John')

if the table "Sample.Person" has exactly two user-defined fields.

In this example, the value 5 is assigned to the field with the lower column number, and the value "John" is assigned to the other field.

A VALUES clause can specify an element of an array, as is the following embedded SQL example:

   &sql( UPDATE Person(Tel)
        VALUES :per('tel',)
        WHERE ID \= :id )

An UPDATE query can also reference an array with unspecified last subscript. Whereas INSERT uses the presence and absence of array elements to assign values and default values to a newly created row, UPDATE uses the presence of an array element to indicate that the corresponding field should be updated. For example, consider the following array for a table with six columns:

emp("profile",2)="Smith"
emp("profile",3)=2
emp("profile",3,1)="1441 Main St."
emp("profile",3,2)="Cableton, IL 60433"
emp("profile",5)=NULL
emp("profile",7)=25
emp("profile","next")="F"

Column 1 is always reserved for the ID field, and is not user-specified. The inserted "Employee" row has Column 2, "Name", set to "Smith"; Column 3, "Address", set to a two-line value; Column 4, "Department", is not specified, and is thus set to the default, and Column 5, "Location", is set to NULL. The default value for "Location" is not used since the corresponding array element is defined with a null value. The array elements "7" and "next" do not correspond to column numbers in the "Employee" table, therefore the query ignores them. Here’s the UPDATE statement that uses this array:

  &sql(UPDATE Employee
       VALUES :emp('profile',)
       WHERE Employee \= 379)

Given the above definitions and array values, this statement will update the values of the "Name", "Address", and "Location" fields of the "Employee" row for which Row ID = 379.

However, omitting the subscript entirely results in an SQLCODE -54 error: Array designator (last subscript omitted) expected after VALUES.

You may also use an array reference with an UPDATE query that targets multiple rows, for example:

  &sql(UPDATE Employee
       VALUES :emp('profile',)
       WHERE Type \= 'PART-TIME')

A VALUES clause variable cannot use dot syntax. Therefore, the following embedded SQL example is correct:

   SET sname = state.Name
   &sql(INSERT INTO StateTbl VALUES :sname)

The following is not correct:

     &sql(INSERT INTO State VALUES :state.Name)

NULL and empty string values are different. For further details, see [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null). For backward compatibility, all empty string ('') values in older existing data are considered as NULL values. In new data, empty strings are stored in the data as $CHAR(0). Through SQL, NULL is referenced as 'NULL'. For example:

INSERT INTO Sample.Person
(SSN,Name,Home\_City) VALUES ('123-45-6789','Doe,John',NULL)

Through SQL, empty string is referenced as '' (two single quotes). For example:

INSERT INTO Sample.Person
(SSN,Name,Home\_City) VALUES ('123-45-6789','Doe,John','')

You cannot insert a NULL value for the ID field.

Examples

The following embedded SQL example inserts a record for “Doe,John” into the Sample.Person table. It then selects this record, and then deletes this record. A second SELECT confirms the deletion.

   SET x\="Doe,John",y\="123-45-6789",z\="Metropolis"
   SET (a,b,c,d,e)\=0
   NEW SQLCODE,%ROWCOUNT,%ROWID
   &sql(INSERT INTO Sample.Person
   (Name,SSN,Home\_City) VALUES (:x,:y,:z))
   IF SQLCODE'=0 {
     WRITE !,"INSERT Error code ",SQLCODE
     QUIT }
   &sql(SELECT Name,SSN,Home\_City
        INTO :a,:b,:c
        FROM Sample.Person
        WHERE Name \=:x)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
      WRITE !,"After INSERT:"
      WRITE !,"Name=",a," SSN=",b," City=",c
      WRITE !,"SQL code=",SQLCODE," Number of rows=",%ROWCOUNT }
    &sql(DELETE FROM Sample.Person
        WHERE Name\=:x)
    &sql(SELECT Name,SSN
        INTO :d,:e
        FROM Sample.Person
        WHERE Name\='Doe,John')
   IF SQLCODE <0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"After DELETE:"
     WRITE !,"Name=",d," SSN=",e
     WRITE !,"SQL code=",SQLCODE," Number of rows=",%ROWCOUNT }

 

See Also

*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert)
    
*   [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update)
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_usedatabase "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_values.xml**