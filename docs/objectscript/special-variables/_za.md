# $ZA

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vza

---

 

Caché ObjectScript Reference

$ZA

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vza "$ZA") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the status of the last READ on the current device.

Synopsis

$ZA

Description

$ZA contains the status of the last READ on the current device.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Notes

$ZA with Terminal I/O

$ZA is implemented as a sequence of bit flags, with each bit indicating a specific piece of information. The following table shows the possible values, their meanings, and how to test them using the [modulo](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_modulo) (#) and [integer divide](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_intdiv) (\\) operators:

Bit

Test

Meaning

0

$ZA#2

A <CTRL-C> arrived, whether or not breaks were enabled.

1

$ZA\\2#2

The READ timed out.

2

$ZA\\4#2

I/O error.

8

$ZA\\256#2

Caché detected an invalid escape sequence.

9

$ZA\\512#2

The hardware detected a parity or framing error.

10

$ZA\\1024#2

OpenVMS only: Data overrun error.

11

$ZA\\2048#2

The process is disconnected from its principal device.

12

$ZA\\4096#2

For COM ports: CTS (Clear To Send). A signal sent from the modem to its computer indicating that transmission can proceed. For TCP devices: the device is functioning in Server mode.

13

$ZA\\8192#2

For COM ports: DSR (Data Set Ready). A signal sent from the modem to its computer indicating that it is ready to operate. For TCP devices: the device is currently in the Connected state talking to a remote host.

14

$ZA\\16384#2

Ring set if TRUE.

15

$ZA\\32768#2

Carrier detect set if TRUE.

16

$ZA\\65536#2

CE\_BREAK COM port error state.

17

$ZA\\131072#2

CE\_FRAME COM port error state.

18

$ZA\\262144#2

CE\_IOE COM port error state.

19

$ZA\\524288#2

CE\_OVERRUN COM port error state.

20

$ZA\\1048576#2

CE\_RXPARITY COM port error state.

21

$ZA\\2097152#2

CE\_TXFULL COM port error state.

22

$ZA\\4194304#2

TXHOLD COM port error state. Set if any of the following fields are true in the error mask returned by ClearCommError(): fCtsHold, fDsrHold, fRlsdHold, fXoffHold, fXoffSent.

24 & 25

$ZA\\16777216#4

Caché requested DTR (Data Terminal Ready) setting: 0=DTR off. 1=DTR=on. 2=DTR handshaking. When set (1), indicates readiness to transmit and receive data.

While many of the conditions that $ZA shows are errors, they do not interrupt the program’s flow by trapping to $ZTRAP. (A <CTRL-C> with breaks enabled traps to $ZTRAP.) A program concerned with these errors must check $ZA after every READ.

COM ports use bits 12 through 15, 24 and 25 to report the status of modem control pins. This can be done regardless of whether Caché modem control checking is on or off for the port. A user can enable or disable $ZA error reporting for COM ports by setting the OPEN or USE command portstate parameter (byte 8, to be specific). If error reporting is enabled, the port error state is reported in bits 16 through 22. For further details, see [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide.

You can use the [DisconnectErr()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#DisconnectErr) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class for modem disconnect detection for the current process. The system-wide default behavior can be established by setting the [DisconnectErr](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#DisconnectErr) property of the Config.Miscellaneous class.

$ZA With Magnetic Tape I/O

With magnetic tape I/O, the bit fields in $ZA indicate errors and special conditions. Caché updates $ZA after each command that references the magnetic tape device.

The following table shows the meanings of the $ZA bits for magnetic tape I/O. Note the Trap column. The letter Y indicates a <MAGTAPE> error. If you have set the $ZTRAP variable, Caché issues the associated $ZTRAP error code.

Bit

Value

Trap

Meaning

Notes

0

1

Y

Logical Error (mixed Reads and Writes)

To switch between reading and writing, either Close and then Open the device or issue a Forward Space, Backspace, or Rewind command.

2

4

N

Write Protected

Reflects the state of the OPEN or USE read-only parameter at all times. This bit does not reflect the state of the tape’s physical write protection (write ring or write lock) because many versions of UNIX® give no notice of tape write protection until an actual write to tape is attempted. If you attempt to open a write-protected 9-track tape without the read-only parameter, Caché sets this bit and opens the tape as read only. No error occurs.

3

8

Y

Error Summary

The error summary is the logical OR of all conditions that cause a Caché error (all conditions marked Y under Trap)

5

32

N

Beginning of Tape \[BOT\]

On UNIX® systems this bit is set upon rewind, and cleared when tape is opened.

6

64

N

On Line

 

7

128

Y

Controller or drive error

 

10

1024

N

End of Tape \[EOT\]

Not supported on most UNIX® platforms.

14

16384

Y

Tape Mark

Caché sets the Tape Mark bit when it encounters a tape mark on Read, Read Block, Forward Space or Backspace. This sets the Error Summary bit and traps to $ZTRAP on Read, Read Label, and Read Block

15

32768

Y

Tape Not Ready

 

Some bits indicate error conditions, while other bits indicate conditions that do not necessarily produce an error. To monitor these non-error conditions, your program must test the appropriate bit of $ZA after every magnetic tape operation. For example, if your program might write beyond the end of the tape, it must check bit 10 (End of Tape).

To test a bit, integer divide $ZA by the value listed for that bit in the table and perform a modulo 2 operation. For example, the following command checks if bit 14 (Tape Mark) is set:

   USE 47 IF $ZA\\16384#2 {DO Endfile}

where 16384 equals 2 to the 14th power and #2 indicates the modulo 2 operation. Since any number to the 0 power equals 1, you do not need a divisor to check bit 0 (Logical Error). For example:

   USE 47 GOTO Logerr:$ZA#2

See Also

*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [$ZB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb) special variable
    
*   [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) in Caché I/O Device Guide
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vza.xml**