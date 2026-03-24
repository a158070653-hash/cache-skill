# $X

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vx

---

 

Caché ObjectScript Reference

$X

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$X](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx "$X") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the current horizontal position of the cursor.

Synopsis

$X

Description

$X contains the current horizontal position of the cursor. As characters are written to a device, Caché updates $X to reflect the horizontal cursor position.

Each printable character that is output increments $X by 1. A carriage return (ASCII 13) or form feed (ASCII 12) resets $X to 0 (zero).

$X is a 16-bit unsigned integer.

*   On non-Unicode systems, $X wraps to 0 when its value reaches 65536. In other words, if $X is 65535, the next output character resets it to 0.
    
*   On Unicode systems, $X wraps to 0 when its value reaches 16384 (the two remaining bits are used for Japanese pitch encoding).
    

You can use the SET command to give a value to $X and $Y. For example, you may use special escape sequences that alter the physical cursor position without updating the $X and $Y values. In this case, use SET to assign the correct values to $X and $Y after you use the escape sequences.

Notes

NLS Character Mapping

The National Language Support (NLS) utility $X/$Y tab defines the $X and $Y cursor movement characters for the current locale. For further details, refer to the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.

$X with Terminal I/O

The following table shows the effects of different characters on $X.

Echoed Character

ASCII Code

Effect on $X

<FORM FEED>

12

$X=0

<RETURN>

13

$X=0

<LINE FEED>

10

$X=$X

<BACKSPACE>

8

$X=$X-1

<TAB>

9

$X=$X+1

Any printable ASCII character

32-126

$X=$X+1

Nonprintable characters (such as escape sequences)

127-255

See [Using Caché ObjectScript](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS).

The S(ecret) protocol of the OPEN and USE commands turns off echoing. It also prevents $X from being changed during input, so it indicates the true cursor position.

WRITE $CHAR() changes $X. WRITE \* does not change $X. For example, WRITE $X,"/",$CHAR(8),$X performs the backspace (deleting the / character) and resets $X accordingly, returning 01. In contrast, WRITE $X,"/",\*8,$X performs the backspace (deleting the / character) but does not reset $X; it returns 02. (See the [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command for further details.)

Using WRITE \*, you can send a control sequence to your terminal and $X will still reflect the true cursor position. Since some control sequences do move the cursor, you can use the SET command to set $X directly. For example, the following commands move the cursor to column 20 and line 10 on a Digital VT100 terminal (or equivalent) and set $X and $Y accordingly:

   SET dy\=10,dx\=20
   WRITE \*27,\*91,dy+1,\*59,dx+1,\*72
   SET $Y\=dy,$X\=dx

ANSI standard control sequences (such as escape sequences) that the device acts on but does not output can produce a discrepancy between the $X and $Y values and the true cursor position. To avoid this problem use the WRITE \* (integer expression) syntax and specify the ASCII value of each character in the string. For example, instead of using:

   WRITE !,$CHAR(27)\_"\[1m"
   WRITE !,$X

 

use this equivalent form:

   WRITE !,\*27,\*91,\*49,\*109
   WRITE !,$X

 

As a rule, after any escape sequence that explicitly moves the cursor, you should update $X and $Y to reflect the actual cursor position.

You can set how $X handles escape sequences for the current process using the [DX()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#DX) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [DX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#DX) property of the Config.Miscellaneous class.

$X with TCP and Interprocess Communication

When you use the WRITE command to send data to either a client or server TCP device, Caché first stores the data in a buffer. It also updates $X to reflect the number of characters in the buffer. It does not include the ASCII characters <RETURN> and <LINE FEED> in this count because they are considered to be part of the record.

If you flush the $X buffer with the WRITE ! command, Caché resets $X to 0 and increments the $Y value by 1. If you flush the $X and $Y buffers with the WRITE # command, Caché writes the ASCII character <FORM FEED> as a separate record and resets both $X and $Y to 0.

See Also

*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [$Y](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [Local Interprocess Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_interproccomm) in Caché I/O Device Guide
    
*   [TCP Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vx.xml**