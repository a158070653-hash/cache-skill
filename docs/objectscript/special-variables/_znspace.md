# $ZNSPACE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vznspace

---

 

Caché ObjectScript Reference

$ZNSPACE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzname "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzorder "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vznspace "$ZNSPACE") \]

Go to: Description Setting the Current Namespace Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the current namespace name.

Synopsis

$ZNSPACE

Description

$ZNSPACE contains the name of the current namespace. By setting $ZNSPACE, you can change the current namespace.

To obtain the current namespace name:

   WRITE $ZNSPACE

 

You can also obtain the name of the current namespace by invoking the [NameSpace()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS#NameSpace) method of [%SYSTEM.SYS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS) class, as follows:

   WRITE $SYSTEM.SYS.NameSpace()

 

You can test whether a namespace is defined by using the [Exists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace#Exists) method of the [%SYS.Namespace](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace) class, as follows:

   WRITE ##class(%SYS.Namespace).Exists("USER"),!  ; an existing namespace
   WRITE ##class(%SYS.Namespace).Exists("LOSER")   ; a non-existent namespace

 

These methods are described in the InterSystems Class Reference.

For UNIX® and OpenVMS, the default namespace is established as a System Configuration option. For Windows systems, it is set using a command line start-up option.

Namespace names are not case-sensitive. Caché always displays explicit namespace names in all uppercase letters, and implied namespace names in all lowercase letters.

To obtain the namespace name of a specified process, use a method of the SYS.Process class, as shown in the following example:

   ZNSPACE "%SYS"
   WRITE ##class(SYS.Process).%OpenId($JOB).NameSpaceGet()

 

This example requires that UnknownUser have assigned the %DB\_CACHESYS role.

Setting the Current Namespace

You can change the current namespace using a ZNSPACE command, a SET $NAMESPACE, or a SET $ZNSPACE. SET $ZNSPACE is functionally identical to the ZNSPACE command.

Note:

When you wish to temporarily change the current namespace, perform some operation, then revert to the prior namespace, use SET $NAMESPACE, rather than SET $ZNSPACE. Because $NAMESPACE permits you to NEW $NAMESPACE, it reverts to the original namespace when either the subroutine completes or an unexpected error occurs. See [$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace) special variable for details.

You can use the SET command to change the value of $ZNSPACE. This sets the current namespace for the process. Specify the new namespace as a string literal or a variable or expression that evaluates to a quoted string. You can specify an explicit namespace ("namespace") or an implied namespace ("^system^dir" or "^^dir").

If you specify the current namespace, SET $ZNSPACE performs no operation and returns no error. If you specify an undefined namespace, SET $ZNSPACE generates a <NAMESPACE> error.

You cannot NEW the $ZNSPACE special variable.

Example

In the following example, if the current namespace is not USER, a SET $ZNSPACE command changes the current namespace to USER. Note that because of the IF test, the namespace must be specified in all uppercase letters.

   SET ns\="USER"
   IF $ZNSPACE\=ns {
     WRITE !,"Namespace already was ",$ZNSPACE }
   ELSEIF 1\=##class(%SYS.Namespace).Exists(ns) {
     WRITE !,"Namespace was ",$ZNSPACE
     SET $ZNSPACE\=ns
     WRITE !,"Set namespace to ",$ZNSPACE }
   ELSE { WRITE !,ns," is not a defined namespace" }
   QUIT

 

This example requires that UnknownUser have assigned the %DB\_CACHESYS and the %DB\_USER roles.

See Also

*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace) command
    
*   [$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace) special variable
    
*   [Configuring Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_config#GSA_config_namespace) in Caché System Administration Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzname "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzorder "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vznspace.xml**