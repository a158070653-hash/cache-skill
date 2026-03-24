# CLOSE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cclose

---

 

Caché ObjectScript Reference

CLOSE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose "CLOSE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Closes a file or a device.

Synopsis

CLOSE:pc closearg,...
C:pc closearg,... 

where closearg is:

device:parameters

Arguments

pc

Optional — A postconditional expression.

device

The device to be closed.

parameters

Optional — One or more parameters used to set characteristics of the device. A single parameter may be specified as a quoted string: CLOSE device:"D". Multiple parameters must be specified enclosed by parentheses and separated by colons.

Description

CLOSE device releases ownership of the specified device, optionally sets a parameter, and returns it to the pool of available devices.

If the process does not own the specified device, or if the specified device is not open or does not exist, Caché ignores CLOSE and returns without issuing an error.

Arguments

pc

An optional postconditional expression. Caché executes the CLOSE command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

device

The device to be closed. A device can be a physical device, a TCP connection, or a sequential file. Specify the same device ID (mnemonic or number) as specified on the corresponding OPEN command. For more information on specifying device IDs, refer to the [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command.

You can specify a single device ID or as a comma-separated list of device IDs. CLOSE closes all of the listed devices that the process currently has open. It ignores any listed devices that do not exist or are not currently open by this process.

The device ID of the current device is contained in the [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio) special variable.

parameters

A parameter or a colon-separated list of parameters used when closing the specified device. Parameter codes are not case-sensitive. Multiple parameters must be enclosed with parentheses and separated by colons.

The available parameter values are as follows:

"D"

Closes and deletes a sequential file. Can also be specified as /DEL, /DEL=1, /DELETE, or /DELETE=1.

"R":newname

Closes and renames a sequential file. Can also be specified as /REN=newname or /RENAME=newname.

“K”

Closes at the Caché level but not at the operating system level. Used only on non-Windows systems.

If the specified parameter is not valid, CLOSE still closes the device.

Refer to [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in the Caché I/O Device Guide for further information.

Examples

In the following UNIX® example, the CLOSE command closes device C (/dev/tty02), but only if it is not the current device. The postconditional uses the $IO special variable to check for the current device.

CloseDevC
  SET C\="/dev/tty02"
  OPEN C
    ; ...
  CLOSE:$IO'=C C

Notes

Acquiring Ownership of a Device

A process acquires ownership of a device with the OPEN command and makes it active with the USE command. If the closed device is the active (that is, current) device, the default I/O device becomes the current device. (The default I/O device is established at login.) When a process is terminated (for example, after a HALT), all its opened devices are automatically closed and returned to the system.

If the process’s default device is closed, any subsequent output (such as error messages) to that device causes the process to hang. In this case, you must explicitly reopen the default device.

See Also

*   [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cclose.xml**