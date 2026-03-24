# $ZF( 1)

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzf-1

---

 

Caché ObjectScript Reference

$ZF(-1)

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2 "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZF(-1)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1 "$ZF(-1)") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes an operating system command as a spawned child process and waits for the child process to return.

Synopsis

Windows and UNIX®:
$ZF(-1,oscommand)

OpenVMS:
$ZF(-1,oscommand,outdev,indev)

Parameters

oscommand

Optional — The command or program to be executed as a child process, specified as a quoted string. If you omit oscommand, $ZF(-1) launches the operating system shell.

outdev

Optional: OpenVMS Only — Output device. Used to specify SYS$OUTPUT for the child process. Uses current SYS$OUTPUT if not specified.

indev

Optional: OpenVMS Only — Input device. Used to specify SYS$INPUT for the child process. Uses current SYS$INPUT if not specified.

Description

$ZF(-1) permits a Caché process to invoke a command of the host operating system. It executes the program or command specified in oscommand as a spawned child process from the current console. It waits for the process to return. It returns the child process exit status.

$ZF(-1) returns the following status codes:

*   It returns 0 if the child process executed successfully.
    
*   It returns a positive integer based on the exit status error code issued by the operating system shell. This integer exit status code value is determined by the host operating system. For example, for most Windows command syntax errors, $ZF(-1) returns 1.
    
*   It returns -1 if the child process could not be forked.
    

Because $ZF(-1) waits for a response from the spawned child process, you cannot successfully shut down Caché while the child process is executing.

$ZF(-1) with no specified parameters launches the default operating system shell. For further details, see “[Issuing Operating System Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).

If a pathname supplied in oscommand contains a space character, pathname handling is platform-dependent:

*   OpenVMS permits space characters in pathnames; no special processing is required.
    
*   Windows and UNIX® permit space characters in pathnames, but the entire pathname containing spaces must be enclosed in an additional set of double quote (") characters. This is in accordance with the Windows cmd /c statement. For further details, specify cmd /? at the Windows command prompt.
    

You can use the [NormalizeFilenameWithSpaces()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.File#NormalizeFilenameWithSpaces) method of the [%Library.File](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.File) class to handle spaces in pathnames as appropriate for the host platform.

$ZF(-1) requires the %System\_Callout:U privilege. See “[Adding the %System\_Callout:USE Privilege](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall#BGCL_syscall_privilege)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface) for details.

If $ZF(-1) is unable to spawn a process, it generates a <FUNCTION> error. On OpenVMS systems this may be caused by the child process trying to use the same SYS$INPUT as the parent process, and can be handled by specifying an indev value. The OpenVMS error code may be seen using the SYSLOG utility.

At the programmer prompt in the Caché Terminal, you can perform operations similar to $ZF(-1) by using an exclamation point (!) or a dollar sign ($) as the first character, followed by the operating system command you wish to execute. The ! or $ command line prefix executes the operating system command, returns results from the invoked process and displays those results at the Terminal. $ZF(-1) does not return operating system command results; it executes the operating system command, then returns the exit status code for the invoked process. For further details, see “[Issuing Operating System Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).

$ZF(-1) and $ZF(-2)

These two functions are in most respects identical. They differ in the following way:

*   $ZF(-1) suspends execution of the current process while awaiting completion of the spawned child process. It receives status information from the spawned process, which it returns as an exit status code (an integer value) when the spawned process completes.
    
*   [$ZF(-2)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2) does not suspend execution of the current process. It immediately returns a status value upon spawning the child process. Because it does not await completion of the spawned child process it cannot receive status information from that process.
    

Examples

The following Windows example executes a user-written program, in this case displaying the contents of a .txt file. It uses NormalizeFilenameWithSpaces() to handle a pathname for $ZF(-1). A pathname containing spaces is handled as appropriate for the host platform. A pathname that does not contain spaces is passed through unchanged. $ZF(-1) returns the Windows shell exit status of 0 if the specified file could be accessed, or 1 if the file access failed:

   SET fname\="C:\\My Test.txt"
   WRITE fname,!
   SET x\=##class(%Library.File).NormalizeFilenameWithSpaces(fname)
   WRITE x,!
   WRITE $ZF(\-1,x)

 

The following Windows example invokes the Windows operating system SOL command. SOL opens a window that displays the Solitaire game provided with the Windows operating system. Upon closing of the Solitaire interactive window, $ZF(-1) returns the Windows shell exit status of 0, indicating success:

   SET x\=$ZF(\-1,"SOL")
   WRITE x

The following Windows example invokes a non-existent Windows operating system command. $ZF(-1) returns the Windows shell exit status of 1, indicating a syntax error:

   WRITE $ZF(\-1,"SOX")

 

The following Windows example invokes a Windows operating system command, specifying a non-existent network name. $ZF(-1) returns the Windows shell exit error status of 2:

   WRITE $ZF(\-1,"NET USE :k \\\\bogusname")

 

See Also

*   [$ZF(-2)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2) function
    
*   [Issuing Operating System Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_syscall) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2 "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzf-1.xml**