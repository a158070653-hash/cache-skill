# $KEY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vkey

---

 

Caché ObjectScript Reference

$KEY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vjob "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$KEY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vkey "$KEY") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the terminator character from the most recent READ.

Synopsis

$KEY
$K

Description

$KEY contains the character or character sequence that terminated the last READ command on the current device. $KEY and $ZB are very similar in function; see below for a detailed comparison.

*   If the last read terminated because of a terminator character (such as the <RETURN> key), $KEY contains the terminator character.
    
*   If the last read terminated because of a timeout or a fixed-length read length limit, $KEY contains the null string. No terminator character was encountered.
    
*   If the last read was a single-character read (READ \*a), and a character was entered, $KEY contains the actual input character.
    

$KEY and $ZB are very similar, though not identical. See below for a comparison.

You can use the SET command to specify a value for $KEY. You can use the ZZDUMP command to display the value of $KEY.

During a terminal session, the end of every command line is recorded in $KEY as a carriage return (hexadecimal 0D). In addition, the $KEY special variable is initialized to carriage return by the process that initializes the terminal session. Therefore, to display the value of $KEY set by the READ command or a SET command during a terminal session, you must copy the $KEY value to a local variable within the same line of code.

Examples

In the following example, a variable-length READ command either receives data from the terminal or times out after 10 seconds. If the user inputs the data before the timeout, $KEY contains the user-input carriage return (hex 0D) that terminated the data input. If, however, the READ timed out, $KEY contains the null string, indicating that no terminator character was received.

  READ "Ready or Not: ",x:10
  ZZDUMP $KEY

In the following example, a fixed-length READ command either receives data from the terminal or times out after 10 seconds. If the user inputs the specified number of characters (in this case, one character), the user does not have to press <RETURN> to conclude the READ operation. The user can respond to the read prompt by pressing <RETURN> rather than entering the specified number of characters.

If the read operation timed out, both $KEY and $ZB contain the null string. If the user inputs a one-character middle initial, $KEY contains the null string, because the fixed-length READ operation concluded without a terminator character. If the user pressed <RETURN> rather than entering a middle initial, $KEY contains the user-input carriage return.

   READ "Middle initial: ",z#1:10
   IF $ASCII($ZB)\=\-1 {
     WRITE !,"The read timed out" }
   ELSEIF $ASCII($KEY)\=\-1 {
     WRITE !,"A character was entered" }
   ELSEIF $ASCII($KEY)\=13 {
     WRITE !,"A line return was entered" }
   ELSE {
     WRITE !,"Unexpected result" } 

Notes

$KEY and $ZB Compared

Both $KEY and $ZB contain the character that terminates a READ operation. These two special variables are similar, but not identical. Here are the principal differences:

*   $KEY can be set using the SET command. $ZB cannot be SET.
    
*   Following a successful fixed-length READ, $ZB contains the final character input (for example, when the 5-digit postal code “02138” is input as a fixed-length READ, $ZB contains “8”). Following a successful fixed-length READ, $KEY contains the null string ("").
    
*   $KEY does not support block-based read and write operations, such as magnetic tape I/O.
    

$KEY on the Command Line

When issuing commands interactively from the Caché Terminal command line, you press <RETURN> to issue each command line. The $KEY and $ZB special variables record this command line terminator character. Therefore, when using $KEY or $ZB to return the termination status of a read operation, you must set a variable as part of the same command line.

For example, if you issue the command:

\>READ x:10

from the command line, then check $KEY, it will not contain the results of the read operation; it will contain the <RETURN> character that executed the command line. To return the results of the read operation, set a local variable with $KEY in the same command line, as follows:

\>READ x:10 SET rkey=$KEY

This preserves the value of $KEY set by the read operation. To display this read operation value, issue either of the following command line statements:

\>WRITE $ASCII(rkey)
   ; returns -1 for null string (time out) 
   ; returns ASCII decimal value for terminator character
>ZZDUMP rkey
   ; returns blank line for null string (time out)
   ; returns hexadecimal value for terminator character

See Also

*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    
*   [$ZB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vjob "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vkey.xml**