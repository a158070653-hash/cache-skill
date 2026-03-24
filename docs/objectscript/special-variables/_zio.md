# $ZIO

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzio

---

 

Caché ObjectScript Reference

$ZIO

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzhorolog "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZIO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzio "$ZIO") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains information about the current terminal I/O device.

Synopsis

$ZIO
$ZI

Description

$ZIO contains information about the current I/O device.

For a terminal device that is a Caché terminal, $ZIO contains the string TRM:. If the current terminal device is connected remotely, $ZIO contains information about the remote connection.

For a terminal device connected through TELNET, $ZIO contains the following: host|port:

host

The remote host IP address, in either IPv4 format:nnn.nnn.nnn.nnn, where nnn is a decimal number, or in IPv6 format: h:h:h:h:h:h:h:h, where h is a hexadecimal number. Further details on IPv4 and IPv6 formats can be found in the section “[Use of IPv6 Addressing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_ch_serverconfig#GORIENT_serverconfig_IPv6)” in the chapter “[Server Configuration Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_ch_serverconfig)” in the Caché Programming Orientation Guide.

port

The remote IP port number.

These two values are separated by a vertical bar character. For example, 127.0.0.1|23. For further information, refer to the [%Library.NetworkAddress](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.NetworkAddress) class, a subclass of the data type class [%Library.String](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.String).

If the current device is not a terminal:

*   If a file, $ZIO contains the full canonical pathname of the file.
    
*   If not a file, $ZIO contains the null string.
    

The following example returns the current device information:

   SET x\=$CASE($ZIO,"TRM:":"a terminal",
               "CON:":"a console",
               "":"neither terminal nor file")
   WRITE "The current device is ",x

 

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

See Also

*   [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio) special variable
    
*   [$PRINCIPAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzhorolog "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzio.xml**