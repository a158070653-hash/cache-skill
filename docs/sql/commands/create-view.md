# CREATE VIEW

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createview

---

 

Caché SQL Reference

CREATE VIEW

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview "CREATE VIEW") \]

Go to: Description Updating Through Views Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a view.

Synopsis

CREATE \[OR REPLACE\] VIEW view-name \[(column-commalist)\]
       AS select-statement 
       \[ WITH READ ONLY  |  WITH \[level\] CHECK OPTION \]

Arguments

view-name

The [name for the view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview#RSQL_createview_name) being created. A valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers), subject to the same additional naming restrictions as a [table name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables). A view name can be qualified (schema.viewname) or unqualified. Note that you cannot use the same name for a table and a view in the same schema.

column-commalist

Optional — The column names that compose the view, one or more valid [identifiers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). If specified, this list is enclosed in parentheses and items in the list are separated by commas.

AS select-statement

A [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement that defines the view.

WITH READ ONLY

Optional — Specifies that no insert, update, or delete operations can be performed through this view upon the table on which the view is based. The default is to permit these operations through a view, subject to the constraints described below.

WITH level CHECK OPTION

Optional — Specifies how insert, update, or delete operations are performed through this view upon the table on which the view is based. The level can be the keywords LOCAL or CASCADED. If no level is specified, the WITH CHECK OPTION default is CASCADED.

Description

The CREATE VIEW command defines the content of a [view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views). The SELECT statement that defines the view can reference more than one table and can reference other views.

Privileges

To select from the objects referenced in the SELECT clause of a view being created, it is necessary to have the appropriate privileges:

*   You must have SELECT privileges for all tables (or views) referenced by the view. If you do not have SELECT privilege for a specified table (or view) the CREATE VIEW command will not execute.
    
*   To receive SELECT privilege WITH GRANT OPTION for a view, you must have WITH GRANT OPTION for every table (or view) referenced by the view.
    
*   To receive INSERT, UPDATE, DELETE, or REFERENCES privilege for a view, you must have the same privilege for every table (or view) referenced by the view. To receive WITH GRANT OPTION for any of these privileges, you must hold the privilege WITH GRANT OPTION on the underlying tables.
    
*   If the view is specified WITH READ ONLY, the view is not granted INSERT, UPDATE, or DELETE privileges, regardless of the privileges you hold for the underlying tables. If the view is later redefined as read/write, these privileges are added when the class projecting the view is recompiled.
    

You can determine if the current user has these table-level privileges by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has these table-level privileges by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

The creator (owner) of a view is granted the %ALTER privilege WITH GRANT OPTION when the view is compiled.

The CREATE VIEW command is a privileged operation. Prior to using CREATE VIEW it is necessary for your process to have %CREATE\_VIEW privileges. Failing to do so results in an SQLCODE -99 error (Privilege Violation). You can use the GRANT command to assign %CREATE\_VIEW privileges, if you hold appropriate granting privileges.

In embedded SQL, you can use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to log in as a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

%CREATE\_VIEW privileges are assigned using the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command, which requires you to assign this privilege to a user or role. This requirement is configurable:

*   The [$SYSTEM.SQL.SetSQLSecurity()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetSQLSecurity) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a SQL Security ON: setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of SQL Security Enabled.
    

The default is “Yes” (1). When “Yes”, a user can only perform actions on a table or view for which that user has been granted privilege. This is the recommended setting for this option.

If this option is set to “No” (0), SQL Security is disabled for any new process started after changing this setting. This means privilege-based table/view security is suppressed. You can create a table without specifying a user. In this case, Dynamic SQL assigns “\_SYSTEM” as user, and embedded SQL assigns "" (the empty string) as user. Any user can perform actions on a table or view even if that user has no privileges to do so.

View Naming Conventions

A view name has the same naming conventions as a table name, and shares the same name set. Therefore, you cannot use the same name for a table and a view in the same schema. Attempting to do so results in an SQLCODE -201 error. A class that projects a table definition and a view definition with the same name also generates an SQLCODE -201 error.

View names follow [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) conventions, subject to the restrictions below. By default, view names are simple identifiers. A view name should not exceed 128 characters. View names are not case-sensitive. For further details see the “Identifiers” chapter of Using Caché SQL.

Caché uses the view name to generate a corresponding class name. A class name contains only alphanumeric characters (letters and numbers) and must be unique within the first 96 characters. To generate this class name, Caché first strips punctuation characters from the view name, and then generates a identifier that is unique within the first 96 characters, substituting an integer (beginning with 0) for the final character when needed to create a unique class name. Caché generates a unique class name from a valid view name, but this name generation imposes the following restrictions on the naming of views:

*   A view name must include at least one letter. Either the first character of the view name or the first character after initial punctuation characters must be a letter.
    
*   Caché supports 16-bit (wide) characters for view names on Unicode systems. A character is a valid letter if it passes the [$ZNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname) test.
    
*   If the first character of the view name is a punctuation character, the second character cannot be a number. This results in an SQLCODE -400 error, with a %msg value of “ERROR #5053: Class name 'schema.name' is invalid” (without the punctuation character). For example, specifying the view name %7A generates the %msg “ERROR #5053: Class name 'User.7A' is invalid”.
    
*   Because generated class names do not include punctuation characters, it is not advisable (though possible) to create a view name that differs from an existing view or table name only in its punctuation characters. In this case, Caché substitutes an integer (beginning with 0) for the final character of the name to create a unique class name.
    
*   A view name may be much longer than 96 characters, but view names that differ in their first 96 alphanumeric characters are much easier to work with.
    

A view name can be qualified or unqualified. A qualified view name has the following syntax:

schema.viewname

A qualified view name can specify an existing schema or a new schema. If it specifies a new schema, Caché creates that schema.

An unqualified view name takes as default the schema SQLUser (or a configured system default schema name). Caché uses the schema name to generate a corresponding class package name. SQLUser generates the package name User. For further details on schema names and default value, see the “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL.

Existing View

To determine if a specified view already exists in the current namespace, use the [$SYSTEM.SQL.ViewExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#ViewExists) method.

What happens when you try to create a view that has the same name as an existing view depends on the optional OR REPLACE keyword and on the configuration setting.

With OR REPLACE

If you specify CREATE OR REPLACE VIEW, the existing view is replaced by the view definition specified in the SELECT clause and any specified WITH READ ONLY or WITH CHECK OPTION. This is the same as performing the corresponding [ALTER VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alterview) statement. Any privileges that had been granted to the original view remain.

This keyword phrase provides no functionality not available through ALTER VIEW. It is provided for compatibility with Oracle SQL code.

Without OR REPLACE

If you specify CREATE VIEW, Caché, by default, rejects an attempt to create a view with the name of an existing view and issues an SQLCODE -201 error. This behavior is configurable. Note that changing this behavior affects both CREATE VIEW and CREATE TABLE. You can configure this behavior as follows:

*   The [$SYSTEM.SQL.SetDDLNo201()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo201) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-201 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow DDL CREATE TABLE or CREATE VIEW for Existing Table.
    

The default is “No” (0). This is the recommended setting for this option. If this option is set to “Yes” (1), Caché deletes the class definition associated with the view and then recreates it. This is much the same as performing a DROP VIEW and then performing a CREATE VIEW.

Column Names

A view can optionally include a column-commalist list of column names, enclosed in parentheses. These column names, if specified, are the names used to access and display the data for the columns when using that view. If the list of column names is omitted, the column names of the SELECT source table are used. If you omit the list of column names, you must also omit the parentheses.

If you specify the column-commalist, the following apply:

*   A column name list must specify the enclosing parentheses, even when specifying a single field. You must separate multiple column names with commas. Whitespace and [comments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_comments) are permitted within a column-commalist.
    
*   The number of column names must correspond to the number of fields specified in the SELECT statement. Mismatch between the number of view columns and query columns results in an SQLCODE -142 error at compile time.
    
*   The names of column names must be valid [identifiers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). They may be different names than the SELECT field names, the same names as the SELECT field names, or a combination of both. The specified order of the view column names corresponds to the order of the SELECT field names. Because it is possible to assign a view column the name of an unrelated SELECT field, you must exercise caution when assigning view column names.
    

The following example shows a CREATE VIEW with matching lists of view columns and query columns:

CREATE VIEW MyView (ViewCol1, ViewCol2, ViewCol3) AS
     SELECT TableCol1, TableCol2, TableCol3 
     FROM MyTable

Alternatively, you can use the AS keyword in the query to specify the view columns as query column / view column pairs, as shown in the following example:

CREATE VIEW MyView AS 
  SELECT TableCol1 AS ViewCol1,
     TableCol2 AS ViewCol2,
     TableCol3 AS ViewCol3
     FROM MyTable

SELECT Clause Considerations

A view does not have to be a simple subset of the rows and columns of one particular table. A view can be created using more than one table or other views with a SELECT clause of any complexity. There are, however, a few restrictions on the SELECT clauses in a view definition. A CREATE VIEW statement:

*   Can only include an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause if this clause is paired with a [TOP clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_top). If you wish to include all of the rows in the view, you can use a TOP ALL clause. You can include a TOP clause without an ORDER BY clause. However, if you include an ORDER BY clause without a TOP clause, an SQLCODE -143 error is generated. If you project an SQL view from a view class, the query of which contains an ORDER BY clause, the ORDER BY clause is ignored in the view projection.
    
*   Cannot contain [host variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) or include the [INTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into) keyword. If you attempt to reference a host variable in the SELECT clause, Caché generates an SQLCODE -148 error.
    
*   Cannot reference a temporary table.
    
*   May have a [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) clause.
    
*   May use functions.
    

CREATE VIEW can contain a [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union) statement to select columns from the union of two tables. You can specify a UNION as shown in the following embedded SQL example:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  &sql(CREATE VIEW MyView (vname,vstate) AS
  SELECT t1.name,t1.home\_state
    FROM Sample.Person AS t1
  UNION
  SELECT t2.name,t2.office\_state
    FROM Sample.Employee AS t2)
  IF SQLCODE\=0 { WRITE !,"Created view" }
  ELSE { WRITE "CREATE VIEW error SQLCODE=",SQLCODE }

 

Note that an unqualified view name, such as in the above example, defaults to the system-wide default SQL schema name (in this case, User.MyView), even though the tables referenced by the view are in the Sample schema. Thus it is usually a good practice to always qualify a view name to ensure that it is stored with its associated table(s).

View ID: %vid

When data is accessed through a view, Caché assigns a sequential integer view ID (%vid) to each row returned by that view. Like table row ID numbers, these view row ID numbers are system-assigned, unique, non-zero, non-null, and non-modifiable. This %vid is usually invisible. Unlike a table row ID, it is not displayed when using asterisk syntax; it is only displayed when explicitly specified in the SELECT. The %vid can be used to further restrict the number of rows returned by a SELECT accessing a view. For further details on using %vid, refer to the [Defining and Using Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) chapter of Using Caché SQL.

Updating Through Views

A view can be used to update the tables on which the view is based. You can [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) new rows through the view, [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) data in rows seen through the view, and [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) rows seen through the view. INSERT, UPDATE, and DELETE statements can be issued for a view, if the CREATE VIEW statement specified this ability. To allow updating through a view, specify WITH CHECK OPTION (the default) when defining the view.

To prevent updating through a view, specify WITH READ ONLY. Attempting an INSERT, UPDATE, or DELETE through a view created WITH READ ONLY generates an SQLCODE -35 error.

In order to update through a view, you must have the appropriate privileges for the table or view to be updated, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

Updating through views is subject to the following restrictions:

*   The view cannot be a class query projected as a view.
    
*   The view’s class cannot contain the class parameter READONLY=1.
    
*   The view’s SELECT statement cannot contain a DISTINCT, TOP, GROUP BY, or HAVING clause, or be part of a UNION.
    
*   The view’s SELECT statement cannot contain a subquery.
    
*   The view’s SELECT statement can only list value expressions that are column references.
    
*   The view’s SELECT statement can have only one table reference, and it must specify either an updateable table or an updateable view.
    

The WITH CHECK OPTION clause causes an insert or update operation to validate the resulting row against the [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause of the view definition. This ensures that the inserted or modified row is part of the derived view table. There are two available check options:

WITH LOCAL CHECK OPTION — only the WHERE clause of the view specified in the INSERT or UPDATE statement is checked.

WITH CASCADED CHECK OPTION — the WHERE clause of the view specified in the INSERT or UPDATE statement and all underlying views are checked. This overrides any WITH LOCAL CHECK OPTION clauses in these underlying views. CASCADED is the default and is recommended for all updateable views.

If an INSERT operation fails WITH CHECK OPTION validation (as defined above), Caché issues an SQLCODE -136 error.

If an UPDATE operation fails WITH CHECK OPTION validation (as defined above), Caché issues an SQLCODE -137 error.

Examples

The following example creates a view named "CityPhoneBook" from the PhoneBook table:

CREATE VIEW CityPhoneBook AS
     SELECT Name FROM PhoneBook WHERE City\='Boston'

The following example creates a view named "GuideHistory" from the Guides table. It lists all titles (from the Title column) and whether or not the person is retired:

CREATE VIEW GuideHistory AS
     SELECT Guides, Title, Retired, Date\_Retired 
     FROM Guides

The following Embedded SQL example creates the table MyTest, and then creates a view for this table, MyTestView, which selects one field from MyTest:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  &sql(DROP TABLE Sample.MyTest)
  &sql(DROP VIEW Sample.MyTestView)
CreateTable
  &sql(CREATE TABLE Sample.MyTest (
     TestNum     INT NOT NULL,
     FirstWord   CHAR (30) NOT NULL,
     LastWord    CHAR (30) NOT NULL,
     CONSTRAINT MyTestPK PRIMARY KEY (TestNum))
  )
  IF SQLCODE\=0 { WRITE !,"Created table" }
  ELSE { WRITE "CREATE TABLE error SQLCODE=",SQLCODE }
CreateView
  &sql(CREATE VIEW Sample.MyTestView AS
     SELECT FirstWord FROM Sample.MyTest
     WITH CASCADED CHECK OPTION)
  IF SQLCODE\=0 { WRITE !,"Created view" }
  ELSE { WRITE "CREATE VIEW error SQLCODE=",SQLCODE }

 

The following Embedded SQL example creates a view MyTestView, which selects two fields from MyTest. The SELECT query for this view contains a TOP clause and an ORDER BY clause:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  &sql(DROP TABLE Sample.MyTest)
  &sql(DROP VIEW Sample.MyTestView)
CreateTable
  &sql(CREATE TABLE Sample.MyTest (
     TestNum     INT NOT NULL,
     FirstWord   CHAR (30) NOT NULL,
     LastWord    CHAR (30) NOT NULL,
     CONSTRAINT MyTestPK PRIMARY KEY (TestNum))
  )
  IF SQLCODE\=0 { WRITE !,"Created table" }
  ELSE { WRITE "CREATE TABLE error SQLCODE=",SQLCODE }
CreateView
  &sql(CREATE VIEW Sample.MyTestView AS
     SELECT TOP ALL FirstWord,LastWord FROM Sample.MyTest
     ORDER BY LastWord)
  IF SQLCODE\=0 { WRITE !,"Created view" }
  ELSE { WRITE "CREATE VIEW error SQLCODE=",SQLCODE }

 

The following example creates a view named "StaffWorksDesign" from three tables (Proj, Staff, and Works). The columns Name, Cost, and Project provide the data.

CREATE VIEW StaffWorksDesign (Name,Cost,Project)
     AS SELECT EmpName,Hours\*2\*Grade,PName
     FROM Proj,Staff,Works 
     WHERE Staff.EmpNum\=Works.EmpNum 
          AND Works.PNum\=Proj.PNum AND PType\='Design'

The following example creates a view named “v\_3” by selecting from b.table2 and a.table1 using a UNION:

CREATE VIEW v\_3(fvarchar)
     AS SELECT DISTINCT \* 
     FROM
       (SELECT fVARCHAR2 FROM b.table2 
        UNION ALL
        SELECT fVARCHAR1 FROM a.table1) 

See Also

*   [ALTER VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alterview)
    
*   [DROP VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropview)
    
*   [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable)
    
*   [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant)
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   “[Defining and Using Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views)” chapter in Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createview.xml**