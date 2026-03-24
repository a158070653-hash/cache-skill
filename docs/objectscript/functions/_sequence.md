# $SEQUENCE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fsequence

---

 

Caché ObjectScript Reference

$SEQUENCE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortbegin "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$SEQUENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence "$SEQUENCE") \]

Go to: Description Parameter Incrementing Very Large Numbers $SEQUENCE or $INCREMENT Locking and Simultaneous Global Increments $SEQUENCE and Transaction Processing See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Increments a global variable shared by multiple processes.

Synopsis

$SEQUENCE(gvar)
$SEQ(gvar)

SET $SEQUENCE(gvar)=value
SET $SEQ(gvar)=value

Parameter

gvar

The variable whose value is to be incremented. Commonly, gvar is a global variable (^gvar), either subscripted or unsubscripted. The variable need not be defined. If gvar is not defined, or is set to the null string (""), $SEQUENCE treats it as having an initial value of zero and increments accordingly, returning a value of 1.

You cannot specify a literal value for gvar. You cannot specify a simple object property reference as gvar; you can specify a multidimensional property reference as gvar with the syntax obj.property.

Description

$SEQUENCE provides a fast way for multiple processes to obtain unique (non-duplicate) integer indices for the same global variable. For each process, $SEQUENCE allocates a sequence (range) of integer values. Subsequent calls to $SEQUENCE increment to the next value in the allocated sequence for that process. When a process consumes all of the integer values in its allocated sequence, it is automatically assigned a new sequence of integer values. $SEQUENCE automatically determines the size of the sequence of integer values to allocate. It determines the size of the allocated sequence separately for each sequence allocation. In some cases, this sequence may be a single integer.

$SEQUENCE always increments an integer value by 1. By default, $SEQUENCE assigns positive integers, beginning with 1. However, $SEQUENCE can be set to a negative integer; negative integers are incremented towards zero. If $SEQUENCE was set to a negative integer, a subsequent call can assign zero as an increment. Setting gvar to a non-integer numeric value generates an <ILLEGAL VALUE> error.

$SEQUENCE is intended to be used when multiple processes concurrently increment the same global. Both $SEQUENCE and $INCREMENT can perform this operation, but $SEQUENCE commonly provides better performance. The order in which $SEQUENCE allocates indices is different from the $INCREMENT order. $SEQUENCE can allocate a sequential range of increments to a process, rather than the $INCREMENT behavior of allocating single integer increments to each process. This can substantially improve performance by reducing process collision and synchronization. It can also improve data block performance when inserting records, because sequential record IDs are grouped by process.

When a process calls $SEQUENCE, one of the following occurs:

*   The ^gvar global variable is undefined because no process has defined this variable. The integers returned by $SEQUENCE will start with 1.
    
*   The ^gvar global variable was last modified by a $SEQUENCE call from another process. The integers returned by $SEQUENCE will start with the first integer after the sequence allocated to the other process.
    
*   The ^gvar global variable was last modified by a SET $SEQUENCE called from any process. The integers returned by $SEQUENCE will start with the first integer after the value to which $SEQUENCE was set.
    

The size of the sequence that $SEQUENCE allocates to a process depends on an internal timestamp. When a process invokes $SEQUENCE for the second time, Caché compares the prior timestamp with the current time. Depending on the duration between these $SEQUENCE calls, Caché allocates either a single increment or a calculated sequence of increments to the process:

*   Allocated sequence is 1: $SEQUENCE behaves like $INCREMENT.
    
*   Allocated sequence is > 1: $SEQUENCE uses this per-process sequence of increments. Each process uses its allocated sequence, then is assigned a new sequence.
    

For example, Process A and Process B are both incrementing the same global. The first time each process increments the global it is a single increment. The next time each process increments the global, Caché compares the two $SEQUENCE operations and calculates a sequence of increments (this sequence may be one integer). Subsequent $SEQUENCE operations use up these per-process sequences before re-allocating increments. This might result in increments such as the following: A1, B2 (single increments setting the clock), A3 (Cache compares A1 & A3, allocates 4, 5, 6, 7 to Process A), B8 (Cache compare B2 and B8, allocates 9, 10, 11 to Process B). The full increment sequence might be as follows: A1, B2, A3, A4, B8, A5, A6, B9, A7, B10, B11.

If a process does not use all of its allocated sequence, the remaining numbers are unused, gaps in the increment sequence.

The following example shows the difference between the increment integer returned by $SEQUENCE (the current sequence number) and the value of gvar (the highest allocated sequence number):

  SET $SEQUENCE(^myseq)\=""
  FOR i\=1:1:15 {WRITE "increment:",$SEQ(^myseq)," allocated:",^myseq,! }

 

For further details on using $SEQUENCE with global variables, see [Using Multidimensional Storage (Globals)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Dedicated Global Variable

Once a sequence has been started by the first call on $SEQUENCE(^gvar), all future changes to the value of ^gvar during the life of that sequence can only be made by calling $SEQUENCE(^gvar). Using any other function or statement to change the value of ^gvar causes the sequence to become undefined.

Restrictions on the use of $SEQUENCE and $INCREMENT on the same global are described below.

SET $SEQUENCE

You can use SET $SEQUENCE to kill or reset a $SEQUENCE global. SET $SEQUENCE resets the global variable and deallocates sequences of integers allocated to other processes.

*   SET $SEQUENCE(^gvar)="" kills the specified global node and notifies all jobs with cached $SEQUENCE numbers to purge their current increment value. The first call to $SEQUENCE increments to 1. SET $SEQUENCE(^gvar)="" only kills the specified global node; it does not kill that node’s descendants (if any).
    
*   SET $SEQUENCE(^gvar)=n (where n is an integer) resets the specified global node to n, and notifies all jobs with cached $SEQUENCE numbers to purge their current increment value. Subsequent invocations of $SEQUENCE on all jobs will use the new n increment starting value.
    
    If SET $SEQUENCE attempts to set ^gvar to a fractional number, an <ILLEGAL VALUE> error occurs. If SET $SEQUENCE attempts to set ^gvar to a non-numeric string, ^gvar is set to 0.
    

You cannot use KILL ^gvar or SET ^gvar to kill or reset a $SEQUENCE global because these commands do not deallocate sequences of integers allocated to processes.

Parameter

gvar

A variable containing an integer value to be incremented. The variable does not need to be defined; the first call to $SEQUENCE defines an undefined variable as 0 then increments its value to 1. The gvar value must be a positive or negative integer.

Commonly, the gvar parameter is a [global variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global), either subscripted or unsubscripted: ^gvar. It can contain an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). If a subscripted global variable, it can be specified using a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using).

The gvar parameter can be a local variable or [process-private global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls). However, because $SEQUENCE is intended for use across processes, this usage is not meaningful, in most cases. Using $SEQUENCE on a local variable or process-private global is the same as using $INCREMENT with a numeric increment of 1. The $SEQUENCE restrictions described below concerning locking, journaling, and transaction rollback do not apply to local variables or process-private globals. Using $SEQUENCE on a local variable or process-private global has the same error behavior as $INCREMENT; this is different from the error behavior for $SEQUENCE on a global variable, as described in the next section.

The gvar parameter can be a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference. For example, $SEQUENCE(..Count). It cannot be a non-multidimensional object property. Attempting to increment a non-multidimensional object property results in an <OBJECT DISPATCH> error.

$SEQUENCE cannot increment [special variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special), even those that can be modified using SET. Attempting to increment a special variable results in a <SYNTAX> error.

Incrementing Very Large Numbers

The integers returned by $SEQUENCE are in the range -9223372036854775807 to 9223372036854775806 (-2\*\*63+1 to 2\*\*63-2). Attempting a SET $SEQUENCE on a global variable with an integer beyond this range generates an <ILLEGAL VALUE> error.

In the following example, $SEQUENCE on a global variable can be set to 9.223372036854775800E18, but incrementing this number past the range limit generates a <MAXINCREMENT> error. You can run this example repeatedly to perform “slow increments” and “fast increments”. Note that “fast increments” in this example may result in <MAXINCREMENT> before actually incrementing to the range limit, because $SEQUENCE is attempting to allocate a sequence of numbers beyond the range limit:

  TRY {
  SET rand\=$RANDOM(2)
  SET $SEQUENCE(^bignum)\=9.223372036854775800E18
    IF rand\=0 {  WRITE "slow increments:",!
    FOR x\=1:1:10 {WRITE $SEQUENCE(^bignum)," increment #",x,!
                HANG .5 }
    }
    IF rand\=1 {WRITE "fast increments:",!
    FOR i\=1:1:10 {WRITE $SEQUENCE(^bignum)," increment #",i,!}
    }
  }
  CATCH exp { WRITE !,"In the CATCH block",!
                IF 1\=exp.%IsA("%Exception.SystemException") {
                  WRITE "System exception",!
                  WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
                  WRITE "Location: ",exp.Location,!
                }
                ELSE { WRITE "unknown error",! }
  }

 

These types of errors only occur when incrementing a global variable.

$SEQUENCE or $INCREMENT

$SEQUENCE is intended specifically for integer increment operations involving multiple simultaneous processes. $INCREMENT is a more general increment/decrement function:

*   $SEQUENCE increments an integer by 1. $INCREMENT increments or decrements any numeric value by any specified numeric value.
    
*   $SEQUENCE can allocate a sequence of increments to a process. $INCREMENT allocates only a single increment.
    
*   SET $SEQUENCE can be used to change or undefine (kill) a global. $INCREMENT cannot be used on the left side of the SET command.
    

Note:

$SEQUENCE and $INCREMENT may be used on the same global variable only when performing an operation that simply increments a numeric value, such as Id allocation. Any other use of $SEQUENCE and $INCREMENT on the same global may produce unpredictable results and is not recommended.

Locking and Simultaneous Global Increments

$SEQUENCE uses special, efficient locking techniques that only synchronize $SEQUENCE calls with other $SEQUENCE calls. Attempting to use the LOCK command on a global used by $SEQUENCE will have no effect on $SEQUENCE. For example, suppose Process 1 executes a lock on ^COUNTER:

   LOCK ^COUNTER

Then suppose, Process 2 increments ^COUNTER:

   SET x\=$SEQUENCE(^COUNTER)

Process 2 is not prevented from incrementing ^COUNTER by the lock held by Process 1.

$SEQUENCE and Transaction Processing

*   Locking: The common usage for $SEQUENCE is to increment a counter before adding a new entry to a database. $SEQUENCE provides a way to do this very quickly, avoiding the use of the LOCK command. The trade-off for this is that gvar is not locked. The gvar may be incremented by one process within a transaction and, while that transaction is still processing, be incremented by another process in a parallel transaction.
    
*   Rollback: Calls to $SEQUENCE are not journaled. Therefore, rolling back a transaction will not change the value of gvar. Any integer values allocated by $SEQUENCE during a rolled back transaction will not be available for allocation by any future $SEQUENCE call.
    

For further details on using $SEQUENCE in a distributed database environment, refer to [The $INCREMENT Function and Application Counters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDDM_develop#GDDM_develop_considerations) in the “Developing Distributed Applications” chapter of the Caché Distributed Data Management Guide.

See Also

*   [$INCREMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement) function
    
*   [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) command
    
*   [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) function
    
*   [$ZINCREMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzincrement) function
    
*   [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortbegin "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fsequence.xml**