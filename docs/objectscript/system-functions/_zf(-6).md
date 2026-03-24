# $ZF( 6)

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzf-6

---

 

Caché ObjectScript Reference

$ZF(-6)

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5 "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fziswide "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZF(-6)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-6 "$ZF(-6)") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes a DLL function indexed using $ZF(-4).

Synopsis

$ZF(-6,dll\_index,func\_ID,args)

Parameters

dll\_index

A user-specified index to a DLL filename in the DLL index tables, from $ZF(-4).

func\_ID

Optional — The ID value of the function within the DLL as supplied by $ZF(-4). If omitted, call verifies the validity of DLL\_index, loads the image, and returns the image location.

args

Optional — The argument(s) to pass to the function, if any, specified as a comma-separated list.

Description

$ZF(-6) provides a fast Dynamic Link Library (DLL) function interface using a user-defined index for a DLL filename. You establish this user-defined index in $ZF(-4) by assigning an integer (dll\_index) to uniquely associate with a dll\_name. You can place this entry in either a process DLL index table, or a system DLL index table.

Both $ZF(-5) and $ZF(-6) can be used to execute a function from a DLL. which has been located by $ZF(-4).

For a detailed description of how to use $ZF(-6), refer to “[Using $ZF(-6) to Access Libraries by User Index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_dynamic#BGCL_dynamic_index)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).

See Also

*   [$ZF(-3)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3) function
    
*   [$ZF(-4)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-4) function
    
*   [$ZF(-5)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5) function
    
*   [Using $ZF(-6) to Access Libraries by User Index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_dynamic#BGCL_dynamic_index) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5 "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fziswide "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzf-6.xml**