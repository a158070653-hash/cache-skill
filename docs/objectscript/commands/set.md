# SET

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cset

---

 

Caché ObjectScript Reference

SET

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset "SET") \]

Go to: Description Arguments JSON Object and JSON Array Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Assigns a value to a variable.

Synopsis

SET:pc setargument,... 
S:pc setargument,...

where setargument can be: 
   variable=value 
   (variable-list)=value

Arguments

pc

Optional — A postconditional expression.

variable

The variable to set to the corresponding value. variable can be a local variable, a process-private global, a global, an object property, or special variable. (Not all special variables can be set by an application; see documentation of individual special variables.)

variable-list

A comma-separated list, enclosed in parentheses, that consists of one or more variable arguments. All of the variable arguments in variable-list are assigned the same value.

value

A literal value, or any valid Caché ObjectScript expression that evaluates to a value. Can be a JSON object or JSON array. (OpenVMS does not support JSON.)

Description

The SET command assigns a value to a variable. It can set a single variable, or set multiple variables using any combination of two syntactic forms. It can assign values to variables by specifying a comma-separated list of variable=value pairs. For example:

   SET a\=1,b\=2,c\=3
   WRITE a,b,c

 

There is no restriction in the number of assignments you can perform with a single invocation of SET a=value,b=value,c=value,....

If a specified variable does not exist, SET creates it and assigns the value. If a specified variable exists, SET replaces the previous value with the specified value. Because SET executes in left-to-right order, you can assign a value to a variable, then assign that variable to another variable:

   SET a\=1,b\=a
   WRITE a,b

 

A value can be a string, a numeric, a JSON object, a JSON array, or an expression that evaluates to one of these values. To define an “empty” variable, you can set the variable to the empty string ("") value.

Setting Multiple Variables to the Same Value

You can use SET to assign the same value to multiple variables by specifying a comma-separated list of variables enclosed in parentheses. For example:

   SET (a,b,c)\=1
   WRITE a,b,c

 

You can combine the two SET syntactic forms in any combination. For example:

   SET (a,b)\=1,c\=2,(d,e,f)\=3
   WRITE a,b,c,d,e,f

 

The maximum number of assignments you can perform with a single invocation of SET (a,b,c,...)=value is 128. Exceeding this number results in a <SYNTAX> error.

You cannot use SET (a,b,c,...)=value syntax to assign a value to a function that uses relative offset syntax: the [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract), [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece), or [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function. In relative offset syntax, an asterisk represents the end of a string, and \*-n and \*+n represent a relative offset from the end of the string. You must use SET a=value,b=value,c=value,... syntax when setting one of these functions using relative offset.

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

variable

The variable to receive the value resulting from the evaluation of value. It can be a [local variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local), a [process-private global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls), a [global variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global), or a [special variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special). A local variable, process-private global, or global variable can be either subscripted or unsubscripted. A global variable can be specified with extended global reference (see [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals).

Local variables, process-private globals, and special variables are specific to the current process; they are mapped to be accessible from all namespaces. A global variable persists after the process that created it terminates.

A global is specific to the namespace in which it was created. By default, a SET assigns a global in the current namespace. You can use SET to define a global (^myglobal) in another namespace by using syntax such as the following: SET ^\["Samples"\]myglobal="Ansel Adams". If you specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error, followed by the global name and database path, such as the following: <PROTECT> ^myglobal,c:\\intersystems\\cache\\mgr\\.

A variable can be a piece or segment of a variable as specified in the argument of a $PIECE or $EXTRACT function.

A variable can be represented as an object property using obj.property or [.. property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos) syntax, or by using the [$PROPERTY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty) function. You can set an [i%property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos#GOBJ_specialcos_ipcpropname) instance property reference using the following syntax:

  SET i%propname \= "abc"

SET accepts a variable name of any length, but it truncates a long variable name to 31 characters before assigning it a value. If a variable name is not unique within the first 31 characters this name truncation can cause unintended overwriting of variable values, as shown in the following example:

  SET abcdefghijklmnopqrstuvwxyz2abc\="30 characters"
  SET abcdefghijklmnopqrstuvwxyz2abcd\="31 characters"
  SET abcdefghijklmnopqrstuvwxyz2abcde\="32 characters"
  SET abcdefghijklmnopqrstuvwxyz2abcdef\="33 characters"
  WRITE !,abcdefghijklmnopqrstuvwxyz2abc     // returns "30 characters"
  WRITE !,abcdefghijklmnopqrstuvwxyz2abcd    // returns "33 characters"
  WRITE !,abcdefghijklmnopqrstuvwxyz2abcde   // returns "33 characters"
  WRITE !,abcdefghijklmnopqrstuvwxyz2abcdef  // returns "33 characters"

 

Special variables are, by definition, set by system events. You can use SET to assign a value to certain special variables. However, most special variables cannot be assigned a value using SET. See the reference pages for individual special variables for further details.

Refer to the “[Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables)” chapter of Using Caché ObjectScript for further details on variable types and naming conventions.

value

A literal value or any valid Caché ObjectScript expression. Usually a value is a numeric or string expression. A value can be a [JSON object or JSON array](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset#RCOS_cset_json).

*   A numeric value is converted to canonical form before assignment: leading and trailing zeros, a plus sign or a trailing decimal point are removed. Conversion from scientific notation and evaluation of arithmetic operations are performed.
    
*   A string value is enclosed in quotation marks. A string is assigned unchanged, except that doubled quotation marks within the string are converted to a single quotation mark. The null string ("") is a valid value.
    
*   A numeric value enclosed in quotation marks is not converted to canonical form and no arithmetic operations are performed before assignment.
    
*   If a relational or logical expression is used, Caché assigns the truth value (0 or 1) resulting from the expression.
    
*   Object properties and object methods that return a value are valid expressions. Use the [relative dot syntax (..)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos) for assigning a property or method value to a variable.
    

JSON Object and JSON Array

You can use the SET command to set a variable to a JSON object or a JSON array. For a JSON object, the value is a JSON object delimited by curly braces. For a JSON array, the value is a JSON array delimited by square brackets.

Within these delimiters, the literal values are JSON literals, not Caché literals. For string literals this means that a literal double-quote character within a string must be specified using the JSON escape sequence: either \\" or \\u0022. For numeric literals, JSON validation rules are stricter than Caché validation rules: a leading plus sign is not permitted; leading zeros are not permitted; however, a decimal separator must have a digit character on both sides of it. Therefore, the JSON numerics 0, 0.0, 0.4, and 0.400 are valid. An invalid JSON literal generates a <SYNTAX> error.

To include a Caché literal or expression within a JSON object or a JSON array, you must enclose it in parentheses. In the following example, a JSON string literal and a Caché string literal are specified in a JSON array:

  SET jarray\=\["This is a \\"good\\" JSON string",("This is a ""good"" Cache string")\]
  WRITE jarray.%ToJSON()

 

JSON Object

A value can be a JSON object delimited by curly braces. The variable is set to an oref, such as the following: 3@%Library.DynamicObject. This oref can be resolved to the JSON object value using the %ToJSON() function. This is shown in the following example:

  SET jobj\={"name":"Fred"}
  WRITE "JSON object reference = ",jobj,!
  WRITE "JSON object value = ",jobj.%ToJSON()

 

A valid JSON object has the following format:

*   Begins with an open curly brace, ends with a close curly brace. The empty object {} is a valid JSON object.
    
*   Within the curly braces, a key:value pair or a comma-separated list of key:value pairs. Both the key and the value components are JSON literals, not Caché literals.
    
*   The key component must be a JSON quoted string literal. It cannot be a Caché literal or expression enclosed in parentheses.
    
*   The value component can be a JSON string or a JSON numeric literal. These JSON literals follow JSON validation criteria. A value component can be a Caché literal or expression enclosed in parentheses. The value component can be specified as a defined variable specifying a string, a numeric, a JSON object, or a JSON array. The value component can contain nested JSON objects or JSON arrays. A value component can also be one of the following three JSON special values: true, false, null, specified as an unquoted literal in lowercase letters; these JSON special values cannot be specified using a variable.
    

The following are all valid JSON objects: {"name":"Fred"}, {"name":"Fred","city":"Bedrock"}, {"bool":true}, {"1":true,"0":false,"Else":null}, {"name":{"fname":"Fred","lname":"Flintstone"},"city":"Bedrock"}, {"name":\["Fred","Wilma","Barney"\],"city":"Bedrock"}.

A JSON object can specify a null property name and assign it a value, as shown in the following example:

  SET jobj\={}
  SET jobj.""\="This is the ""null"" property value"
  WRITE "JSON null property object value = ",jobj.%ToJSON()

 

Note that the returned JSON string uses the JSON escape sequence (\\") for a literal double quote character.

For further details, refer to “Creating a Dynamic Object” in [Using JSON in Caché](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GJSON).

JSON Array

A value can be a JSON array delimited by square brackets. The variable is set to an oref, such as the following: 1@%Library.DynamicArray. This oref can be resolved to the JSON array value using the %ToJSON() function. This is shown in the following example:

  SET jary\=\["Fred","Wilma","Barney"\]
  WRITE "JSON array reference = ",jary,!
  WRITE "JSON array value = ",jary.%ToJSON()

 

A valid JSON array has the following format:

*   Begins with an open square bracket, ends with a close square bracket. The empty array \[\] is a valid JSON array.
    
*   Within the square brackets, an element or a comma-separated list of elements. Each array element can be a JSON string or JSON numeric literal. These JSON literals follow JSON validation criteria. An array element can be a Caché literal or expression enclosed in parentheses. An array element can be specified as a defined variable specifying a string, a numeric, a JSON object, or a JSON array. An array element can contain one or more JSON objects or JSON arrays. An array element can also be one of the following three JSON special values: true, false, null, specified as an unquoted literal in lowercase letters; these JSON special values cannot be specified using a variable.
    

The following are all valid JSON arrays: \[1\], \[5,7,11,13,17\], \["Fred","Wilma","Barney"\], \[true,false\], \["Bedrock",\["Fred","Wilma","Barney"\]\], \[{"name":"Fred"},{"name":"Wilma"}\], \[{"name":"Fred","city":"Bedrock"},{"name":"Wilma","city":"Bedrock"}\], \[{"names":\["Fred","Wilma","Barney"\]}\].

For further details, refer to “Creating a Dynamic Array” in [Using JSON in Caché](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GJSON).

Examples

The following example specifies multiple arguments for the same SET command. Specifically, the command assigns values to three variables. Note that arguments are evaluated in left-to-right order.

   SET var1\=12,var2\=var1\*3,var3\=var1+var2
   WRITE "var1=",var1,!,"var2=",var2,!,"var3=",var3

 

The following example shows the (variable-list)=value form of the SET command. It shows how to assign the same value to multiple variables. Specifically, the command assigns the value 0 to three variables.

   SET (sum,count,average)\=0
   WRITE "sum=",sum,!,"count=",count,!,"average=",average

 

The following example sets a subscripted global variable in a different namespace using extended global reference.

   ZNSPACE "user"
   SET ^\["samples"\]name(1)\="fred"
   ZNSPACE "samples"
   WRITE ^name(1)

 

SET Command with Objects

The following example contains three SET commands: the first sets a variable to an oref (object reference); the second sets a variable to the value of an object property; the third sets an object property to a value:

  SET myobj\=##class(%SQL.Statement).%New()
  SET dmode\=myobj.%SelectMode
  WRITE "Default select mode=",dmode,!
  SET myobj.%SelectMode\=2
  WRITE "Newly set select mode=",myobj.%SelectMode

 

Note that dot syntax is used in object expressions; a dot is placed between the object reference and the object property name or object method name.

To set a variable with an object property or object method value for the current object, use the double-dot syntax:

   SET x\=..LastName

If the specified object property does not exist, Caché issues a <PROPERTY DOES NOT EXIST> error. If you use double-dot syntax and the current object has not been defined, Caché issues a <NO CURRENT OBJECT> error.

For further details, refer to [Object-Specific ObjectScript Features](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos) in Using Caché Objects.

When using SET with objects, do not perform multiple assignments. For example, avoid statements such as:

 // Avoid this syntax:
   SET (a.Name,b.Name)\=z

Instead, issue a separate SET command for each assignment, as shown in the following example:

  SET a.Name\=z
  SET b.Name\=z

The following command sets x to the value returned by the GetNodeName() method:

  SET x\=##class(%SYS.System).GetNodeName()
  WRITE "the current system node is: ",x

 

A SET command for objects can take an expression with cascading dot syntax, as shown in the following examples:

   SET x\=patient.Doctor.Hospital.Name

In this example, the patient.Doctor object property references the Hospital object, which contains the Name property. Thus, this command sets x to the name of the hospital affiliated with the doctor of the specified patient. The same cascading dot syntax can be used with object methods.

A SET command for objects can be used with system-level methods, such as the following data type property method:

   SET x\=patient.NameIsValid(Name)

In this example, the NameIsValid() method returns its result for the current patient object. NameIsValid() is a boolean method generated for data type validation of the Name property. Thus, this command sets x to 1 if the specified name is a valid name, and sets x to 0 if the specified name is not a valid name.

SET Using an Object Method

You can specify an object method on the left side of a SET expression. The following example specifies the %Get() method:

  SET obj\=##class(test).%New()  // Where test is class with a multidimensional property md
  SET myarray\=\[(obj)\]
  SET index\=0,subscript\=2
  SET myarray.%Get(index).md(subscript)\="value"
  IF obj.md(2)\="value" {WRITE "success"}
  ELSE {WRITE "failure"} 

Notes

Each variable assignment can be a local variable, a process-private global, or a global, the $PIECE function, the $EXTRACT function, and certain special variables, including $ECODE, $ETRAP, $DEVICE, $KEY, $TEST, $X, and $Y.

If the target variable does not already exist, SET creates it and then assigns the value. If it does exist, SET replaces the existing value with the assigned value.

SET and Subscripts

You can set individual subscripted values (array nodes) for a local variable, process-private global, or a global. You can set subscripts in any order. If the variable subscript level does not already exist, SET creates it and then assigns the value. Each subscript level is treated as an independent variable; only those subscript levels set are defined. For example:

   KILL myarray
   SET myarray(1,1,1)\="Cambridge"
   WRITE !,myarray(1,1,1)
   SET myarray(1)\="address"
   WRITE !,myarray(1)

 

In this example, the variables myarray(1,1,1) and myarray(1) are defined and contain values. However, the variables myarray and myarray(1,1) are not defined, and return an <UNDEFINED> error when invoked.

The maximum number of subscript levels for a local variable is 255. The maximum number of subscript levels for a global variable depends on the subscript level names, and may exceed 255 levels. Attempting to set a local variable to more than 255 subscript levels (either directly or by [indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection)) results in a <SYNTAX> error. For further information on subscripted variables, refer to [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

Order of Evaluation

Caché evaluates the arguments of the SET command in strict left-to-right order. For each argument, it performs the evaluation in the following sequence:

1.  Evaluates occurrences of indirection or subscripts to the left of the equal sign in a left-to-right order to determine the variable name(s). For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.
    
2.  Evaluates the expression to the right of the equal sign.
    
3.  Assigns the expression to the right of the equal sign to the variable name or references to the left of the equal sign.
    

Transaction Processing

A SET of a global variable is journaled as part of the current transaction; this global variable assignment is rolled back during transaction rollback. A SET of a local variable or a process-private global variable is not journaled, and thus this assignment is unaffected by a transaction rollback.

Defined and Undefined Variables

Most Caché commands and functions require that a variable be defined before it is referenced. By default, attempting to reference an undefined variable generates an <UNDEFINED> error. Attempting to reference an undefined object generates a <PROPERTY DOES NOT EXIST> or <METHOD DOES NOT EXIST> error. Refer to [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) for further details on these error codes.

You can change Caché behavior when referencing an undefined variable by setting the [%SYSTEM.Process.Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method.

The [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command and the [$INCREMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement) function can reference an undefined variable and assign a value to it. The [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) function can take an undefined or defined variable and return its status. The [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) function returns the value of a defined variable; optionally, it can also assign a value to an undefined variable.

SET with $PIECE and $EXTRACT

You can use the $PIECE and $EXTRACT functions with SET on either side of the equals sign. For detailed descriptions, refer to [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) and [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract).

When used on the right side of the equals sign, $PIECE and $EXTRACT extract a substring from a variable and assign its value to the specified variable(s) on the left side of the equals sign. $PIECE extracts a substring using a specified delimiter, and $EXTRACT extracts a substring using a character count.

For example, assume that variable x contains the string "HELLO WORLD". The following commands extract the substring "HELLO" and assign it to variables y and z, respectively:

   SET x\="HELLO WORLD"
   SET y\=$PIECE(x," ",1)
   SET z\=$EXTRACT(x,1,5)
   WRITE "x=",x,!,"y=",y,!,"z=",z

 

When used on the left side of the equals sign, $PIECE and $EXTRACT insert the value from the expression on the right side of the equals sign into the specified portion of the target variable. Any existing value in the specified portion of the target variable is replaced by the inserted value.

For example, assume that variable x contains the string "HELLO WORLD" and that variable y contains the string "HI THERE". In the command:

   SET x\="HELLO WORLD"
   SET y\="HI THERE"
   SET $PIECE(x," ",2)\=$EXTRACT(y,4,9)
   WRITE "x=",x

 

The $EXTRACT function extracts the string "THERE" from variable y and the $PIECE function inserts it into variable x at the second field position, replacing the existing string "WORLD". Variable x now contains the string "HELLO THERE".

If the target variable does not exist, Caché creates it and pads it with delimiters (in the case of $PIECE) or with spaces (in the case of $EXTRACT) as needed.

In the following example, SET $EXTRACT is used to insert the value of z into strings x and y, overwriting the existing values:

   SET x\="HELLO WORLD"
   SET y\="OVER EASY"
   SET z\="THERE"
   SET $EXTRACT(x,7,11)\=z
   SET $EXTRACT(y,\*\-3,\*)\=z
   WRITE "edited x=",x,!
   WRITE "edited y=",y  

 

Variable x now contains the string "HELLO THERE" and y contains the string "OVER THERE". Note that because one of the SET $EXTRACT operations in this example uses a negative offset (\*-3) these operations must be done as separate sets. You cannot set multiple variables with a single SET using enclosing parentheses if any of the variables uses negative offset.

In the following example, assume that the global array ^client is structured so that the root node contains the client’s name, with subordinate nodes containing the street address and city. For example, ^client(2,1,1) would contain the city address for the second client stored in the array.

Assume further that the city node (x,1,1) contains field values identifying the city, state abbreviation, and ZIP code (postal code), with the comma as the field separator. For example, a typical city node value might be "Cambridge,MA,02142". The three SET commands in the following code each use the $PIECE function to assign a specific portion of the array node value to the appropriate local variable. Note that in each case $PIECE references the comma (",") as the string separator.

ADDRESSPIECE
   SET ^client(2,1,1)\="Cambridge,MA,02142"
   SET city\=$PIECE(^client(2,1,1),",",1)
   SET state\=$PIECE(^client(2,1,1),",",2)
   SET zip\=$PIECE(^client(2,1,1),",",3)
   WRITE "City is ",city,!,
         "State or Province is ",state,!
        ,"Postal code is ",zip
   QUIT

 

The $EXTRACT function could be used to perform the same operation, but only if the fields were fixed length and the lengths were known. For example, if the city field was known to contain only up to 9 characters and the state and ZIP fields were known to contain only 2 and 5 characters, respectively, the SET commands could be coded with the $EXTRACT function as follows:

ADDRESSEXTRACT
   SET ^client(2,1,1)\="Cambridge,MA,02142"
   SET city\=$EXTRACT(^client(2,1,1),1,9)
   SET state\=$EXTRACT(^client(2,1,1),11,12)
   SET zip\=$EXTRACT(^client(2,1,1),14,18)
   WRITE "City is ",city,!,
         "State or Province is ",state,!,
         "Postal code is ",zip
   QUIT

 

Notice the gaps between 9 and 11 and 12 and 14 to accommodate the comma field separators.

The following example replaces the first substring in A (originally set to 1) with the string "abc".

StringPiece
   SET A\="1^2^3^4^5^6^7^8^9"
   SET $PIECE(A,"^")\="abc"
   WRITE !,"A=",A
   QUIT

 

A="abc^2^3^4^5^6^7^8^9"

The following example uses $EXTRACT to replace the first character in A (again, a 1) with the string "abc".

StringExtract
   SET A\="123456789"
   SET $EXTRACT(A)\="abc"
   WRITE !,"A=",A
   QUIT

 

A="abc23456789"

The following example replaces the third through sixth pieces of A with the string "abc" and replaces the first character in the variable B with the string "abc".

StringInsert
   SET A\="1^2^3^4^5^6^7^8^9"
   SET B\="123"
   SET ($PIECE(A,"^",3,6),$EXTRACT(B))\="abc"
   WRITE !,"A=",A,!,"B=",B
   QUIT

 

A="1^2^abc^7^8^9"

B="abc23"

The following example sets $X, $Y, $KEY, and the fourth piece of a previously undefined local variable, A, to the value of 20. It also sets the local variable K to the current value of $KEY. A includes the previous three pieces and their caret delimiter (^).

SetVars
   SET ($X,$Y,$KEY,$PIECE(A,"^",4))\=20,X\=$X,Y\=$Y,K\=$KEY
   WRITE !,"A=",A,!,"K=",K,!,"X=",X,!,"Y=",Y
   QUIT

 

A="^^^20" K="20" X=20 Y=20

The following example sets $ECODE and $ETRAP to the null string. Then, the example sets the local variables EC and ET to the values of $ECODE and $ETRAP.

SetEvars
   SET ($ECODE,$ETRAP)\="",EC\=$ECODE,ET\=$ETRAP 
   WRITE "EC=",EC,!,"ET=",ET
   QUIT

 

EC=""

ET=""

SET with $LIST and $LISTBUILD

The $LIST functions create and manipulate lists. They encode the length (and type) of each element within the list, rather than using an element delimiter. They then use the encoded length specifications to extract specified list elements during list manipulation. Because the $LIST functions do not use delimiter characters, the lists created using these functions should not be input to $PIECE or other character-delimiter functions.

When used on the right side of the equal sign, these functions return the following:

$LIST

$LIST returns the specified element of the specified list.

$LISTBUILD

$LISTBUILD returns a list containing one element for each argument given.

When used on the left side of the equal sign, in a SET argument, these functions perform the following tasks:

$LIST

Replaces the specified element(s) with the value given on the right side of the equal sign.

   SET A\=$LISTBUILD("red","blue","green","white")
   WRITE "Created list A=",$LISTTOSTRING(A),!
   SET $LIST(A,2)\="yellow"
   WRITE "Edited list A=",$LISTTOSTRING(A)

 

   SET A\=$LISTBUILD("red","blue","green","white")
   WRITE "Created list A=",$LISTTOSTRING(A),!
   SET $LIST(A,\*\-1,\*)\=$LISTBUILD("yellow")
   WRITE "Edited list A=",$LISTTOSTRING(A)

 

You cannot use parentheses with SET $LIST to assign the same value to multiple variables.

$LISTBUILD

Extracts several elements of a list in a single operation. The arguments of $LISTBUILD are variables, each of which receives an element of the list corresponding to their position in the $LISTBUILD parameter list. Variable names may be omitted for positions that are not of interest.

In the following example, $LISTBUILD (on the right side of the equal sign) is first used to return a list. Then $LISTBUILD (on the left side of the equal sign) is used to extract two items from that list and set the appropriate variables.

SetListBuild
   SET J\=$LISTBUILD("red","blue","green","white")
   SET $LISTBUILD(A,,B)\=J
   WRITE "A=",A,!,"B=",B

 

In this example, A="red" and B="green".

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$X](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx) special variable
    
*   [$Y](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cset.xml**