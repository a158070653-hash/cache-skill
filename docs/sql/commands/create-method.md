# CREATE METHOD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createmethod

---

 

Caché SQL Reference

CREATE METHOD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod "CREATE METHOD") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a method in a class.

Synopsis

CREATE \[STATIC\] METHOD name (parameter\_list) 
    characteristics 
    \[LANGUAGE OBJECTSCRIPT | LANGUAGE SQL\]
    code\_body

Arguments

name

The name of the method to be created, which is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). This method name may be unqualified (StoreName) or qualified (Patient.StoreName). The name must be followed by parentheses, even if no parameters are specified. For further details see the “Identifiers” chapter of Using Caché SQL.

parameter\_list

Optional — A list of parameters to pass to the method. The parameter list is enclosed in parentheses, and parameters in the list are separated by commas. The parentheses are mandatory, even when no parameters are specified.

characteristics

Optional — One or more keywords specifying the characteristics of the method. Permitted keywords are RETURNS, FOR, FINAL, PRIVATE, PROCEDURE, SELECTMODE.

You can specify the characteristics keyword phrase RESULT SETS, DYNAMIC RESULT SETS, or DYNAMIC RESULT SETS n, where n is an integer. These phrases are synonyms; the DYNAMIC keyword and the n integer are no-ops provided for compatibility.

Multiple characteristics are separated by whitespace (a space or line break). Characteristics can be specified in any order.

LANGUAGE OBJECTSCRIPT

LANGUAGE SQL

Optional — The programming language used for code\_body. Specify LANGUAGE OBJECTSCRIPT (for Caché ObjectScript) or LANGUAGE SQL. If the LANGUAGE clause is omitted, SQL is the default.

code\_body

The program code for the method.

SQL program code is prefaced with a BEGIN keyword and concludes with an END keyword. Each complete SQL statement within code\_body ends with a semicolon (;).

Caché ObjectScript program code is enclosed in curly braces. ObjectScript code lines must be indented.

Description

The CREATE METHOD statement creates a class method. This class method may or may not be a stored procedure. To create a method in a class that is exposed as an SQL stored procedure, you must specify the PROCEDURE keyword. By default, CREATE METHOD does not create a method which is also a stored procedure; the [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure) statement always creates a method which is also a stored procedure.

The optional STATIC keyword is provided to clarify that the method created is a static (class) method, not an instance method. This keyword provides no actual functionality.

In order to create a method, you must have %CREATE\_METHOD administrative privilege, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. If you are attempting to create a method for an existing class with a defined owner, you must be logged in as the owner of the class. Otherwise, the operation fails with an SQLCODE -99 error.

For information on calling methods from within SQL statements, refer to [User-defined Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_funcs) in the “Querying the Database” chapter of Using Caché SQL. For calling SQL stored procedures in a variety of contexts, refer to the [CALL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call) statement.

Arguments

name

The name of the method to be created. This name may be unqualified (StoreName) and take the default schema name, or qualified by specifying the schema name (Patient.StoreName). If you specify \_CURRENT\_USER as the schema name, Caché uses the default schema name. You can use the [$SYSTEM.SQL.DefaultSchema()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DefaultSchema) method to determine the default schema name. The default schema name SQLUser corresponds to the class package name User.

Note that the FOR characteristic (described below) overrides the class name specified in name. If a method with this name already exists, the operation fails with an SQLCODE -361 error.

If the method name is unqualified, the name of the generated class is the default schema name, followed by a dot, followed by “meth”, followed by the specified name. Thus the unqualified name StoreName results in a class name such as the following: User.methStoreName. For further details, see [SQL to Class Name Transformations](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_procedures#GSQL_procedures_sqltoclass) in the “Defining and Using Stored Procedures” chapter of Using Caché SQL.

Caché SQL does not allow you to specify a duplicate method name that differs only in letter case. Specifying a method name that differs only in letter case from an existing method name results in an SQLCODE -400 error.

parameter-list

A list of parameters used to pass values to the method. The parameter list is enclosed in parentheses, and parameter declarations in the list are separated by commas. The parentheses are mandatory, even when specifying no parameters. Each parameter declaration in the list consists of (in order):

*   An optional keyword specifying whether the parameter mode is IN (input value), OUT (output value), or INOUT (modify value). If omitted, the default parameter mode is IN.
    
*   The parameter name. Parameter names are case-sensitive.
    
*   The [data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) of the parameter.
    
*   Optional: A default value for the parameter. You can specify the DEFAULT keyword followed by a default value; the DEFAULT keyword is optional. If no default is specified, the assumed default is NULL.
    

The output value from a method is automatically converted from Logical format to Display/ODBC format.

An input value to a method is, by default, not converted from Display/ODBC format to Logical format. However, input display-to-logical conversion can be configured systemwide using the [$SYSTEM.SQL.SetSQLFunctionArgConversion()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetSQLFunctionArgConversion) method. You can use [$SYSTEM.SQL.GetSQLFunctionArgConversion()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetSQLFunctionArgConversion) to determine the current configuration of this option.

The following example specifies two input parameters, both of which have default values. The optional DEFAULT keyword is specified for the first parameter, omitted for the second parameter:

CREATE METHOD RandomLetter(IN firstlet CHAR DEFAULT 'A',IN lastlet CHAR 'Z')
BEGIN
\-- SQL program code
END

characteristics

The available keywords are as follows:

FOR className

Specifies the name of the class in which to create the method. If the class does not exist, it will be created. You can also specify a class name by qualifying the method name. The class name specified in the FOR clause overrides a class name specified by qualifying the method name.

FINAL

Specifies that subclasses cannot override the method. By default, methods are not final. The FINAL keyword is inherited by subclasses.

PRIVATE

Specifies that the method can only be invoked by other methods of its own class or subclasses. By default, a method is public, and can be invoked without restriction. This restriction is inherited by subclasses.

PROCEDURE

Specifies that the method is an SQL stored procedure. Stored procedures are inherited by subclasses. (This keyword can be abbreviated as PROC.)

RESULT SETS

DYNAMIC RESULT SETS \[n\]

Specifies that the method created will contain the [ReturnResultsets](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_method_returnresultsets) keyword. All forms of this characteristics phrase are synonyms.

RETURNS datatype

Specifies the [data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) of the value returned by a call to the method. If RETURNS is omitted, the method cannot return a value. This specification is inherited by subclasses, and can be modified by subclasses. This datatype can specify type parameters such as MINVAL, MAXVAL, and SCALE. For example RETURNS DECIMAL(19,4). Note that when returning a value, Caché ignores the length of datatype; for example, RETURNS VARCHAR(32) can receive a string of any length that is returned by a call to the method.

SELECTMODE mode

Only used when LANGUAGE is SQL (the default). When specified, Caché adds an [#SQLCOMPILE SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select)\=mode statement to the corresponding class method, thus generating the SQL statements defined in the method with the specified SELECTMODE. The possible mode values are LOGICAL, ODBC, RUNTIME, and DISPLAY. The default is LOGICAL.

If you specify a query keyword (such as CONTAINSID or RESULTS) that is not valid for a method, Caché generates an SQLCODE -47 error. If you specify a duplicate query keyword (such as FINAL FINAL), Caché generates an SQLCODE -44 error.

The SELECTMODE clause is used for SELECT query operations and for INSERT and UPDATE operations. It specifies the compile-time select mode. The value that you specify for SELECTMODE is added at the beginning of the Caché ObjectScript class method code as: #SQLCompile Select=mode. For further details, see [#SQLCompile Select](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select) in the “ObjectScript Macros and the Macro Preprocessor” chapter of Using Caché ObjectScript.

*   In a SELECT query, the SELECTMODE specifies the mode in which data is returned. If the mode value is LOGICAL, then logical (internal storage) values are returned. For example, dates are returned in $HOROLOG format. If the mode value is ODBC, logical-to-ODBC conversion is applied, and ODBC format values are returned. If the mode value is DISPLAY, logical-to-display conversion is applied, and display format values are returned. If the mode value is RUNTIME, the display mode can be set (to LOGICAL, ODBC, or DISPLAY) at execution time.
    
*   In an [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) or [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) operation, the SELECTMODE RUNTIME option supports automatic conversion of input data values from a display format (DISPLAY or ODBC) to logical storage format. This compiled display-to-logical data conversion code is applied only if the select mode setting when the SQL code is executed is LOGICAL (which is the default for all Caché SQL execution interfaces).
    

When the SQL code is executed, the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement#%SelectMode) property specifies the execution-time select mode, as described in “[Using Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql)” chapter of Using Caché SQL. For further details on SelectMode options, refer to “[Data Display Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_selectmode)” in the “Caché SQL Basics” chapter of Using Caché SQL.

LANGUAGE

A keyword clause specifying the language you are using for code\_body. Permitted clauses are LANGUAGE OBJECTSCRIPT (for Caché ObjectScript) or LANGUAGE SQL. If the LANGUAGE clause is omitted, SQL is the default.

code\_body

The program code for the method to be created. You specify this code in either SQL or Caché ObjectScript. The language used must match the LANGUAGE clause. However, code specified in Caché ObjectScript can contain embedded SQL.

Caché uses the code you supply to generate the actual code of the method.

If the code you specify is SQL, Caché provides additional lines of code when generating the method that embed the SQL in a Caché ObjectScript “wrapper,” provide a procedure context handler (if necessary), and handle return values. The following is an example of this Caché-generated wrapper code:

   NEW SQLCODE,%ROWID,%ROWCOUNT,title
   &sql( SELECT col FROM tbl )
   QUIT $GET(title)

If the code you specify is OBJECTSCRIPT, the Caché ObjectScript code must be enclosed in curly braces. All code lines must be indented from column 1, except for labels and macro preprocessor directives. A label or macro directive must be prefaced by a colon (:) in column 1.

For Caché ObjectScript code, you must explicitly define the “wrapper” (which NEWs variable and uses QUIT exit and (optionally) to return a value upon completion).

The method can be exposed as a stored procedure by specifying the PROCEDURE keyword. When a stored procedure is called, an object of the class [%Library.SQLProcContext](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.SQLProcContext) is instantiated in the %sqlcontext variable. This procedure context handler is used to pass the procedure context back and forth between the procedure and its caller (for example, the ODBC server).

%sqlcontext consists of several properties, including an Error object, the SQLCODE error status, the SQL row count, and an error message. The following example shows the values used to set several of these:

  SET %sqlcontext.%SQLCODE=SQLCODE
  SET %sqlcontext.%ROWCOUNT=%ROWCOUNT
  SET %sqlcontext.%Message=%msg

The values of SQLCODE and %ROWCOUNT are automatically set by the execution of an SQL statement. The %sqlcontext object is reset before each execution.

Alternatively, an error context can be established by instantiating a %SYSTEM.Error object and setting it as %sqlcontext.Error.

Examples

The following example uses CREATE METHOD with SQL code to generate the method UpdateSalary in the class Sample.Employee:

CREATE METHOD UpdateSalary ( IN SSN VARCHAR(11), IN Salary INTEGER )
   FOR Sample.Employee
   BEGIN
     UPDATE Sample.Employee SET Salary \= :Salary WHERE SSN \= :SSN;
   END

The following example creates the RandomLetter() method stored as a procedure that generates a random capital letter. You can then invoke this method as a function in a SELECT statement. A DROP METHOD is provided to delete the RandomLetter() method.

CREATE METHOD RandomLetter()
RETURNS INTEGER
PROCEDURE
LANGUAGE OBJECTSCRIPT
{
:Top
 SET x\=$RANDOM(90)
 IF x<65 {GOTO Top}
 ELSE {QUIT $CHAR(x)}
}

 

SELECT Name FROM Sample.Person
WHERE Name %STARTSWITH RandomLetter()

 

DROP METHOD RandomLetter

 

The following Embedded SQL example uses CREATE METHOD with Caché ObjectScript code to generate the method TraineeTitle in the class Sample.MyStudents and returns a Title value:

  &sql(CREATE METHOD TraineeTitle(
   IN SSN VARCHAR(11), 
   INOUT Title VARCHAR(50) )
    RETURNS VARCHAR(30)
    FOR Sample.MyStudents
    LANGUAGE OBJECTSCRIPT
    {
        NEW SQLCODE,%ROWCOUNT
        &sql(SELECT Title INTO :Title FROM Sample.Employee 
             WHERE SSN \= :SSN)
        IF $GET(%sqlcontext)'= "" {
           SET %sqlcontext.%SQLCODE\=SQLCODE
           SET %sqlcontext.%ROWCOUNT\=%ROWCOUNT }
           QUIT
     })
    IF SQLCODE\=0 { WRITE !,"Created a method" QUIT}
    ELSEIF SQLCODE\=\-361 { WRITE !,"Method already exists SQLCODE: ",SQLCODE
      &sql(DROP METHOD TraineeTitle FROM Sample.MyStudents)
      IF SQLCODE\=0 { WRITE !,"Dropped a method" QUIT}}
    ELSE { WRITE !,"SQL error: ",SQLCODE }

 

It uses the %sqlcontext object, and sets its %SQLCODE and %ROWCOUNT properties using the corresponding SQL variables. Note the curly braces enclosing the Caché ObjectScript code following the method’s LANGUAGE OBJECTSCRIPT keyword. Within the Caché ObjectScript code there is [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) code, marked by &sql and enclosed in parentheses.

See Also

*   [CALL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call)
    
*   [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure)
    
*   [DROP METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropmethod)
    
*   “[Defining and Using Stored Procedures](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_procedures)” chapter in Using Caché SQL.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createmethod.xml**