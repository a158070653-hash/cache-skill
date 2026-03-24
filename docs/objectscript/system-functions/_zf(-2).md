# $ZF( 2)

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzf-2

---

 

Caché ObjectScript Reference

$ZF(-2)

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1 "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3 "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZF(-2)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2 "$ZF(-2)") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes an operating system command as a spawned child process and returns immediately.

Synopsis

$ZF(-2,oscommand)

Parameter

oscommand

Optional — The command or program to be executed as a child process, specified as a quoted string. If you omit oscommand, $ZF(-2) launches the operating system shell.

Description

$ZF(-2) permits a Caché process to invoke a command of the host operating system. $ZF(-2) executes the operating system command specified in oscommand as a spawned child process from the current console. It returns immediately after spawning the child process and does not wait for the process to terminate. Input and output devices default to the null device.

$ZF(-2) does not return the child process exit status. Instead, if the child process was created successfully,$ZF(-2) returns either 0 (on most platforms) or 1 (on OpenVMS platforms). $ZF(-2) returns -1 if a child process could not be forked.

Because $ZF(-2) does not wait for a response from the spawned child process, you can shut down Caché while the child process is executing.

$ZF(-2) closes the parent process principal device (specified in $PRINCIPAL) before executing the operating system command. This is done because the child process executes concurrently with the parent. If $ZF(-2) did not close $PRINCIPAL, output from the parent and the child would become intermingled. When using $ZF(-2) you should redirect I/O in the command if you wish to recover output from the child process. For example:

   SET x\=$ZF(\-2,"ls -l > mydir.txt")

$ZF(-2) with no specified parameters launches the default operating system shell. For further details, see “[Issuing Operating System Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).

If a pathname supplied in oscommand contains a space character, pathname handling is platform-dependent:

*   OpenVMS permits space characters in pathnames; no special processing is required.
    
*   Windows and UNIX® permit space characters in pathnames, but the entire pathname containing spaces must be enclosed in an additional set of double quote (") characters. This is in accordance with the Windows cmd /c statement. For further details, specify cmd /? at the Windows command prompt.
    

You can use the [NormalizeFilenameWithSpaces()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.File#NormalizeFilenameWithSpaces) method of the [%Library.File](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.File) class to handle spaces in pathnames as appropriate for the host platform.

$ZF(-2) is a privileged operation, which requires the %System\_Callout:U privilege. See “[Adding the %System\_Callout:USE Privilege](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall#BGCL_syscall_privilege)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface) for details.

$ZF(-1) and $ZF(-2)

These two functions are in most respects identical. They differ in the following way:

*   [$ZF(-1)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1) suspends execution of the current process while awaiting completion of the spawned child process. It receives status information from the spawned process, which it returns as an exit status code (an integer value) when the spawned process completes.
    
*   $ZF(-2) does not suspend execution of the current process. It immediately returns a status value upon spawning the child process. Because it does not await completion of the spawned child process it cannot receive status information from that process.
    

See Also

*   [$ZF(-1)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1) function
    
*   [$PRINCIPAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal) special variable
    
*   [Issuing Operating System Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    
*   [Adding the %System\_Callout:USE Privilege](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall#BGCL_syscall_privilege) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1 "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3 "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzf-2.xml**