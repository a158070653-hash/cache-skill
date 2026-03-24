# $SORTBEGIN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fsortbegin

---

 

Caché ObjectScript Reference

$SORTBEGIN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortend "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$SORTBEGIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortbegin "$SORTBEGIN") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Initiates a sorting mode to improve performance of multiple sets to a global.

Synopsis

$SORTBEGIN(set\_global)

Parameter

set\_global

A global variable name.

Description

$SORTBEGIN initiates a special sorting mode during which SET operations to the specified target global are redirected to a process-private temporary area and sorted into subsets. This mode is ended with a call to $SORTEND which copies the data into the target global reference. When the special sorting mode is in effect, all sets to the target global reference and any of its descendants are affected.

$SORTBEGIN is designed to help the performance of operations, such as index building, where a large amount of unordered data needs to be written to a global. When the amount of data written approaches or exceeds the amount of available buffer pool memory, performance can suffer drastically. $SORTBEGIN solves this problem by guaranteeing that data is written to the target global in sequential order, thus minimizing the number of physical disk accesses needed. It does this by writing and sorting data into one or more temporary buffers (using space in the ^CacheTemp global if needed) and then, when $SORTEND is called, copying the data sequentially into the target global.

While $SORTBEGIN is in effect, data read from the target global will not reflect any SET operations. You cannot use $SORTBEGIN in cases where you need to read global values from the same global in which you are inserting values.

Caché object and Caché SQL applications automatically make use of $SORTBEGIN for index and temporary index creation.

The $SORTBEGIN sorting mode can be terminated without writing data to the target global by calling $SORTEND with it optional second parameter set to 0.

If successful, $SORTBEGIN returns a nonzero integer value. If unsuccessful, $SORTBEGIN returns zero.

Sorting Mode Errors

Invoking some operations between the $SORTBEGIN and $SORTEND result in Caché issuing an error code:

*   If the mapping of the namespace of set\_global is changed between $SORTBEGIN and $SORTEND, a <NAMESPACE> error occurs when you invoke $SORTEND. However, if $SORTBEGIN specifies set\_global with an [implied namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace), subsequent namespace mapping changes have no effect on $SORTEND. Global references with implied namespace and global references with explicit namespaces should not be mixed in the same sort operation. For information on modifying namespaces, see [Configuring Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_config#GSA_config_namespace) in the Caché System Administration Guide.
    
*   If you establish a $SORTBEGIN global, and then issue a $SORTBEGIN for an ancestor or descendent of that global, Caché issues a <DUPLICATEARG> error. For example, if you invoke $SORTBEGIN(^test(1,2,3)), the following function calls result in a <DUPLICATEARG> error: $SORTBEGIN(^test(1,2)) or $SORTBEGIN(^test(1,2,3,4)).
    

See Also

*   [$SORTEND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortend) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortend "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fsortbegin.xml**