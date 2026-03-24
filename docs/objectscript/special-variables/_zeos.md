# $ZEOS

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzeos

---

 

Caché ObjectScript Reference

$ZEOS

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZEOS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeos "$ZEOS") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains end-of-stream status when reading a compressed stream.

Synopsis

$ZEOS

Description

$ZEOS contains a boolean value that indicates whether the end of an incoming (compressed) stream has been received and processed. If $ZEOS\=1, an end-of-stream for a compressed data stream has been received. The $ZEOS value is only meaningful when stream compression/decompression is active (/GZIP=1). You activate stream compression/decompression by issuing the [/GZIP command keyword](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp#GIOD_tcp_openusekeywords) from an OPEN or USE command.

You must check the $ZEOS value before disabling stream compression/decompression by changing the setting to /GZIP=0. If you issue a USE command with /GZIP=0 before the end of a compressed incoming stream has been processed, the USE generates a <TRANSLATE> error. If the end of the compressed incoming stream has not been reached ($ZEOS\=0) you must issue block [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) commands until $ZEOS\=1.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Example

The following example begins with /GZIP=1 (compression enabled). It performs a loop which tests for $ZEOS\=1, and issues READ commands until $ZEOS\=1. It then can set /GZIP=0 (compression disabled):

  OPEN dev:/GZIP\=1
  READ block#length
  FOR {QUIT:$ZEOS
       READ x:10 }
  USE dev:/GZIP\=0

See Also

*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) chapter of Caché I/O Device Guide
    
*   [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) chapter of Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) chapter of Caché I/O Device Guide
    
*   [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) chapter of Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzeos.xml**