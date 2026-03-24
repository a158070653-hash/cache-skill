# $TLEVEL

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vtlevel

---

 

Caché ObjectScript Reference

$TLEVEL

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthrowobj "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtlevel "$TLEVEL") \]

Go to: Description Transaction Level and the Terminal Prompt Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the current nesting level for transaction processing.

Synopsis

$TLEVEL
$TL

Description

$TLEVEL contains the current transaction level, the number of nested open transactions. The number of TSTART commands issued determines the transaction level.

*   Each [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart) increments $TLEVEL by 1.
    
*   Each [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit) decrements $TLEVEL by 1.
    
*   Each [TROLLBACK 1](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) decrements $TLEVEL by 1.
    
*   A [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) resets $TLEVEL to 0.
    

A $TLEVEL of 0 cannot be decremented. Issuing a TROLLBACK (or TROLLBACK 1) when $TLEVEL\=0 performs no operation. Issuing a TCOMMIT when $TLEVEL\=0 results in a <COMMAND> error.

The maximum number of transaction levels is 255. Attempting to exceed 255 transaction levels generates a <TRANSACTION LEVEL> error.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

SQL and $TLEVEL

$TLEVEL is also set by SQL transaction statements as follows:

*   An initial [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction) sets $TLEVEL to 1. Additional START TRANSACTION statements have no effect on $TLEVEL.
    
*   Each [SAVEPOINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint) statement increments $TLEVEL by 1.
    
*   A [ROLLBACK TO SAVEPOINT pointname](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback) statement decrements $TLEVEL. The amount of decrement depends on the savepoint specified.
    
*   A [COMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit) resets $TLEVEL to 0.
    
*   A [ROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback) resets $TLEVEL to 0.
    

Despite their shared use of $TLEVEL, Caché ObjectScript transaction processing differs from, and is incompatible with, SQL transaction processing. An application should not attempt to mix the two types of transaction processing statements within the same transaction.

Transaction Level and the Terminal Prompt

By default, if $TLEVEL is greater than 0 at the conclusion of a command line or program executed from the Terminal prompt, the current transaction level is displayed as a Terminal prompt prefix.

*   When $TLEVEL\=0, the Terminal prompt displays the namespace name (by default). For example, USER>
    
*   When $TLEVEL\>0, the Terminal prompt displays the TLn: prefix before the namespace name, n being an integer 1 through 255. For example, TL4:USER>.
    

This Terminal prompt display is configurable, as described in [ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace).

The [SQL Shell](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_shell) prompt does not display the current transaction level. Upon exiting the SQL Shell the current $TLEVEL value is displayed at the Terminal prompt. This can including transaction levels established before entering the SQL Shell and transaction level changes that occurred while in the SQL Shell.

The [MV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmv) command (with no argument) enters the interactive MultiValue Shell and immediately rolls back any open transactions. Upon exiting the MultiValue Shell the current $TLEVEL value is 0. The MV command with a MultiValue command line argument does not affect the current transaction level.

Examples

The following example shows that each TSTART increments $TLEVEL and each TCOMMIT decrements $TLEVEL:

  WRITE !,"transaction level ",$TLEVEL  // 0
  TSTART
  WRITE !,"transaction level ",$TLEVEL  // 1
  TSTART
  WRITE !,"transaction level ",$TLEVEL  // 2
  TCOMMIT
  WRITE !,"transaction level ",$TLEVEL  // 1
  TCOMMIT
  WRITE !,"transaction level ",$TLEVEL  // 0

 

The following example shows that repeated invocations of TSTART increment $TLEVEL, and TROLLBACK 1 decrements $TLEVEL.

  WRITE !,"transaction level ",$TLEVEL  // 0
  TSTART
  WRITE !,"transaction level ",$TLEVEL  // 1
  TSTART
  WRITE !,"transaction level ",$TLEVEL  // 2
  TROLLBACK 1
  WRITE !,"transaction level ",$TLEVEL  // 1

 

The following example shows that repeated invocations of TSTART increment $TLEVEL, and TROLLBACK resets $TLEVEL to 0.

  WRITE !,"transaction level ",$TLEVEL  // 0
  TSTART
  TSTART
  TSTART
  WRITE !,"transaction level ",$TLEVEL  // 3
  TROLLBACK
  WRITE !,"transaction level ",$TLEVEL  // 0

 

The following example shows that if $TLEVEL is 0, TROLLBACK commands have no effect:

  WRITE !,"transaction level ",$TLEVEL  // 0
  TROLLBACK
  WRITE !,"transaction level ",$TLEVEL  // 0
  TROLLBACK 1
  WRITE !,"transaction level ",$TLEVEL  // 0
  TROLLBACK
  WRITE !,"transaction level ",$TLEVEL  // 0

 

See Also

*   [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit) command
    
*   [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) command
    
*   [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart) command
    
*   [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthrowobj "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vtlevel.xml**