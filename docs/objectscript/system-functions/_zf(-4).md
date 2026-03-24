# $ZF( 4)

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzf-4

---

 

Caché ObjectScript Reference

$ZF(-4)

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3 "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5 "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZF(-4)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-4 "$ZF(-4)") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Provides utility functions used with $ZF(-5) and $ZF(-6).

Synopsis

$ZF(-4,1,dll\_name)

$ZF(-4,n,dll\_id,func\_name)

$ZF(-4,n,dll\_id,decr\_flag)

$ZF(-4,n,dll\_index,dll\_name)

$ZF(-4,n,dll\_index,decr\_flag)

Parameters

n

A code for the type of operation to perform: 1=load DLL by name. 2=unload DLL by id. 3=look up function in DLL by id. 4=unload DLL by index. 5=create an entry in the system DLL index table. 6=delete an entry in the system DLL index table. 7=create an entry in the process DLL index table. 8=delete an entry in the process DLL index table.

dll\_name

The name of the dynamic-link library (DLL). Used with n\=1, 5, or 7.

dll\_id

The id value of a loaded dynamic-link library (DLL). Used with n\=2, or 3.

dll\_index

A user-defined index to a dynamic-link library (DLL) in a DLL index table. Must be a unique, positive, nonzero integer. The numbers 1024 through 2047 are reserved for system use. Used with n\=4, 5, 6, 7, or 8.

func\_name

The name of the function to look up within the DLL. Used only when n\=3.

decr\_flag

Optional — A flag for decrementing the DLL reference count. Used with n\=2 or 4.

Description

$ZF(-4) can be used to establish an ID value for a DLL or for a function within a DLL. These ID values are used by $ZF(-5) to execute a function.

$ZF(-4) can be used to establish an index to a DLL index table. These index values are used by $ZF(-6) to execute a function.

*   You can explicitly load shared libraries using $ZF(-4,1), which loads a library and returns a handle that can be used to access library functions with $ZF(-5).
    
*   You can explicitly load a single shared library using $ZF(-3), which loads a single active library and invokes its methods.
    
*   You can implicitly load shared libraries using $ZF(-6), after indexing a library with $ZF(-4,5) or $ZF(-4,7).
    

Establishing ID Values

To load a DLL and return its ID, use the following syntax:

dll\_id=$ZF(-4,1,dll\_name)

To look up a function from a DLL loaded by $ZF(-4,1), and return an ID for that function, use the following syntax:

func\_id=$ZF(-4,3,dll\_id,func\_name)

To execute a function located by $ZF(-4,3), use $ZF(-5).

To unload a specific DLL loaded by $ZF(-4,1), use the following syntax:

$ZF(-4,2,dll\_id)

To unload all DLLs loaded by $ZF(-4,1), use the following syntax:

$ZF(-4,2)

Increment and Decrement DLL Loads

When two classes have loaded the same library, the library will be unloaded by the first call to $ZF(-4,2,dll\_id) or $ZF(-4,4,dll\_index). This can leave the other class stranded without access to the library. For this reason, Caché supports a reference count on each DLL. Caché maintains a reference count of the number of times a library is loaded with $ZF(-4,1,dll\_name). Each call to $ZF(-4,1,dll\_name) increases the reference count.

$ZF(-4,2) provides an optional decrement flag argument, decr\_flag. Each call to $ZF(-4,2,dll\_id,1) decrements the reference count by 1. A call to $ZF(-4,2,dll\_id,1) unloads the library if the reference count goes to zero. A call to $ZF(-4,2,dll\_id) (or $ZF(-4,2,dll\_id,0)) ignores the reference count and unloads the library immediately.

A call to $ZF(-4,5) or $ZF(-4,7) establishes a library index. Subsequent calls to $ZF(-6) to execute a function implicitly loads the library and increment the reference count. Each call to $ZF(-4,4,dll\_index,1) decrements this reference count by 1.

The reference count interactions between reference counts established by dll\_name and dll\_index are as follows:

*   Libraries loaded with $ZF(-4,1,dll\_name) are not unloaded by a call to $ZF(-4,4,dll\_index,1) unless the reference count is zero.
    
*   Libraries loaded with $ZF(-4,1,dll\_name) are immediately unloaded by either $ZF(-4,2,dll\_id) or $ZF(-4,4,dll\_index) (with no decrement flag argument) with no regard to the reference count.
    
*   Libraries loaded implicitly with $ZF(-6) are not unloaded by $ZF(-4,2,dll\_id,1), even if the reference count goes to zero; they can only be unloaded by $ZF(-4,4,dll\_index,1).
    
*   Libraries loaded implicitly with $ZF(-6) are immediately unloaded by either $ZF(-4,2,dll\_id) or $ZF(-4,4,dll\_index) (with no decrement flag argument) with no regard to the reference count.
    

$ZF(-4,2) with no dll\_id argument unloads all libraries immediately, without regard to the reference count, or whether they were loaded with $ZF(-4,1,dll\_name) or implicitly with $ZF(-6).

Establishing Index Values

To index a DLL in the system DLL index table, use the following syntax:

$ZF(-4,5,dll\_index,dll\_name)

To index a DLL in the process DLL index table, use the following syntax:

$ZF(-4,7,dll\_index,dll\_name)

To look up and execute a function indexed by $ZF(-4,5) or $ZF(-4,7), use $ZF(-6).

To unload an indexed DLL, use the following syntax:

$ZF(-4,4,dll\_index)

To delete an index entry in the system DLL index table, use the following syntax:

$ZF(-4,6,dll\_index)

To delete an index entry in the process DLL index table, use the following syntax:

$ZF(-4,8,dll\_index)

To delete all index entries in the process DLL index table, use the following syntax:

$ZF(-4,8)

For a detailed description of how to use $ZF(-4) and $ZF(-5), refer to “[Using $ZF(-5) to Access Libraries by System ID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_dynamic#BGCL_dynamic_libid)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).

For a detailed description of how to use $ZF(-4) and $ZF(-6), refer to “[Using $ZF(-6) to Access Libraries by User Index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_dynamic#BGCL_dynamic_index)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).

See Also

*   [$ZF(-3)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3) function
    
*   [$ZF(-5)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5) function
    
*   [$ZF(-6)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-6) function
    
*   [Using $ZF(-5) to Access Libraries by System ID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_dynamic#BGCL_dynamic_libid) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    
*   [Using $ZF(-6) to Access Libraries by User Index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_dynamic#BGCL_dynamic_index) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3 "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5 "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzf-4.xml**