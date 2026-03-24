# $PREFETCHON

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fprefetchon

---

 

Caché ObjectScript Reference

$PREFETCHON

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchoff "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$PREFETCHON](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchon "$PREFETCHON") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Establishes pre-fetch for specified globals.

Synopsis

$PREFETCHON(gref,gref2)

Parameters

gref

A global reference.

gref2

Optional — A global reference used to establish a range.

Description

$PREFETCHON improves performance by turning on pre-fetching for a global or a range of globals. $PREFETCHON returns 1 indicating successful completion (pre-fetching is enabled). $PREFETCHON returns 0 indicating the desired pre-fetch could not be established. A 0 might be returned if the specified range includes two different global names, or if there is some other problem that prevents pre-fetching. A returned 0 is not an error; it does not interrupt program execution, and processing of global references in the specified range is not impaired. It simply means that these global operations do not have the performance boost of pre-fetching.

Note:

Pre-fetching of globals is not supported on a remote database.

$PREFETCHOFF turns off pre-fetching.

There are two forms of $PREFETCHON:

*   $PREFETCHON(gref) pre-fetches the gref node and all of its descendents. For example, $PREFETCHON(^abc(4)) pre-fetches all of the descendents of ^abc(4), such as ^abc(4,1), ^abc(4,2,2), and so forth. It does not pre-fetch ^abc(5).
    
*   $PREFETCHON(gref,gref2) pre-fetches the nodes in the range gref through gref2. This does not include the descendents of gref2. gref and gref2 must be nodes of the same global. For example, $PREFETCHON(^abc(4),^abc(7,5)) pre-fetches all of the global nodes in the range of ^abc(4) through ^abc(7,5), including ^abc(4,2,2), ^abc(5), and ^abc(7,1,2). However, it does not pre-fetch ^abc(7,5,1).
    

Pre-fetching is not restricted to read access; it also works well when a large number of SET operations are being performed.

Pre-fetching and Performance

When you invoke $PREFETCHON, one or more pre-fetch background processes (daemons) are started as required. These pre-fetch daemons are shared systemwide by all processes. Because each pre-fetch daemon processes only one pre-fetch request at a time, it is usually advantageous to have several pre-fetch daemons running on your system. However, large numbers of concurrent pre-fetch daemons can have a performance impact on interactive system access.

Pre-fetching can improve performance when running an application that reads a large number of disk blocks containing nodes from the same global tree. Pre-fetching is most efficient when:

*   Data is accessed in generally ascending order, meaning that data blocks of the global tree are generally accessed in left-to-right order. However, there is no requirement for adhering strictly to ascending order. Pre-fetching works best when either the data blocks of the global tree are generally accessed in left-to-right order, or when at least one data block within the range is likely to be accessed prior to most of its neighbors to the right in the logical tree.
    
*   Most of the data blocks in the specified range are accessed. However, the initial pre-fetch does not fetch as many blocks as subsequent fetches, in case the user decides to cancel the operation after accessing only a small portion of the data.
    
*   Less than 100 pre-fetches are active at any given time.
    

Parameters

gref

A global reference, either a global or a process-private global. The global does not need to be defined at the time that the pre-fetch is established.

You can specify this global using @ indirection. Refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

You cannot specify a [structured system variable name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SVARIABLES) (SSVN) for this parameter.

gref2

A global reference used to establish a range with gref. Therefore, gref2 must be a global node lower in the same global tree as gref.

You can specify this global using @ indirection. Refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

Examples

The following example establishes pre-fetch for the global ^a.

  SET ^a\="myglobal"
  SET x\=^a
  SET ret\=$PREFETCHON(^a)
  IF ret\=1 { WRITE !,"prefetch established" }
  ELSE { WRITE !,"prefetch not established" }
  SET ret\=$PREFETCHOFF()

 

The following example establishes pre-fetch for the range of globals ^||a(1) through ^a||(50).

  SET ret\=$PREFETCHON(^||a(1),^||a(50))
  IF ret\=1 { WRITE !,"prefetch established" }
  ELSE { WRITE !,"prefetch not established" }

 

See Also

*   [$PREFETCHOFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchoff) function
    
*   [Globals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchoff "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fprefetchon.xml**