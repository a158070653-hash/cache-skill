# $IO

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vio

---

 

Caché ObjectScript Reference

$IO

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vjob "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio "$IO") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the ID of the current input/output device.

Synopsis

$IO
$I

Description

$IO contains the device ID of the current device to which all input/output operations are directed. (If the input and output devices are different, $IO contains the ID of the current input device.)

Caché sets the value of $IO to the principal input/output device at login. $PRINCIPAL contains the ID of the principal device. You issue a USE command to change the current device. Only the USE and CLOSE commands, a BREAK command, or a return to the programmer prompt can change this value.

You can return the device type of the current device by using the [GetType()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Device#GetType) method of the [%Library.Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Device) class.

On UNIX® and OpenVMS systems, $IO contains the actual device name.

On Windows systems, $IO contains a Caché-generated unique identifier for the principal device. For terminal devices (TRM or TNT), this consists of a pseudo-device name enclosed in vertical bars, a colon and another vertical bar, followed by the device’s process ID (pid) number. For non-terminal devices, the pseudo-device name is enclosed in vertical bars and followed by a unique numeric identifier.

For a Caché terminal: |TRM|:|pid

For a Telnet terminal: |TNT|nodename:portnumber|pid

For a file descriptor: |FD|file\_descriptor\_number

(File descriptors are used with CALLIN/CALLOUT remote access.)

For a TCP device: |TCP|unique\_device\_identifier

For a named pipe: |NPIPE|unique\_device\_identifier

For the default printer: |PRN|

For a printer other than the default: |PRN|physical\_device\_name

If the principal device is a null device (which is the default for a background process), $IO contains the null device name with ":pid" appended, thus allowing you to use $IO for a unique subscript. The null device name contained in $IO depends on the operating system.

*   For Windows systems, $IO contains //./nul:pid
    
*   For UNIX® systems, $IO contains /dev/null:pid
    
*   For OpenVMS systems, $IO contains NLA0::pid
    

If the input device is redirected via a pipe or file, $IO contains “00”.

You can test the value of $IO, as in the following OpenVMS example:

TESTPRG 
  OPEN "/dev/tty04"
  USE "/dev/tty04"
  SET old\=$IO
  CLOSE $IO
  OPEN "/dev/tty03"
  USE "/dev/tty03"
  WRITE !,"Old device: ",old,!,"Current device: ",$IO
  CLOSE $IO
  QUIT

The default device number for a device is configurable. Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[Devices\]. For the desired device, click “Edit” to display and modify its Physical Device Name: option. If you do this, $IO will contain the assigned device number, rather than the actual operating system device name.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

See Also

*   [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse) command
    
*   [$PRINCIPAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vjob "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vio.xml**