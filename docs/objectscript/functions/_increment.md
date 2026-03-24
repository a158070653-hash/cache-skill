# $INCREMENT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fincrement

---

 

Caché ObjectScript Reference

$INCREMENT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$INCREMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement "$INCREMENT") \]

Go to: Description Parameters $INCREMENT or $SEQUENCE $INCREMENT and Global Variables Incrementing Strings Failure to Increment Locking and Simultaneous Global Increments $INCREMENT and Transaction Processing Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Adds a specified increment to the numeric value of a variable.

Synopsis

$INCREMENT(variable,num)
$I(variable,num)

Parameters

variable

The variable whose value is to be incremented. It can specify a local variable, a process private global, or a global variable and can be either subscripted or unsubscripted. The variable need not be defined. If the variable is not defined, or is set to the null string (""), $INCREMENT treats it as having an initial value of zero and increments accordingly. A literal value cannot be specified here. You cannot specify a simple object property reference as variable; you can specify a multidimensional property reference as variable with the syntax obj.property.

num

Optional — The numeric increment you want to add to variable. The value can be a number (integer or non-integer, positive or negative), a string containing a number, or any expression which evaluates to a number. Leading and trailing blanks and multiple signs are evaluated. A string is evaluated until the first nonnumeric character is encountered. The null string ("") is evaluated as zero.

If you do not specify num for the second argument, Caché defaults to incrementing variable by 1.

Description

$INCREMENT resets the value of a variable by adding a specified increment to the existing value of the variable and returning the incremented value. This is shown in the following example:

  SET a\=7
  SET result\=$INCREMENT(a)
  WRITE !,result   /\* result is 8 (a+1)        \*/
  WRITE !,a        /\* variable a is also now 8 \*/

 

You can use the $GET function to return the current value of a variable.

$INCREMENT performs this increment as an atomic operation, which does not require the use of the LOCK command.

If multiple processes simultaneously increment the same global through $INCREMENT, each process receives a unique, increasing number (or decreasing number if num is negative). In some situations, certain numbers may be skipped due to timing issues. For further details on using $INCREMENT with global variables, see [Using Multidimensional Storage (Globals)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Caché does not restore the original, non-incremented value if $INCREMENT is in a transaction that is rolled back.

$INCREMENT and $ZINCREMENT have the same syntax and effects. You can use $ZINCREMENT in any situation in which you would use $INCREMENT.

Parameters

variable

The variable whose data value is to be incremented. It must be a variable, it cannot be a literal. The variable does not need to be defined. $INCREMENT defines an undefined variable, setting its value to num (1, by default).

The variable parameter can be a [local variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local), [process-private global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls), or [global variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global), either subscripted or unsubscripted. If a global variable, it can contain an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). If a subscripted global variable, it can be specified using a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using).

The variable parameter can be a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference. For example, $INCREMENT(..Count). It cannot be a non-multidimensional object property. Attempting to increment a non-multidimensional object property results in an <OBJECT DISPATCH> error.

$INCREMENT cannot increment [special variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special), even those that can be modified using SET. Attempting to increment a special variable results in a <SYNTAX> error.

num

The amount to increment (or decrement) by. The num parameter can be a positive number, incrementing the value of variable, or a negative number, decrementing the value of variable. It can be an integer or a fractional number. num can be zero (no increment). A numeric string is treated as a number. An empty string ("") or a non-numeric string is treated as an increment of zero. If you do not specify an increment, Caché uses the default increment of one (1).

$INCREMENT or $SEQUENCE

[$SEQUENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence) and $INCREMENT can be used as alternatives, or can be used in combination with each other. $SEQUENCE is intended specifically for integer increment operations involving multiple simultaneous processes. $INCREMENT is a more general increment/decrement function.

*   $SEQUENCE increments global variables. $INCREMENT increments local variables, global variables, or process-private globals.
    
*   $SEQUENCE increments an integer by 1. $INCREMENT increments or decrements any numeric value by any specified numeric value.
    
*   $SEQUENCE can allocate a range of increments to a process. $INCREMENT allocates only a single increment.
    
*   SET $SEQUENCE can be used to change or undefine (kill) a global. $INCREMENT cannot be used on the left side of the SET command.
    

$INCREMENT and Global Variables

You can use $INCREMENT on a global variable or a subscript node of a global variable. You can access a global variable mapped to another namespace using an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). You can access a subscripted global variable using a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using).

Caché evaluates parameters in left-to-right order. If num (the amount to increment) is a subscripted global, Caché uses this global reference to set the naked indicator, affecting all subsequent naked global references.

Incrementing Strings

$INCREMENT is generally used for incrementing a variable containing a numeric value. However, it does accept a variable containing a string. The following rules apply when using $INCREMENT on a string:

*   A null string ("") is treated as having a value of zero.
    
*   A numeric string ("123" or "+0012.30") is treated as having that numeric value. The string is converted to canonical form: leading and trailing zeros and the plus sign are removed.
    
*   A mixed numeric/nonnumeric string ("12AB" or "1,000") is treated as the numeric value up to the first nonnumeric character and then truncated at that point. (Note that a comma is a nonnumeric character.) The resulting numeric substring is converted to canonical form: leading and trailing zeros and the plus sign are removed.
    
*   A nonnumeric string ("ABC" or "$12") is treated as having a value of zero.
    
*   Scientific notation conversion is performed. For example, if strvar\="3E2", $INCREMENT treats it as having a value of 300.
    
*   Arithmetic operations are not performed. For example, if strvar\="3+7", $INCREMENT will truncate the string at the plus sign (treating it as a nonnumeric character) and increment strvar to 4.
    
*   Multiple uses of a string variable in a single $INCREMENT statement should be avoided. For example, avoid concatenating a string variable to the increment of that variable: strvar\_$INCREMENT(strvar). This returns unpredictable results.
    

Failure to Increment

If $INCREMENT cannot increment variable, it issues a <MAXINCREMENT> error. This only occurs when the num increment value is extremely small, and/or the variable value is extremely large.

An increment by zero (num\=0) always returns the original number, regardless of its size. It does not issue a <MAXINCREMENT> error.

<MAXINCREMENT> occurs when the numeric types of the parameters differ and the resulting type conversion and rounding would result in no increment. If you use $INCREMENT on a very large number, the default increment of 1 (or some other small positive or negative value of num) is too small to be significant. Similarly, if you specify a very small fractional num value, its value is too small to be significant. Rather than returning the original variable number without incrementing it, $INCREMENT generates a <MAXINCREMENT> error.

In the following example, 1.2E18 is a number that can be incremented or decremented by 1; 1.2E20 is a number that is too large to be incremented or decremented by 1. The first three $INCREMENT functions successfully increment or decrement the number 1.2E18. The fourth and fifth $INCREMENT functions increment by zero, and so always return the original number unchanged, regardless of the size of the original number. The sixth and seventh $INCREMENT functions provide a num increment sufficiently large to successfully increment or decrement the number 1.2E20. The eighth $INCREMENT function attempts to increment 1.2E20 by 1, and thus generates a <MAXINCREMENT> error.

  SET x\=1.2E18
  WRITE "E18      :",x,!
  WRITE "E18+1    :",$INCREMENT(x),!
  WRITE "E18+4    :",$INCREMENT(x,4),!
  WRITE "E18-6    :",$INCREMENT(x,\-6),!
  WRITE "E18+0    :",$INCREMENT(x,0),!
  SET y\=1.2E20
  WRITE "E20      :",y,!
  WRITE "E20+0    :",$INCREMENT(y,0),!
  WRITE "E20-10000:",$INCREMENT(y,\-10000),!
  WRITE "E20+10000:",$INCREMENT(y,10000),!
  WRITE "E20+1    :",$INCREMENT(y),!

 

Locking and Simultaneous Global Increments

$INCREMENT and $ZINCREMENT do not perform a lock operation when they increment variable nor does using the LOCK command on variable prevent $INCREMENT from incrementing or decrementing its value. For example, suppose process 1 executes a lock on ^COUNTER:

   LOCK ^COUNTER

Then suppose, process 2 increments ^COUNTER:

   SET x\=$INCREMENT(^COUNTER,VAL)

Process 2 is not prevented from incrementing ^COUNTER by the lock held by process 1.

The two processes are not guaranteed their own unique ^COUNTER values unless both are using $INCREMENT.

$INCREMENT and Transaction Processing

The common usage for $INCREMENT is to increment a counter before adding a new entry to a database. $INCREMENT provides a way to do this very quickly, avoiding the use of the LOCK command.

The trade off for this is that the counter is not locked. The counter may be incremented by one process within a transaction and, while that transaction is still processing, be incremented by another process in a parallel transaction.

In the event either transaction (or any other transaction that uses $INCREMENT) must be rolled back (with the TROLLBACK command), counter increments are ignored. The counter variables are not decremented since it is not clear whether the resulting counter value would be valid. In all likelihood, such a rollback would be disastrous for other transactions.

For further details on using $INCREMENT in a distributed database environment, refer to “The $INCREMENT Function and Application Counters” in the [Developing Distributed Applications](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDDM_develop) chapter of the Caché Distributed Data Management Guide.

Examples

The following example increments the value of myvar by n. Note that myvar does not have to be a prior defined variable:

   SET n\=4
   KILL myvar
   SET VAL\=$INCREMENT(myvar,n)       ; returns 4
   WRITE !,myvar
   SET VAL\=$INCREMENT(myvar,n)       ; returns 8
   WRITE !,myvar
   SET VAL\=$INCREMENT(myvar,n)       ; returns 12
   WRITE !,myvar

 

The following example adds incremental values to the process private variable ^||xyz using $INCREMENT. The one-argument form of $INCREMENT increments by 1; the two-argument form increments by the value specified in the second argument. In this case, the second argument is a non-integer value.

   KILL ^||xyz
   WRITE !,$INCREMENT(^||xyz)       ; returns 1
   WRITE !,$INCREMENT(^||xyz)       ; returns 2
   WRITE !,$INCREMENT(^||xyz)       ; returns 3
   WRITE !,$INCREMENT(^||xyz,3.14)  ; returns 6.14

 

The following example shows the effects of incrementing by zero (0) and incrementing by a negative number:

   KILL xyz
   WRITE !,$INCREMENT(xyz,0)  ; initialized as zero
   WRITE !,$INCREMENT(xyz,0)  ; still zero
   WRITE !,$INCREMENT(xyz)    ; increments by 1 (default)
   WRITE !,$INCREMENT(xyz)    ; increments by 1 (=2)
   WRITE !,$INCREMENT(xyz,\-1) ; decrements by -1 (=1)
   WRITE !,$INCREMENT(xyz,\-1) ; decrements by -1 (=0)
   WRITE !,$INCREMENT(xyz,\-1) ; decrements by -1 (=-1)

 

The following example shows the effects of incrementing using mixed (numeric and nonnumeric) num strings and the null string:

   KILL xyz
   WRITE !,$INCREMENT(xyz,"")
           ; null string initializes to 0
   WRITE !,$INCREMENT(xyz,2)
           ; increments by 2
   WRITE !,$INCREMENT(xyz,"")
           ; null string increments by 0 (xyz=2)
   WRITE !,$INCREMENT(xyz,"3A4") 
           ; increments by 3 (rest of string ignored)
   WRITE !,$INCREMENT(xyz,"A4")
           ; nonnumeric string evaluates as zero (xyz=5)
   WRITE !,$INCREMENT(xyz,"1E2")
           ; increments by 100 (scientific notation)

 

See Also

*   [$SEQUENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence) function
    
*   [$ZINCREMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzincrement) function
    
*   [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) function
    
*   [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) command
    
*   [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fincrement.xml**