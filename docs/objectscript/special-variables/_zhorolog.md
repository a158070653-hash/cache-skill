# $ZHOROLOG

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzhorolog

---

 

Caché ObjectScript Reference

$ZHOROLOG

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzio "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZHOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzhorolog "$ZHOROLOG") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the number of seconds elapsed since Caché startup.

Synopsis

$ZHOROLOG
$ZH

Description

$ZHOROLOG contains the number of seconds that have elapsed since the most recent Caché startup. This is a count, which is independent of clock changes and day boundaries. The value is expressed as a floating point number, indicating seconds and fractions of a second. The number of decimal digits is platform-dependent. $ZHOROLOG truncates trailing zeros in this fractional portion.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Note:

Because of a limitation in the Windows operating system, putting your Windows system into hibernate or standby mode may cause $ZHOROLOG to return unpredictable values. This problem does not affect $HOROLOG or $ZTIMESTAMP values.

Examples

This example outputs the current $ZHOROLOG value.

   WRITE $ZHOROLOG

 

returns a value such as: 1036526.244932.

The following example shows how you might use $ZHOROLOG to time events and do benchmarks. This example times an application through 100 executions, then finds the average runtime.

Cycletime
  SET start\=$ZHOROLOG
    FOR i\=1:1:100 { DO Myapp }
  SET end\=$ZHOROLOG
  WRITE !,"Average run was ",(end\-start)/100," seconds."
   QUIT
Myapp
   WRITE !,"executing my application"
  ; application code goes here
   QUIT

 

See Also

*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzio "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzhorolog.xml**