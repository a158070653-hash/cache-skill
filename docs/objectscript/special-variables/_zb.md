# $ZB

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzb

---

 

Caché ObjectScript Reference

$ZB

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vza "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb "$ZB") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains status information for the current I/O device.

Synopsis

$ZB

Description

$ZB contains status information specific to the current I/O device following a READ operation.

*   When reading from a terminal, sequential file, or other character-based I/O device, $ZB contains the terminating character of the read operation. This can be a terminator character (such as <RETURN>), the final character of the input data if the read operation does not require a terminator character, or the null string if a terminator character is required but was not received (for example, if the read operation timed out).
    
*   When reading from a block-based I/O device, such as magnetic tape, $ZB contains the number of bytes remaining in the I/O buffer. $ZB also contains the number of bytes in the I/O buffer when writing to magnetic tape.
    

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

$ZB and $KEY can both be used to return the READ termination character when reading from a character-based device or file. For character-based reads, these two special variables are very similar, but not identical. For block-based reads and writes (such as magnetic tape) use $ZB; $KEY does not provide support for block-based read and write operations. See [$KEY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vkey) for further details.

End-of-File Behavior

By default, Caché handles an end-of-file on a sequential file by issuing an <ENDOFFILE> error; it does not set $ZB. You can configure end-of-file behavior in a manner compatible with MSM. In this case, when an end-of-file is encountered, Caché does not issue an error, but sets $ZB to "" (the null string), and sets $ZEOF to -1.

To configure end-of-file handling, go to the Management Portal, select \[Home\] > \[Configuration\] > \[Compatibility Settings\]. View and edit the current setting of SetZEOF. When set to “true”, Caché sets $ZB to "" (the null string), and sets $ZEOF to -1. The default is “false”.

You can control end-of-file handling for the current process using the [SetZEOF()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#SetZEOF) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [SetZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#SetZEOF) property of the Config.Miscellaneous class.

Reading from a Terminal or File

$ZB contains the terminating character (or character sequence) from a read operation involving a terminal, sequential file, or other character-based I/O device. $ZB can contain any of the following:

*   A termination character, such as a carriage return.
    
*   An escape sequence (up to 16 characters).
    
*   The nth character in a fixed-length READ x#n. (In this case, the $KEY special variable returns the null string.)
    
*   The single character of READ \*x.
    
*   A null string ("") after a timed READ expires.
    

For example, consider the following variable-length read with a five-second timeout:

Zbread
  READ !,"Enter number:",num:5
  WRITE !, num
  WRITE !, $ASCII($ZB)
  QUIT

If the user types 123 at the READ prompt and presses <RETURN>, Caché stores 123 in the num variable and stores <RETURN> (ASCII decimal code 13, hexadecimal 0D) in $ZB. If the READ times out, $ZB contains the null string; $ASCII("") returns a value of –1.

$ZB on the Command Line

When issuing commands interactively from the Caché Terminal command line, you press <RETURN> to issue each command line. The $ZB and $KEY special variables record this command line terminator character. Therefore, when using $ZB or $KEY to return the termination status of a read operation, you must set a variable as part of the same command line.

For example, if you issue the command:

\>READ x:10

from the command line, then check $ZB it will not contain the results of the read operation; it will contain the <RETURN> character that executed the command line. To return the results of the read operation, set a local variable with $ZB in the same command line, as follows:

\>READ x:10 SET rzb=$ZB

This preserves the value of $ZB set by the read operation. To display this read operation value, issue either of the following command line statements:

\>WRITE $ASCII(rzb)
   ; returns -1 for null string (time out), 
   ; returns ASCII decimal value for terminator character
>ZZDUMP rkey
   ; returns blank line for null string (time out)
   ; returns hexadecimal value for terminator character

$ZB with Magnetic Tape I/O

$ZB contains status information about the driver buffer. Specifically, it contains the number of bytes that remain in the magnetic tape drive’s internal buffer.

Immediately after you read a block, Caché sets $ZB to that block’s size. As you transfer logical records from the buffer to variables (with READ commands), Caché decrements the $ZB value until it reaches 0 and the next block read occurs.

When you write to tape, $ZB shows the available space (in bytes) remaining in the driver’s internal buffer. Immediately after you write a block, Caché sets $ZB to the buffer size specified with the OPEN command. As you transfer logical records from Caché variables into the buffer (with WRITE commands), Caché decrements the $ZB number until it reaches 0 and the block write occurs.

Most magnetic tape programs need not be concerned with $ZB unless they must deal with unusual formats and variable length blocks.

To monitor magnetic tape operations, your program can test the appropriate bit of $ZA after each read and write.

The following code checks both $ZA and $ZB after each magnetic tape read, and sets MTERR when either of these variables indicates an error. It also sets $ZTRAP when a magnetic tape error occurs.

   ; $$MTIN(mtdev) = the next logical record from magtape
   ; device mtdev.
   ; Also returns za=$ZA and zb=$ZB
   ; On a magtape error, mterr=1 and $$MTIN(mtdev)=""
   ; Expects the caller to have set $ZT to trap other
   ; errors.
MTIN(io)
  NEW rec,curdev
  SET mterr\=0,curdev\=$IO,$ZT\="MTIERR"
  USE io 
  READ rec
MTIEXIT
  SET za\=$ZA,zb\=$ZB
  USE curdev
  QUIT rec
MTIERR
  IF $ZERROR\["MAGTAPE" {
        USE curdev 
        ZQUIT 1 
        GOTO @$ZTRAP }
     ; Use caller's error trap.
  ELSE {
       SET $ZTRAP\="",mterr\=1,rec\=""
       GOTO MTIEXIT }

If a terminator completes a READ, Caché mode returns the terminator as a string in $ZB.

If an escape sequence terminates a READ, Caché mode returns the ASCII escape sequence as a string in $ZB.

See Also

*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [$KEY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vkey) special variable
    
*   [$ZA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vza) special variable
    
*   [$ZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) in Caché I/O Device Guide
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    
*   [The Spool Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_spool) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vza "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzb.xml**