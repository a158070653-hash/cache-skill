# CREATE TRIGGER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createtrigger

---

 

Caché SQL Reference

CREATE TRIGGER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger "CREATE TRIGGER") \]

Go to: Description Arguments SQL Trigger Code ObjectScript Trigger Code Listing Existing Triggers Trigger Runtime Errors Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a trigger.

Synopsis

CREATE TRIGGER trigname {BEFORE | AFTER} event
        \[ORDER integer\] 
        ON table 
        \[REFERENCING {OLD | NEW} \[ROW\] \[AS\] alias\] 
        action

Arguments

trigname

The name of the trigger to be created, which is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). A trigger name may be qualified or unqualified; if qualified, its schema name must match the table’s schema name. For further details see the “Identifiers” chapter of Using Caché SQL.

BEFORE event

AFTER event

The time (BEFORE or AFTER) and type of trigger event. Available event options are INSERT, DELETE, UPDATE, and UPDATE OF. The UPDATE OF clause is followed by a column name or a comma-separated list of column names. The UPDATE OF clause can only be specified when LANGUAGE is SQL.

ORDER integer

Optional — The order in which triggers should be executed when there are multiple triggers for a table with the same time and event. If order is omitted, a trigger is assigned an order of 0.

ON table

The table the trigger is created for. A table name may be qualified or unqualified; if qualified, the trigger must reside in the same schema as the table.

REFERENCING OLD ROW AS alias

REFERENCING NEW ROW AS alias

Optional — A REFERENCING clause can only be used when LANGUAGE is SQL. A REFERENCING clause allows you to specify an alias that you can use to reference a column. REFERENCING OLD ROW allows you reference the old value of a column during an UPDATE or DELETE trigger. REFERENCING NEW ROW allows you to reference the new value of a column during an INSERT or UPDATE trigger. The ROW AS keywords are optional. For an UPDATE, you can specify both OLD and NEW in the same REFERENCING clause, as follows: REFERENCING OLD oldalias NEW newalias.

action

The program code for the trigger. The action argument can contain various optional keyword clauses, including (in order): a FOR EACH ROW clause; a WHEN clause with a predicate condition governing execution of the triggered action; and a LANGUAGE clause which specifies either LANGUAGE SQL or LANGUAGE OBJECTSCRIPT. If the LANGUAGE clause is omitted, SQL is the default. Following these clauses, you specify one or more lines of code specifying the action to perform when the trigger is executed.

Description

The CREATE TRIGGER command defines a trigger, a block of code to be executed when data in a specific table is modified. A trigger is executed (“fired”) when a specific triggering event occurs, such as a new row being inserted into a specified table. A triggering event may be an INSERT, DELETE, or UPDATE command. A trigger executes user-specified trigger code. You can specify that the trigger should execute this code before or after the execution of the triggering event. A trigger is specific to a specified table.

A single-event trigger is triggered by a specified INSERT, DELETE, or UPDATE operation. A multiple-event trigger is defined to execute when any one of the specified events occurs on the specified table. You can define an INSERT/UPDATE, an UPDATE/DELETE, or an INSERT/UPDATE/DELETE multiple-event trigger.

Trigger action code cannot modify data in the triggering record. For example, if an update to the data in a table column fires a trigger, that trigger’s code block cannot insert, update, or delete data in any of the records of that table.

Privileges and Locking

The CREATE TRIGGER command is a privileged operation. Before using CREATE TRIGGER it is necessary for your process to have %CREATE\_TRIGGER privileges. Failing to do so results in an SQLCODE -99 error (Privilege Violation). You can use the GRANT command to assign %CREATE\_TRIGGER privileges, if you hold appropriate granting privileges.

In embedded SQL, you can use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to log in as a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

CREATE TRIGGER cannot be used on a [table created by defining a persistent class](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_via_classes), unless the table class definition includes \[[DdlAllowed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_ddlallowed)\]. Otherwise, the operation fails with an SQLCODE -300 error with the %msg DDL not enabled for class 'Schema.tablename'.

The CREATE TRIGGER statement acquires a table-level lock on table. This prevents other processes from modifying the table’s data. This lock is automatically released at the conclusion of the CREATE TRIGGER operation.

Other Ways of Defining Triggers

You can define an SQL trigger as a Caché Object by including the [FOREACH = ROW/OBJECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_trigger_foreach) statement. The following is an example of an Object trigger:

Trigger SQLJournal \[ CodeMode \= objectgenerator, Event \= INSERT/UPDATE, ForEach \= ROW/OBJECT, Time \= AFTER \]
{  /\* ObjectScript trigger code
      that updates a jounal file
      after a row is inserted or updated. \*/
}

Note:

Caché SQL triggers are wholly separate from Caché class methods such as %OnBeforeSave(), %OnAfterSave(), and [%AddToSaveSet()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.RegisteredObject#%AddToSaveSet) that may be invoked when a %Save() method is issued, and both of these are wholly separate from [Caché MultiValue triggers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RVCL_commands#RVCL_commands_createtrigger). A data modification operation in one realm can only fire the triggers defined for that realm. For example, a Caché SQL data modification operation cannot fire a MultiValue trigger. A Caché MultiValue data modification operation cannot fire a Caché SQL trigger.

Arguments

trigname

A trigger name follows the same [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) requirements as a table name, but not the same uniqueness requirements. A trigger name must be unique for a table within a schema. Thus, triggers referencing different tables in a schema may have the same name. A trigger and its associated table must reside in the same schema. You cannot use the same name for a trigger and a table in the same schema. Violating trigger naming conventions results in an SQLCODE -400 error at CREATE TRIGGER execution time.

A trigger name may be unqualified or qualified. A qualified trigger name has the form:

schema.trigger

If the trigger name is unqualified, the trigger schema name defaults to the same schema as the table. If both are unqualified, the system default schema is assumed. If the trigger name is qualified, the trigger schema name must be the same as the table schema name. If no table schema name is specified, the trigger schema name is assumed. A schema name mismatch results in an SQLCODE -366 error; this should only occur when both the trigger name and the table name are qualified, and they specify different schema names.

To return the default schema name for the current process, use the [$SYSTEM.SQL.DefaultSchema()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DefaultSchema) method; to configure the default schema name, use the [$SYSTEM.SQL.SetDefaultSchema()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDefaultSchema) method.

Trigger names follow [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) conventions, subject to the restrictions below. By default, trigger names are simple identifiers. A trigger name should not exceed 128 characters. Trigger names are not case-sensitive.

Caché uses trigname to generate a corresponding trigger name in the Caché class. The corresponding class trigger name contains only alphanumeric characters (letters and numbers) and is a maximum of 96 characters in length. To generate this Caché identifier name, Caché first strips punctuation characters from the trigger name, and then generates a unique identifier of 96 (or less) characters, substituting a number for the 96th character when needed to create a unique name. This name generation imposes the following restrictions on the naming of triggers:

*   A trigger name must include at least one letter. Either the first character of the trigger name or the first character after initial punctuation characters must be a letter.
    
*   Caché supports 16-bit (wide) characters for trigger names on Unicode systems. A character is a valid letter if it passes the [$ZNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname) test.
    
*   Because names generated for a Caché class do not include punctuation characters, it is not advisable (though possible) to create trigger names that differ only in their punctuation characters.
    
*   A trigger name may be much longer than 96 characters, but trigger names that differ in their first 96 alphanumeric characters are much easier to work with.
    

event

A trigger specified as INSERT is executed when a row is inserted into the specified table. A trigger specified as DELETE is executed when a row is deleted from the specified table. A trigger specified as UPDATE is executed when a row is updated in the specified table.

A trigger specified as UPDATE OF is executed only when one or more of the specified columns is updated in a row in the specified table. Column names are specified as a comma-separated list. Column names can be specified in any order, but duplicate column names are not permitted; this results in an SQLCODE -58 error at compile time. The UPDATE OF clause is only valid if the trigger code LANGUAGE is SQL (the default).

The time that the trigger is fired is specified by the BEFORE or AFTER keyword; these keywords specify that the trigger operation should occur either before or after Caché executes the triggering event. A BEFORE trigger is executed before performing the specified event, but after verifying the event. For example, Caché only executes a BEFORE DELETE trigger if the DELETE statement is valid for the specified row(s), and the process has the necessary privileges to perform the DELETE, including any foreign key referential integrity checks. If the process cannot perform the specified event, Caché issues an error code for the event; it does not execute the BEFORE trigger.

The following are examples of four of the eight possible event types:

CREATE TRIGGER TrigBI BEFORE INSERT ON Sample.Person
       INSERT INTO TLog VALUES ('before insert');

CREATE TRIGGER TrigAU AFTER UPDATE ON Sample.Person
       INSERT INTO TLog VALUES ('after update');

CREATE TRIGGER TrigBUOF BEFORE UPDATE OF Home\_City,Home\_State ON Sample.Person
       INSERT INTO TLog VALUES ('before address update');

CREATE TRIGGER TrigAD AFTER DELETE ON Sample.Person
       INSERT INTO TLog VALUES ('after delete');

ORDER

The ORDER clause determines the order in which triggers are executed when there are multiple triggers for the same table with the same time and event. For example, two AFTER DELETE triggers. The trigger with the lowest ORDER integer is executed first, then the next higher integer, and so on. If the ORDER clause is not specified, a trigger is created with an assigned ORDER number of 0 (zero). Thus, triggers with no ORDER clause are always executed before triggers with ORDER clauses.

You can assign the same order value to multiple triggers. You can also create multiple triggers with an (implicit or explicit) order of 0. Multiple triggers with the same time, event, and order are executed together in random order.

Triggers are executed in the sequence: time > order > event. Thus if you have a BEFORE INSERT trigger and a BEFORE INSERT/UPDATE trigger, the trigger with the lowest ORDER value would be executed first. If you have a BEFORE INSERT trigger and a BEFORE INSERT/UPDATE trigger with the same ORDER value, the INSERT is executed before the INSERT/UPDATE. This is because — time and order being the same — a single-event trigger is always executed before a multi-event trigger. If two (or more) triggers have identical time, order, and event values, the order of execution is random.

The following examples show how ORDER numbers work. All of these CREATE TRIGGER statements create triggers that are executed by the same event:

CREATE TRIGGER TrigA BEFORE DELETE ON doctable
       INSERT INTO TLog VALUES ('doc deleted');
  \-- Assigned ORDER=0

CREATE TRIGGER TrigB BEFORE DELETE ORDER 4 ON doctable
       INSERT INTO TReport VALUES ('doc deleted')
  \-- Specified as ORDER=4

CREATE TRIGGER TrigC BEFORE DELETE ORDER 2 ON doctable
       INSERT INTO Ttemps VALUES ('doc deleted')
  \-- Specified as ORDER=2

CREATE TRIGGER TrigD BEFORE DELETE ON doctable
       INSERT INTO Tflags VALUES ('doc deleted')
  \-- Also assigned ORDER=0

These triggers will execute in the sequence: (TrigA, TrigD), TrigC, TrigB. Note that TrigA and TrigD have the same order number, and thus execute in random sequence.

REFERENCING

The REFERENCING clause can specify an alias for the old value of a row, the new value of a row, or both. The old value is the row value before the triggered action of an UPDATE or DELETE trigger. The new value is the row value after the triggered action of an UPDATE or INSERT trigger. For an UPDATE trigger, you can specify aliases for both the before and after row values, as follows:

REFERENCING OLD ROW AS oldalias NEW ROW AS newalias

The keywords ROW and AS are optional. Therefore, the same clause can also be specified as:

REFERENCING OLD oldalias NEW newalias

It is not meaningful to refer to an OLD value before an INSERT or a NEW value after a DELETE. Attempting to do so results in an SQLCODE -48 error at compile time.

A REFERENCING clause can only be used when the action program code is SQL. Specifying a REFERENCING clause with the LANGUAGE OBJECTSCRIPT clause results in an SQLCODE -49 error.

action

A triggered action consists of the following elements:

*   An optional FOR EACH clause. The available values are FOR EACH ROW, FOR EACH ROW\_AND\_OBJECT, and FOR EACH STATEMENT. FOR EACH ROW means that this trigger is invoked by an SQL filing operation. FOR EACH ROW\_AND\_OBJECT means that this trigger is invoked by either an SQL filing operation or a Caché Objects filing operation. FOR EACH STATEMENT is provided for compatibility with [TSQL triggers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GTSQ_commands).
    
*   An optional WHEN clause, consisting of the WHEN keyword followed by a predicate condition (simple or complex) enclosed in parentheses. If the predicate condition evaluates to TRUE, the trigger is executed. A WHEN clause can only be used when LANGUAGE is SQL. The WHEN clause can reference oldalias or newalias values. For further details on predicate condition expressions and a list of available predicates, refer to the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page in this document.
    
*   An optional LANGUAGE clause, either LANGUAGE SQL or LANGUAGE OBJECTSCRIPT. The default is LANGUAGE SQL.
    
*   User-written code that is executed when the trigger is executed.
    

SQL Trigger Code

If LANGUAGE SQL (the default), the triggered statement is an SQL procedure block, consisting of either one SQL procedure statement followed by a semicolon, or the keyword BEGIN followed by one or more SQL procedure statements, each followed by a semicolon, concluding with an END keyword.

A triggered action is atomic, it is either fully applied or not at all, and cannot contain COMMIT or ROLLBACK statements. The keyword BEGIN ATOMIC is synonymous with the keyword BEGIN.

If LANGUAGE SQL, the CREATE TRIGGER statement can optionally contain a REFERENCING clause, a WHEN clause, and/or an UPDATE OF clause. An UPDATE OF clause specifies that the trigger should only be executed when an UPDATE is performed on one or more of the columns specified for this trigger. A CREATE TRIGGER statement with LANGUAGE OBJECTSCRIPT cannot contain these clauses.

SQL trigger code is executed as embedded SQL. This means that Caché converts SQL trigger code to Caché ObjectScript; therefore, if you use Studio to view the class definition corresponding to your SQL trigger code, the trigger definition in Studio will show Language Mode as Caché.

When executing SQL trigger code, Caché automatically resets (NEWs) all variable used in the trigger code. After the execution of each SQL statement, Caché checks SQLCODE. If an error occurs, Caché sets the %ok variable to 0, aborting and rolling back both the trigger code operation(s) and the associated INSERT, UPDATE, or DELETE.

ObjectScript Trigger Code

If LANGUAGE OBJECTSCRIPT, the CREATE TRIGGER statement cannot contain a REFERENCING clause, a WHEN clause, or an UPDATE OF clause. Specifying these SQL-only clauses with LANGUAGE OBJECTSCRIPT results in compile-time SQLCODE errors -49, -57, or -50, respectively.

If LANGUAGE OBJECTSCRIPT, the triggered statement is a block of one or more Caché ObjectScript statements, enclosed by curly braces.

Because the code for a trigger is not generated as a procedure, all local variables in a trigger are public variables. This means all variables in triggers should be explicitly declared with a NEW statement; this protects them from conflicting with variables in the code that invokes the trigger.

If trigger code contains Caché [Macro Preprocessor statements](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros) (# commands, ## functions, or $$$macro references) these statements are compiled before the CREATE TRIGGER DDL code itself.

You can issue an error from trigger code by setting the %ok variable to 0. This creates a runtime error that aborts execution of the trigger. Trigger code can also set the %msg variable to a string describing the cause of the runtime error.

As of version 2012.1, Caché generates trigger code only once, even for a multiple-event trigger.

Field References and Pseudo-field References

Trigger code written in Caché ObjectScript can contain field references, specified as {fieldname}, where fieldname specifies an existing field in the current table. No blank spaces are permitted within the curly braces.

You can follow the fieldname with \*N (new), \*O (old), or \*C (compare) to specify how to handle an inserted, updated, or deleted field data value, as follows:

*   {fieldname\*N}
    
    *   For UPDATE, returns the new field value after the specified change is made.
        
    *   For INSERT, returns the value inserted.
        
    *   For DELETE, returns the value of the field before the delete.
        
*   {fieldname\*O}
    
    *   For UPDATE, returns the old field value before the specified change is made.
        
    *   For INSERT, returns NULL.
        
    *   For DELETE, returns the value of the field before the delete.
        
*   {fieldname\*C}
    
    *   For UPDATE, returns 1 (TRUE) if the new value differs from the old value, otherwise returns 0 (FALSE).
        
    *   For INSERT, returns 1 (TRUE) if the inserted value is non-NULL, otherwise returns 0 (FALSE).
        
    *   For DELETE, returns 1 (TRUE) if the value being deleted is non-NULL, otherwise returns 0 (FALSE).
        

For UPDATE, INSERT, or DELETE, {fieldname} returns the same value as {fieldname\*N}.

Line returns are not permitted within a statement that sets a field value. For further details, refer to the [SqlComputeCode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputecode) property keyword in the Caché Class Definition Reference.

You can use the [GetColumns()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetColumns) method to list the field names defined for a table. For further details, refer to [Column Names and Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_getcols) in the “Defining Tables” chapter of Using Caché SQL.

Trigger code written in Caché ObjectScript can also contain the pseudo-field reference variables {%%CLASSNAME}, {%%CLASSNAMEQ}, {%%OPERATION}, {%%TABLENAME}, and {%%ID}. The pseudo-fields are translated into a specific value at class compilation time. All of these pseudo-field keywords are not case-sensitive.

*   {%%CLASSNAME} and {%%CLASSNAMEQ} both translate to the name of the class which projected the SQL table definition. {%%CLASSNAME} returns an unquoted string and {%%CLASSNAMEQ} returns a quoted string.
    
*   {%%OPERATION} translates to a string literal, either INSERT, UPDATE, or DELETE, depending on the operation that invoked the trigger.
    
*   {%%TABLENAME} translates to the [fully qualified name of the table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_names), returned as a quoted string.
    
*   {%%ID} translates to the [RowID name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_idfield). This reference is useful when you do not know the name of the RowID field.
    

Referencing Stream Property

When a [Stream field/property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs) is referenced in a trigger definition, like {StreamField}, {StreamField\*O}, or {StreamField\*N}, the value of the {StreamField} reference is the stream's OID (object ID) value.

For a BEFORE INSERT or BEFORE UPDATE trigger, if a new value is specified by the INSERT/UPDATE/ObjectSave, the {StreamField\*N} value will be either the OID of the temporary stream object, or the new literal stream value. For a BEFORE UPDATE trigger, if a new value is not specified for the stream field/property, {StreamField\*O} and {StreamField\*N} will both be the OID of the current field/property stream object.

Referencing SQLComputed Property

When a transient SqlComputed field/property (either "Calculated" or explicitly "Transient") is referenced in a trigger definition, Get()/Set() method overrides are not recognized by the trigger. Use [SQLCOMPUTED/SQLCOMPUTONCHANGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_compute), rather than overriding the property's Get() or Set() method.

Using Get()/Set() method overrides can result in the following erroneous result: The {property\*O} value is determined using SQL and does not use the overridden Get()/Set() methods. Because the property is not stored on disk, {property\*O} uses the SqlComputeCode to "recreate" the old value. However, {property\*N} uses the overridden Get()/Set() methods to access the property's value. As a result, there is a possibility for {property\*O} and {property\*N} to be different (and thus {property\*C}=1) even though the property did not actually change.

Labels

Trigger code may contain [line labels](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) (tags). To specify a label in trigger code, prefix the label line with a colon to indicate that this line should begin in the first column. Caché strips out the colon and treats the remaining line as a label. However, because trigger code is generated outside the scope of any procedure blocks, every label must be unique throughout the class definition. Any other code compiled into the class's routine must not have the same label defined, including in other triggers, in non-procedure block methods, [SqlComputeCode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputecode), and other code.

Note:

This use of a colon prefix for a label takes precedence over the use of a colon prefix for a [host variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) reference. To avoid this conflict, it is recommended that embedded SQL trigger code lines never begin with a host variable reference. If you must begin a trigger code line with a host variable reference, you can designate it as a host variable (and not a label) by doubling the colon prefix.

Method Calls

You can call class methods from within trigger code, because class methods do not depend on having an open object. You must use the ##class(classname).Method() syntax to invoke a method. You cannot use the [..Method()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos) syntax, because this syntax requires a current open object.

You can pass the value of a field of the current row as an argument of the class method, but the class method itself cannot use field syntax.

Listing Existing Triggers

You can use the [INFORMATION.SCHEMA.TRIGGERS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=INFORMATION.SCHEMA.TRIGGERS) class to list the currently defined triggers. This class lists for each trigger the name of the trigger, the associated schema and table name, and the trigger creation timestamp. For each trigger it lists the EVENT\_MANIPULATION property (INSERT, UPDATE, DELETE, INSERT/UPDATE, INSERT/UPDATE/DELETE) and ACTION\_TIMING property (BEFORE, AFTER). It also lists the ACTION\_STATEMENT, which is the generated SQL trigger code.

Trigger Runtime Errors

A trigger and its invoking event execute as an atomic operation on a single row basis. That is:

*   A failed BEFORE trigger is rolled back, the associated INSERT, UPDATE, or DELETE operation is not executed, and all locks on the row are released.
    
*   A failed AFTER trigger is rolled back, the associated INSERT, UPDATE, or DELETE operation is rolled back, and all locks on the row are released.
    
*   A failed INSERT, UPDATE, or DELETE operation is rolled back, the associated BEFORE trigger is rolled back, and all locks on the row are released.
    
*   A failed INSERT, UPDATE, or DELETE operation is rolled back, the associated AFTER trigger is not executed, and all locks on the row are released.
    

Note that integrity is maintained for the current row operation only. Your application program must handle data integrity issues involving operation on multiple rows by using transaction processing statements.

Because a trigger is an atomic operation, you cannot code transaction statements, such as commits and rollbacks, within trigger code.

If an INSERT, UPDATE, or DELETE operation causes multiple triggers to execute, the failure of one trigger causes all remaining triggers to remain unexecuted.

When a database operation fails because of a fatal runtime error, Caché issues an SQLCODE -415 error. When a trigger operation fails, Caché issues one of the SQLCODE error codes -130 through -135 indicating the type of trigger that failed. You can force a trigger to fail by setting the %ok variable to 0 in the trigger code. You can also set the %msg variable to a string containing a message to be returned upon trigger failure.

Examples

The following two Embedded SQL programs demonstrate CREATE TRIGGER with an ObjectScript INSERT trigger. The first example creates a table and an INSERT trigger for that table. The second program issues an INSERT against the table, executing the trigger which writes a message. It then drops the table so that this program can be run repeatedly:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  &sql(CREATE TABLE TestDummy (
     testnum     INT NOT NULL,
     firstword   CHAR (30) NOT NULL,
     lastword    CHAR (30) NOT NULL,
     CONSTRAINT TestDummyPK PRIMARY KEY (testnum))
  )
  WRITE !,"SQL table code is: ",SQLCODE
  &sql(CREATE TRIGGER TrigTestDummy AFTER INSERT ON TestDummy
      LANGUAGE OBJECTSCRIPT
    {WRITE "I just fired the trigger" }
  )
  WRITE !,"SQL trigger code is: ",SQLCODE

 

  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(INSERT INTO TestDummy (testnum,firstword,lastword) VALUES 
   (46639,'hello','goodbye'))
  IF SQLCODE\=0 {
    WRITE !,"Insert succeeded"
    WRITE !,"Row count=",%ROWCOUNT
    WRITE !,"Row ID=",%ROWID }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",SQLCODE }
  &sql(DROP TABLE TestDummy)

 

The following examples demonstrate CREATE TRIGGER with an SQL INSERT trigger. The first embedded SQL program creates a table, an INSERT trigger for that table, and a log table for the trigger's use. The second embedded SQL program issues an INSERT against the table, which invokes the trigger, which logs an entry in the log table. After displaying the log entry, the program drops both tables so that this program can be run repeatedly:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  &sql(CREATE TABLE TestDummy (
     testnum     INT NOT NULL,
     firstword   CHAR (30) NOT NULL,
     lastword    CHAR (30) NOT NULL,
     CONSTRAINT TestDummyPK PRIMARY KEY (testnum))
  )
  WRITE !,"SQL table code is: ",SQLCODE
  &sql(CREATE TABLE TestDummyLog (
     entry CHAR (60) NOT NULL)
  )
  WRITE !,"SQL log table code is: ",SQLCODE
  &sql(CREATE TRIGGER TrigTestDummy AFTER INSERT ON TestDummy
      LANGUAGE SQL
   BEGIN
     INSERT INTO TestDummyLog (entry) VALUES 
     (CURRENT\_TIMESTAMP||' INSERT to TestDummy');
   END )
  WRITE !,"SQL trigger code is: ",SQLCODE

 

  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(INSERT INTO TestDummy (testnum,firstword,lastword) VALUES 
   (46639,'hello','goodbye'))
  IF SQLCODE\=0 {
    WRITE !,"Insert succeeded"
    WRITE !,"Row count=",%ROWCOUNT
    WRITE !,"Row ID=",%ROWID }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",SQLCODE }
  &sql(SELECT entry INTO :logitem FROM TestDummyLog)
    WRITE !,"Log entry: ",logitem
  &sql(DROP TABLE TestDummy)
  &sql(DROP TABLE TestDummyLog)
    WRITE !,"finished!"

 

The following example includes a WHEN clause that specifies that the action should only be performed when the predicate condition in parentheses is met:

CREATE TRIGGER Trigger\_2 AFTER INSERT ON Table\_1
  WHEN (f1 %STARTSWITH 'A')
  BEGIN
    INSERT INTO Log\_Table VALUES (new\_row.Category);
  END

See Also

*   [DROP TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptrigger)
    
*   [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant)
    
*   “[Using Triggers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_triggers)” chapter in Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createtrigger.xml**