# $ZF( 3)

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzf-3

---

 

Caché ObjectScript Reference

$ZF(-3)

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2 "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-4 "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZF(-3)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3 "$ZF(-3)") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Loads a Dynamic-Link Library (DLL) and executes a library function.

Synopsis

$ZF(-3,dll\_name,func\_name,args)

Parameters

dll\_name

The name of the dynamic-link library (DLL) to load, specified as a quoted string. When a DLL is already loaded, dll\_name can be specified as a null string ("").

func\_name

Optional — The name of the function to execute within the DLL, specified as a quoted string.

args

Optional — A comma-separated list of arguments to pass to the function.

Description

Use $ZF(-3) to load a Dynamic-Link Library (DLL) and execute the specified function from that DLL. $ZF(-3) returns the function’s return value.

$ZF(-3) can be invoked in any of the following ways:

To just load a DLL:

   SET x\=$ZF(\-3,"mydll")

To load a DLL and execute a function located in that DLL:

   SET x\=$ZF(\-3,"mydll","$$myfunc",1)

Loading a DLL using $ZF(-3) makes it the current DLL, and automatically unloads the DLL loaded by a previous invocation of $ZF(-3).

To execute a function located in a DLL loaded by a previous $ZF(-3), you can speed execution by specifying the current DLL using the null string, as follows:

   SET x\=$ZF(\-3,"","$$myfunc2",1)

To explicitly unload the current DLL (loaded by a previous $ZF(-3) call):

   SET x\=$ZF(\-3,"")

$ZF(-3) can load only one DLL. Loading a DLL unloads the previous DLL. You can also explicitly unload the currently loaded DLL, which would result in no currently loaded DLL. (However, note that $ZF(-3) loads and unloads do not affect loads and unloads for use with $ZF(-5) or $ZF(-6), as described below.)

The DLL name specified can be a full pathname, or a partial pathname. If you specify a partial pathname, Caché canonicalizes it to the current directory. Generally, DLLs are stored in the binary directory (“bindir”). To locate the binary directory, call the [BinaryDirectory()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#BinaryDirectory) method of the [%SYSTEM.Util](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util) class. For further details, refer to the InterSystems Class Reference.

Notes

Dynamic-Link Libraries

A DLL is a binary library that contains routines that can be loaded and called at runtime. When a DLL is loaded, Caché finds a function named GetZFTable() within it. If GetZFTable() is present, it returns a pointer to a table of the functions located in the DLL. Using this table, $ZF(-3) calls the specified function from the DLL.

Loading Multiple DLLs

Calls to $ZF(-3) can only load one DLL at a time; loading a DLL unloads the previous DLL. To load multiple DLLs concurrently, execute DLL functions with $ZF(-5) or $ZF(-6). Loading or unloading a DLL using $ZF(-3) has no effect on DLLs loaded for use with $ZF(-5) or $ZF(-6).

See Also

*   [$ZF(-5)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5) function
    
*   [$ZF(-6)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-6) function
    
*   [Using $ZF(-3) for Simple Library Function Calls](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_dynamic#BGCL_dynamic_loadone) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2 "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-4 "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzf-3.xml**