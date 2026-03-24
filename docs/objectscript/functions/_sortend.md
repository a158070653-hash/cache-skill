# $SORTEND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fsortend

---

 

Caché ObjectScript Reference

$SORTEND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortbegin "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$SORTEND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortend "$SORTEND") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Concludes the sorting mode initiated by $SORTBEGIN.

Synopsis

$SORTEND(set\_global,dosort)

Parameters

set\_global

Optional — A global variable that was specified in a corresponding $SORTBEGIN. If omitted, $SORTEND concludes all $SORTBEGIN operations for the current process.

dosort

Optional — A flag parameter. If 1, Caché performs the sort operation initiated by $SORTBEGIN and copies the sorted data into set\_global. If 0, Caché terminates the sort operation without copying any data. The default is 1.

Description

$SORTEND specifies the end of a special sorting mode initiated by $SORTBEGIN on a specific target global. The value of the $SORTEND set\_global must match the corresponding $SORTBEGIN set\_global.

If you omit set\_global, $SORTEND ends all current sorting modes initiated by all active $SORTBEGIN functions for the current process. Therefore, $SORTEND() or $SORTEND(,1) end and commit all current sorting modes for the process; $SORTEND(,0) aborts all current sorting modes for the process.

*   If successful, $SORTEND returns a positive integer count of the total number of global nodes set. When set\_global is specified, this is the number of sets applied to the specified set\_global variable. When set\_global is omitted, this is the number of sets applied to all current $SORTBEGIN set\_global variables. This integer count is returned regardless of the dosort flag setting.
    
*   If unsuccessful, $SORTEND returns -1. For example, if $SORTEND specifies a set\_global that does not have a corresponding active $SORTBEGIN.
    
*   If no-op, $SORTEND returns 0. This can occur if you there are no sets applied to the specified set\_global variable, or if there is no active $SORTBEGIN when you issue a $SORTEND that does not specify a set\_global.
    

If the mapping of the namespace of set\_global is changed between $SORTBEGIN and $SORTEND, a <NAMESPACE> error occurs when you invoke $SORTEND. However, if $SORTBEGIN specifies set\_global with an [implied namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace), subsequent namespace mapping changes have no effect on $SORTEND. Global references with implied namespace and global references with explicit namespaces should not be mixed in the same sort operation. For information on modifying namespaces, see [Configuring Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_config#GSA_config_namespace) in the Caché System Administration Guide.

Examples

The following example applies three sets to the global ^myyestest. $SORTEND returns 3. Because dosort is 1, these sets are applied, as shown by the $DATA function return values:

  WRITE $SORTBEGIN(^myyestest),!
    SET ^myyestest(1)\="apple"
    SET ^myyestest(2)\="orange"
    SET ^myyestest(3)\="banana"
  WRITE $SORTEND(^myyestest,1),!
  WRITE $DATA(^myyestest(1)),!
  WRITE $DATA(^myyestest(2)),!
  WRITE $DATA(^myyestest(3))
  KILL ^myyestest

 

The following example applies three sets to the global ^mynotest. $SORTEND returns 3. Because dosort is 0, these sets are not applied, as shown by the $DATA function return values:

  WRITE $SORTBEGIN(^mynotest),!
    SET ^mynotest(1)\="apple"
    SET ^mynotest(2)\="orange"
    SET ^mynotest(3)\="banana"
  WRITE $SORTEND(^mynotest,0),!
  WRITE $DATA(^mynotest(1)),!
  WRITE $DATA(^mynotest(2)),!
  WRITE $DATA(^mynotest(3))
  KILL ^mynotest

 

The following two examples specify two $SORTBEGIN operations, and within them apply three sets to the global ^mytesta and two sets to the global ^mytestb. $SORTEND does not specify a set\_global, and therefore ends all current $SORTBEGIN operations and returns 5. Note that in both examples $SORTEND returns 5, though the first example commits these sets and the second example aborts these sets.

  WRITE $SORTBEGIN(^mytesta),!
    SET ^mytesta(1)\="apple"
    SET ^mytesta(2)\="orange"
    SET ^mytesta(3)\="banana"
  WRITE $SORTBEGIN(^mytestb),!
    SET ^mytestb(1)\="corn"
    SET ^mytestb(2)\="carrot"
  WRITE "$SORTEND returns: ",$SORTEND(,1),!
    WRITE "global sets committed?: ",$DATA(^mytesta(2))
  KILL ^mytesta,^mytestb

 

  WRITE $SORTBEGIN(^mytesta),!
    SET ^mytesta(1)\="apple"
    SET ^mytesta(2)\="orange"
    SET ^mytesta(3)\="banana"
  WRITE $SORTBEGIN(^mytestb),!
    SET ^mytestb(1)\="corn"
    SET ^mytestb(2)\="carrot"
  WRITE "$SORTEND returns: ",$SORTEND(,0),!
    WRITE "global sets committed?: ",$DATA(^mytesta(2))
  KILL ^mytesta,^mytestb

 

See Also

*   [$SORTBEGIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortbegin) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortbegin "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fsortend.xml**