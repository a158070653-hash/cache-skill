# $Y

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vy

---

 

Caché ObjectScript Reference

$Y

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vza "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$Y](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy "$Y") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the current vertical position of the cursor.

Synopsis

$Y

Description

$Y contains the current vertical position of the cursor. As characters are written to a device, Caché updates $Y to reflect the vertical cursor position.

Each line feed (newline) character (ASCII 10) that is output increments $Y by 1. A form feed character (ASCII 12) resets $Y to 0.

$Y is a 16-bit unsigned integer. $Y wraps to 0 when its value reaches 65536. In other words, if $Y is 65535, the next output character resets it to 0.

You can use the SET command to give a value to $X and $Y. For example, you may use special escape sequences that alter the physical cursor position without updating the $X and $Y values. In this case, use SET to assign the correct values to $X and $Y after you use the escape sequences.

Notes

NLS Character Mapping

The National Language Support (NLS) utility $X/$Y tab defines the $X and $Y cursor movement characters for the current locale. For further details, refer to the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.

$Y with Terminal I/O

The following table shows the effects of different characters on $Y.

Echoed Character

ASCII Code

Effect on $Y

<FORM FEED>

12

$Y=0

<RETURN>

13

$Y=$Y

<LINE FEED>

10

$Y=$Y+1

<BACKSPACE>

8

$Y=$Y

<TAB>

9

$Y=$Y

Any printable ASCII character

32-126

$Y=$Y

The S(ecret) protocol of the OPEN and USE commands turns off echoing. It also prevents $Y from being changed during input, so it indicates the true cursor position.

A WRITE $CHAR() that changes vertical position also changes $Y. A WRITE \* that changes vertical position does not change $Y. For example, WRITE $Y,$CHAR(10),$Y performs the line feed and increments $Y. In contrast, WRITE $Y,\*10,$Y performs the line feed but does not increment $Y. (See the [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command for further details.)

Because WRITE \* does not change $Y, you can send a control sequence to your terminal and $Y will still reflect the true cursor position. Since some control sequences do move the cursor, you can use the SET command to set $Y directly. For example, the following commands move the cursor to column 20 and line 10 on a VT100-type terminal and set $X and $Y accordingly:

  SET dy\=10,dx\=20
  WRITE \*27,\*91,dy+1,\*59,dx+1,\*72
  SET $Y\=dy,$X\=dx

 

ANSI standard control sequences (such as escape sequences) that the device acts on but does not output can produce a discrepancy between the $X and $Y values and the true cursor position. To avoid this problem, use the WRITE \* statement and specify the ASCII value of each character in the string. For example, instead of using the following code:

   WRITE $CHAR(27)\_"\[1m"

 

use this equivalent form:

   WRITE \*27,\*91,\*49,\*109

 

As a rule, after any escape sequence that explicitly moves the cursor, you should update $X and $Y to reflect the actual cursor position.

See Also

*   [$X](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [Interprocess Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_interproccomm) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vza "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vy.xml**