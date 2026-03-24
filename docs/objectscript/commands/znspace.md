# ZNSPACE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cznspace

---

 

Caché ObjectScript Reference

ZNSPACE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czkill "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace "ZNSPACE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Sets the current namespace.

Synopsis

ZNSPACE:pc nspace
ZN:pc nspace

Arguments

pc

Optional — A postconditional expression.

nspace

A string expression that evaluates to the name of an existing namespace.

Description

ZNSPACE nspace changes the current namespace to the nspace value. nspace can be an explicit namespace or an implied namespace.

If nspace does not exist, Caché generates a <NAMESPACE> error. If you do not have access privileges to a namespace, Caché generates a <PROTECT> error, followed by the database path. For example, the %Developer role does not have access privileges to the %SYS namespace. If you have this role and attempt to access this namespace, Caché issues the following error (on a Windows system): <PROTECT> \*c:\\intersystems\\cache\\mgr\\.

You can change the current namespace by using the ZNSPACE command, the %CD utility (DO ^%CD), or by setting the $NAMESPACE or $ZNSPACE special variables. Use of ZNSPACE or %CD is preferable, because these provide more extensive error checking.

When you wish to temporarily change the current namespace, perform some operation, then revert to the prior namespace, use SET $NAMESPACE. Because $NAMESPACE permits you to NEW $NAMESPACE, it reverts to the original namespace when either the subroutine completes or an unexpected error occurs. See [$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace) special variable for details.

The following methods may assist you when using ZNSPACE:

*   To return the name of the current namespace: return the $NAMESPACE or $ZNSPACE special variable value, or invoke the [NameSpace()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS#NameSpace) method of the [%SYSTEM.SYS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS) class, as follows:
    
       WRITE $SYSTEM.SYS.NameSpace()
    
     
    
*   To list all namespaces available to the current process: invoke the [ListAll()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace#ListAll) method of the [%SYS.Namespace](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace) class, as follows:
    
       DO ##class(%SYS.Namespace).ListAll(.result)
       ZWRITE result
    
     
    
    When ListAll() lists an implied namespace, it delimits the system name using caret (^) delimiters.
    
    Or invoke the [List](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace#List) query of the [%SYS.Namespace](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace) class, as follows:
    
      DO ##class(%ResultSet).RunQuery("%SYS.Namespace","List")
    
     
    
    Note that both of these listings list all namespaces, including those for which the user does not have access privileges.
    
*   To test whether a namespace is defined: use the [Exists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace#Exists) method of [%SYS.Namespace](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace) class, as follows:
    
       WRITE ##class(%SYS.Namespace).Exists("USER"),!  ; an existing namespace
       WRITE ##class(%SYS.Namespace).Exists("LOSER")   ; a non-existent namespace
    
     
    

These methods are described in the InterSystems Class Reference.

For UNIX® and OpenVMS, the system-wide default namespace is established as a System Configuration option. For Windows systems, it is set using a command line start-up option.

For namespace naming conventions and namespace name translation, see [Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_namespaces) in the “Syntax Rules” chapter of Using Caché ObjectScript. For information on using namespaces, see [Namespaces and Databases](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_ch_enviro) in the Caché Programming Orientation Guide. For information on creating and modifying namespaces, see [Configuring Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_config#GSA_config_namespace) in the Caché System Administration Guide.

Arguments

pc

Optional — An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

nspace

Any valid string expression that evaluates to the name of the new namespace. nspace can be an explicit namespace or an implied namespace.

Namespace names are not case-sensitive. Caché always displays explicit namespace names in all uppercase letters, and implied namespace names in all lowercase letters.

An implied namespace specifies the namespace by system name and directory. There are two forms:

*   "^^dir" (if the namespace directory is on the current system).
    
*   "^system^dir" (if the namespace directory is on a remote system).
    

For dir, specify a directory path or an OpenVMS file specification. This is shown in the following examples:

Windows example:

  ZNSPACE "^^c:\\InterSystems\\Cache\\mgr\\user\\"
  WRITE $NAMESPACE

Linux example:

  ZNSPACE "^RemoteLinuxSystem^/usr/Cache/mgr/user/"
  WRITE $NAMESPACE

Examples

The following example uses NEW $NAMESPACE to stack the current namespace. It then uses ZNSPACE to change the namespace. The QUIT reverts to the stacked namespace:

   WRITE "before: ",$NAMESPACE,!
   DO Test
   WRITE "after: ",$NAMESPACE,!
   QUIT
Test
   NEW $NAMESPACE
   ZNSPACE "%SYS"
   WRITE "testing: ",$NAMESPACE,!
   QUIT

 

The following example assumes that a namespace called "accounting" already exists. Otherwise, you receive a <NAMESPACE> error.

From the programmer prompt:

USER>ZNSPACE "Accounting"
ACCOUNTING>

By default, as shown in this example, the Caché Terminal prompt displays the current namespace name. Namespace names are always displayed in uppercase letters.

The following example tests for the existence of a namespace, then uses ZNSPACE to set the current namespace and uses the [TerminalPrompt()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#TerminalPrompt) method to set the Caché Terminal prompt either to the specified namespace or to USER:

   WRITE !,"Current namespace is ",$NAMESPACE
   SET ns\="ACCOUNTING"
   IF 1\=##class(%SYS.Namespace).Exists(ns) {
     WRITE !,"Changing namespace to: ",ns
     ZNSPACE ns
     DO ##class(%SYSTEM.Process).TerminalPrompt(2)
     WRITE !,"and ",$NAMESPACE," will display at the prompt"
     }
   ELSE {
     WRITE !,"Namespace ",ns," does not exist"
     SET ns\="USER"
     WRITE !,"Changing namespace to: ",ns
     ZNSPACE ns
     DO ##class(%SYSTEM.Process).TerminalPrompt(2)
     WRITE !,"and ",$NAMESPACE," will display at the prompt"
     }

 

Notes

Namespaces with Default Directories

If the namespace you select has a default directory on a remote machine, ZNSPACE does not change the current directory of your process to that namespace’s directory. Thus, your current namespace becomes the namespace you selected, but your current directory remains the directory that was current before you issued the ZNSPACE command.

Implied Namespace Mapping

ZNSPACE creates additional default mappings from an implied namespace. These mappings are the same as for a normal (explicit) namespace. They allow a process to find and execute the % routines and % globals that are physically located in the CACHESYS and CACHELIB databases (the cache\\mgr\\ and cache\\mgr\\cachelib directories).

Setting the $NAMESPACE or $ZNSPACE special variable or running the %CD routine with an implied namespace is the same as issuing a ZNSPACE command.

% Routine Mapping

When a process switches namespaces using the ZNSPACE command, the system routines path mapping is normally reset. This is true for both a normal (explicit) namespace and an implied namespace. The only exception to this is when the process switches from an implied namespace to an implied namespace, in which case the existing mapping is preserved. For further information on implied namespaces, see [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

You can override this remapping of system routines by using the [SysRoutinePath()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#SysRoutinePath) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. This can be used to override an existing system routine. Commonly, this is used to create an additional mapping when debugging a % routine. The process must have Write permission for the CACHESYS database. This method should be used with extreme caution.

Caution:

Changing the mapping of a system routine supplied by InterSystems is strongly discouraged. Doing so could break current or future library routines and methods supplied by InterSystems.

% Global Mapping

The first time a user uses ZNSPACE (or its equivalent) to go to an implied namespace, Caché creates a mapping for that implied namespace, as follows: Caché first maps to existing % globals in that implied namespace. Caché then maps all other % globals to CACHESYS.

Once this mapping has been created for an implied namespace, the mapping is stored in shared memory. This means that when any subsequent user goes to that implied namespace, Caché uses this pre-existing global mapping.

To update an implied namespace global mapping you must clear this shared memory storage. A system restart is one way to clear shared memory.

The deprecated $ZUTIL(5) function does not create additional default mappings from an implied namespace. Thus a process cannot find and execute % routines and % globals that are physically located in the CACHELIB database. For this reason, ZNSPACE or $NAMESPACE are strongly preferred; existing uses of $ZUTIL(5) should be phased out wherever possible.

Controlling Namespace Display

Terminal Prompt

By default, the Caché Terminal prompt displays the current namespace name. This default is configurable:

Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Startup Settings\]. View and edit the current setting of TerminalPrompt. This also sets the prompt for Telnet windows.

To set this behavior for the current process, use the [TerminalPrompt()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#TerminalPrompt) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [TerminalPrompt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Startup#TerminalPrompt) property of the Config.Startup class.

$NAME and $QUERY Functions

The $NAME and $QUERY functions can return the extended global reference form of a global variable, which includes the namespace name. You can control whether these functions return namespace names as part of the global variable name. You can set this extended global reference switch for the current process using the [RefInKind()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#RefInKind) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [RefInKind](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#RefInKind) property of the Config.Miscellaneous class. For further information on extended global references, see [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

Changing Namespaces within Application Code

Object and SQL code assumes that it is running in a single namespace; hence, changing namespaces with open object instances or SQL cursors can lead to code running incorrectly. Typically, there is no need to explicitly change namespaces, as the various Object, SQL, and CSP servers automatically ensure that application code is run in the correct namespace.

Also, changing namespaces demands a relatively high amount of computing power compared to other commands; if possible, application code should avoid it.

See Also

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace) special variable
    
*   [$ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vznspace) special variable
    
*   [Configuring Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_config#GSA_config_namespace) in Caché System Administration Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czkill "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cznspace.xml**