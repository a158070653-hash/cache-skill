# $PREFETCHOFF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fprefetchoff

---

 

Caché ObjectScript Reference

$PREFETCHOFF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchon "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$PREFETCHOFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchoff "$PREFETCHOFF") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Ends pre-fetching of globals.

Synopsis

$PREFETCHOFF(gref,gref2)

Parameters

gref

Optional — A global reference.

gref2

Optional — A global reference used to establish a range.

Description

$PREFETCHOFF turns off the pre-fetching of global nodes established by $PREFETCHON.

There are three forms of $PREFETCHOFF:

*   $PREFETCHOFF() turns off all pre-fetching established for the current process.
    
*   $PREFETCHOFF(gref) turns off pre-fetching of the gref node and all of its descendents. The gref value must correspond exactly to the $PREFETCHON value.
    
*   $PREFETCHOFF(gref,gref2) turns off pre-fetching of the nodes in the range gref through gref2. gref and gref2 must be nodes of the same global. The gref and gref2 values must correspond exactly to the $PREFETCHON values. You cannot turn off part of a range of values.
    

Upon successful completion, $PREFETCHOFF() returns 0. It returns 0 even if there were no pre-fetches to turn off.

Upon successful completion, $PREFETCHOFF(gref) and $PREFETCHOFF(gref,gref2) return a string of six integers separated by commas. These six values are: number of blocks prefetched, number of I/Os performed, number of prefetch operations, milliseconds of prefetch disk time, background job: number of blocks prefetched, and background job: number of I/Os performed.

Upon failure, all forms of $PREFETCHOFF return -1. $PREFETCHOFF(gref) and $PREFETCHOFF(gref,gref2) return -1 if there is no corresponding $PREFETCHON that exactly matches the specified global or range of globals, or if the specified prefetch global or range of globals has already been turned off.

Parameters

gref

A global reference, either a global or a process-private global. The global does not need to be defined at the time that the pre-fetch is turned off.

You can specify this global using @ indirection. Refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

You cannot specify a [structured system variable name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SVARIABLES) (SSVN) for this parameter.

gref2

A global reference used to establish a range with gref. Therefore, gref2 must be a global node lower in the same global tree as gref.

You can specify this global using @ indirection. Refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

Examples

The following example establishes two pre-fetches, then turns them off individually:

  SET ret\=$PREFETCHON(^a)
  IF ret\=1 { WRITE !,"prefetch established" }
  ELSE { WRITE !,"prefetch not established" }
  SET ret2\=$PREFETCHON(^b)
  IF ret2\=1 { WRITE !,"prefetch established" }
  ELSE { WRITE !,"prefetch not established" }
  SET retoff\=$PREFETCHOFF(^a)
  IF retoff'=\-1 { WRITE !,"prefetch turned off. Values:",retoff }
  ELSE { WRITE !,"prefetch not turned off" }
  SET retoff2\=$PREFETCHOFF(^b)
  IF retoff2'=\-1 { WRITE !,"prefetch turned off. Values:",retoff2 }
  ELSE { WRITE !,"prefetch not turned off" }

 

The following example establishes two pre-fetches, then turns off all pre-fetches for the current process:

  SET ret\=$PREFETCHON(^a)
  IF ret\=1 { WRITE !,"prefetch established" }
  ELSE { WRITE !,"prefetch not established" }
  SET ret2\=$PREFETCHON(^b)
  IF ret2\=1 { WRITE !,"prefetch established" }
  ELSE { WRITE !,"prefetch not established" }
  SET retoff\=$PREFETCHOFF()
  IF retoff\=0 { WRITE !,"all prefetches turned off" }
  ELSE { WRITE !,"prefetch not turned off" }

 

See Also

*   [$PREFETCHON](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchon) function
    
*   [Globals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchon "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fprefetchoff.xml**