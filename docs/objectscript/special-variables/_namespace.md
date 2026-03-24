# $NAMESPACE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vnamespace

---

 

Caché ObjectScript Reference

$NAMESPACE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vkey "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace "$NAMESPACE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the namespace for the current stack level.

Synopsis

$NAMESPACE
SET $NAMESPACE=namespace
NEW $NAMESPACE

Description

$NAMESPACE contains the name of the current namespace for the current stack level. By setting $NAMESPACE you can change the current namespace.

You can also obtain the name of the current namespace by invoking the [NameSpace()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS#NameSpace) method of [%SYSTEM.SYS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS) class, as follows:

   WRITE $SYSTEM.SYS.NameSpace()

 

You can obtain the full pathname of the current namespace by using the [NormalizeDirectory()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.File#NormalizeDirectory) method of [%Library.File](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.File) class, as follows:

   WRITE $NAMESPACE,!
   WRITE ##class(%Library.File).NormalizeDirectory("")

 

You can test whether a namespace is defined by using the [Exists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace#Exists) method of the [%SYS.Namespace](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace) class, as follows:

   WRITE ##class(%SYS.Namespace).Exists("USER"),!  ; an existing namespace
   WRITE ##class(%SYS.Namespace).Exists("LOSER")   ; a non-existent namespace

 

These methods are described in the InterSystems Class Reference.

SET $NAMESPACE

You can set $NAMESPACE to an existing namespace using the SET command. When you wish to temporarily change the current namespace, perform some operation, then revert to the prior namespace, use SET $NAMESPACE, rather than SET $ZNSPACE. Because $NAMESPACE permits you to NEW $NAMESPACE, it reverts to the original namespace when either the subroutine completes or an unexpected error occurs.

In SET $NAMESPACE=namespace, specify namespace as a string literal or a variable or expression that evaluates to a quoted string; namespace is not case sensitive. However, Caché always displays explicit namespace names in all uppercase letters, and implied namespace names in all lowercase letters. A namespace name can contain Unicode letter characters; Caché converts accented lowercase letters to their corresponding accented uppercase letters.

The namespace name can be an explicit namespace name ("USER") or an implied namespace ("^^c:\\InterSystems\\Cache\\mgr\\user\\"). For further details on implied namespaces, refer to the [ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace) command.

If the specified namespace does not exist, SET $NAMESPACE generates a <NAMESPACE> error. If you do not have access privileges to a namespace, Caché generates a <PROTECT> error, followed by the database path. For example, the %Developer role does not have access privileges to the %SYS namespace. If you have this role and attempt to access this namespace, Caché issues the following error (on a Windows system): <PROTECT> \*c:\\intersystems\\cache\\mgr\\.

NEW $NAMESPACE

Before using SET $NAMESPACE to change the namespace, use NEW $NAMESPACE to stack the current namespace. Quitting a routine or branching to an error trap reverts to this stacked namespace. If you create an object instance in the changed namespace, Caché closes the object in that namespace before reverting to the stacked namespace. This is shown in the following Terminal example:

USER>NEW $NAMESPACE
USER 1S1>NEW myoref
USER 2N1>SET $NAMESPACE="SAMPLES"
SAMPLES 2N1>SET myoref=##class(%SQL.Statement).%New()
SAMPLES 2N1>QUIT
  /\* Cache closes myoref in the SAMPLES namespace
     Then reverts to the USER namespace \*/
USER>

In more complex stacked namespace situations, it is the programmer’s responsibility to explicitly close objects in the proper namespace.

Examples

The following example calls a routine that executes in a different namespace than the calling program. It uses NEW $NAMESPACE to stack the current namespace. It then uses SET $NAMESPACE to change the namespace for the duration of Test. The QUIT reverts to the stacked namespace:

   WRITE "before: ",$NAMESPACE,!
   DO Test
   WRITE "after: ",$NAMESPACE,!
   QUIT
Test
   NEW $NAMESPACE
   SET $NAMESPACE\="USER"
   WRITE "testing: ",$NAMESPACE,!
   ; routine code
   QUIT

 

There is no need to handle an error to switch back to the old namespace; Caché restores the old namespace when you leave the current stack level.

The following example differs from the previous example by omitting NEW $NAMESPACE. Note that upon QUIT the namespace does not revert:

   WRITE "before: ",$NAMESPACE,!
   DO Test
   WRITE "after: ",$NAMESPACE,!
   QUIT
Test
   NEW
   SET $NAMESPACE\="USER"
   WRITE "testing: ",$NAMESPACE,!
   ; routine code
   QUIT

 

Calling a separate routine when temporarily changing the current namespace is the preferred programming practice. In situations when calling a separate routine is not practical, you can use the legacy [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo_legacy) command dot syntax. The following example temporarily changes the namespace within a large subroutine by using this DO command syntax to create a stack frame:

   WRITE "before: ",$NAMESPACE,!
   DO
   . NEW $NAMESPACE
   . SET $NAMESPACE\="USER"
   . WRITE "testing: ",$NAMESPACE,!
   . ; routine code
   WRITE "after: ",$NAMESPACE,!

 

See Also

*   [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command
    
*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace) command
    
*   [$ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vznspace) special variable
    
*   [Configuring Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_config) in Caché System Administration Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vkey "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vnamespace.xml**