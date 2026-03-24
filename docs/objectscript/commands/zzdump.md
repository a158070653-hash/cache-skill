# ZZDUMP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_czzdump

---

 

Caché ObjectScript Reference

ZZDUMP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump "ZZDUMP") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Displays an expression in hexadecimal dump format.

Synopsis

ZZDUMP:pc expression,...

Arguments

pc

Optional — A postconditional expression.

expression

The data to be displayed in hexadecimal dump format. You can specify a number, a string (enclosed in quotation marks), or a variable that resolves to one of these. You can specify a single expression, or a comma-separated list of expressions.

Description

ZZDUMP displays an expression in hexadecimal dump format. ZZDUMP is primarily of interest to system programmers, but it can be useful in viewing strings that contain control characters.

ZZDUMP returns a number or string value in the following format:

position: hexdata printdata

Arguments

pc

Caché executes the ZZDUMP command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

expression

You can specify expression as a numeric, a string literal, or a variable that resolves to one of these. You can specify a single expression, or a comma-separated list of expressions. Specifying a comma-separated list of expressions is parsed as issuing a separate ZZDUMP command for each expression. Execution of a comma-separated list stops when the first error occurs.

An expression can be a variable of any type, including [local variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local), [process-private globals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls), [global variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global), and [special variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special). You can use extended reference to specify a global variable in another namespace. If you specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error, followed by the global name and database path, such as the following: <PROTECT> ^myglobal,c:\\intersystems\\cache\\mgr\\.

Examples

The following example shows ZZDUMP returning hex dumps for two single-character string variables. Note that each comma-separated expression is treated as a separate invocation of ZZDUMP:

   SET x\="A"
   SET y\="B"
   ZZDUMP x,y

 

0000: 41                                                A
0000: 42                                                B

The following example shows ZZDUMP returning a hex dump for a string variable too long for a single dump line. Note that the position for the second dump line (0010:) is in hexadecimal:

   SET z\="ABCDEFGHIJKLMNOPQRSTUVWXYZ"
   ZZDUMP z

 

0000: 41 42 43 44 45 46 47 48 49 4A 4B 4C 4D 4E 4F 50     ABCDEFGHIJKLMNOP
0010: 51 52 53 54 55 56 57 58 59 5A                       QRSTUVWXYZ

The following example shows ZZDUMP returning hex dumps for three variables. Note that no hex dump (not even a blank line) is returned for a null string variable. Also note that a number is converted to canonical form (leading and trailing zeros and plus sign removed); a string containing a number is not converted to canonical form:

   SET x\=+007
   SET y\=""
   SET z\="+007"
   ZZDUMP x,y,z

 

0000: 37                                         7
0000: 2B 30 30 37                                +007

Notes

Unicode

If one or more characters in a ZZDUMP expression is a wide (Unicode) character, all characters in that expression are represented as wide characters. The following examples show variables containing a Unicode characters. In all cases, all characters are displayed as wide characters.

   SET x\=$CHAR(987)
   SET y\=$CHAR(987)\_"ABC"
   ZZDUMP x,y

 

0000: 03DB                                                    ?
0000: 03DB 0041 0042 0043                                     ?ABC

ZZDUMP Compared with Write Commands

For a comparison of ZZDUMP with the [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite), [ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite), and [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) commands, refer to the [Write Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_cmds_iowrite) features tables in the “Commands” chapter of Using Caché ObjectScript.

See Also

*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite) command
    
*   [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) command
    
*   [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) function
    
*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$ZHEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_czzdump.xml**