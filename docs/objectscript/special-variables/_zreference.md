# $ZREFERENCE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzreference

---

 

Caché ObjectScript Reference

$ZREFERENCE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpos "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZREFERENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzreference "$ZREFERENCE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the current global reference.

Synopsis

$ZREFERENCE
$ZR

Description

$ZREFERENCE contains the name and subscript(s) of the last [global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global) reference. This is known as the naked indicator.

Note:

The last global reference is the most recently accessed global node. Usually, this is the most recent explicit reference to a global. However, certain commands may internally use the $ORDER function to traverse global subscripts (the ZWRITE command is an example of this), or they may refer internally to some other global. When this occurs, $ZREFERENCE contains the last accessed global node, which may not be the global node specified to the command.

The last global reference can be either a global (^myglob) or a [process-private global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls) (^||myppg). $ZREFERENCE returns the process-private global prefix in the form that was initially used for that variable, regardless of which process-private global prefix is used subsequently for that variable. In the description of $ZREFERENCE that follows, the word “global” refers to both types of variables.

The last global reference is the global most recently referred to by a command or a function. Because ObjectScript performs operations in left-to-right order, the last global reference is always the rightmost global. When a command or function takes multiple arguments, the global specified in the rightmost argument is the last global reference. When an argument contains multiple global references, the rightmost specified global is the last global reference. This strict left-to-right order holds true even if parentheses are used to define the order of operations.

Caché updates $ZREFERENCE when an explicit global reference is issued. Invoking an expression (such as a local variable) that evaluates to a global reference does not update $ZREFERENCE.

$ZREFERENCE contains the most recent global reference, even if this global reference was not successful. When a command references an undefined global, issuing an <UNDEFINED> error, Caché updates $ZREFERENCE to that global reference, just as if the global were defined. This behavior is unaffected by setting the [%SYSTEM.Process.Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method.

$ZREFERENCE often contains the most recent global reference, even if the command execution was not successful. Caché updates $ZREFERENCE as each global is referenced. For example, a command issuing a <DIVIDE> error (attempting to divide a number by 0) updates $ZREFERENCE to the last global referenced in the command before the error occurred. However, a <SYNTAX> error does not update $ZREFERENCE.

Long Global Names

If a global name is longer than 31 characters (excluding global prefix character(s), such as ^), $ZREFERENCE returns the global name shortened to 31 characters. For further information on the handling of long global names, refer to the [Global Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global) section of the “Variables” chapter of Using Caché ObjectScript.

Naked Global Reference

If the last global reference was a naked global reference, $ZREFERENCE contains the external, readable, full form of the current naked global reference. This is demonstrated in the following example:

  SET ^MyData(1)\="fruit"
  SET ^MyData(1,1)\="apples" ; Full global reference
  SET ^(2)\="oranges"        ; Naked global reference,
                            ; implicitly ^MyData(1,2)
  WRITE !,$ZREFERENCE       ; Returns "^MyData(1,2)"

 

For further details on naked global references, see [Using Caché Multidimensional Storage (Globals)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Extended Global Reference

Extended global reference is used to reference a global that is in a namespace other than the current namespace. If a command references a global variable using an extended global reference, the $ZREFERENCE value contains that extended global reference. Caché returns an extended global reference in the following circumstances:

*   If the last global reference uses an extended reference to refer to a global in another namespace.
    
*   If the last global reference uses an extended reference to refer to a global in the current namespace.
    
*   If the last global reference is a remote reference (a global on a remote system).
    

In all cases, $ZREFERENCE returns the namespace name in all capital letters, regardless of how it was specified in the global reference.

For further details on global subscripts and extended global references, see [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

Operations that Update $ZREFERENCE

The $ZREFERENCE special variable is initialized to the null string (""). Changing the current namespace resets $ZREFERENCE to the null string.

The following operations set $ZREFERENCE to the most recently referenced global:

*   A command or function that uses a global as an argument. If it uses multiple globals, $ZREFERENCE is set to the rightmost occurrence of a global. (Note, however, the exception of $ORDER.)
    
*   A command that uses a global as a postconditional expression.
    
*   Following a ZWRITE, Caché sets $ZREFERENCE to the last accessed subscript node of the specified global reference.
    
*   A command or function that references an undefined global, and either generates an <UNDEFINED> error, or, in the case of $INCREMENT, defines the global.
    

Setting $ZREFERENCE

You can set this special variable using the SET command, as follows:

*   Set to the null string (""). Doing so deletes the naked indicator. If the next global reference is a naked global reference, Caché issues a <NAKED> error.
    
*   Set to a valid global reference (defined or undefined). This causes subsequent naked references to use the value you set as if it were the last actual global reference.
    

$ZREFERENCE cannot be otherwise modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Examples

The following example returns the last global reference:

   SET ^a(1,1)\="Hello"  ; Full global reference
   SET ^(2)\=" world!"   ; Naked global reference
   WRITE $ZREFERENCE

 

returns:

^a(1,2)

The following example returns global references from several different commands. Note that WRITE and ZWRITE set different representations of the same global reference.

   SET (^barney,^betty,^wilma,^fred)\="flintstone"
   WRITE !,$ZREFERENCE
   KILL ^flies
   WRITE !,$ZREFERENCE
   WRITE !,^fred
   WRITE !,$ZREFERENCE,!
   ZWRITE ^fred
   WRITE !,$ZREFERENCE

 

returns:

^fred          ; last of several globals set in left-to-right order
^flies         ; KILL sets global indicator, though no global to kill
flintstone                ; WRITE global
^fred                       ; global from WRITE
^fred="flintstone"   ; ZWRITE global
^fred("")                 ; global from ZWRITE

The following example returns an extended global reference. Note that the namespace name is always returned in uppercase letters:

   SET ^\["samples"\]a(1,1)\="Hello"
   SET ^(2)\=" world!"
   WRITE $ZREFERENCE
   QUIT

 

returns:

^\["SAMPLES"\]a(1,2)

The following example returns the last global reference. In this case, it is ^a(1), used as an argument to the $LENGTH function:

   SET ^a(1)\="abcdefghijklmnopqrstuvwxyz"
   SET ^b(1)\="1234567890"
   SET x\=$LENGTH(^a(1))
   WRITE $ZREFERENCE
   QUIT

 

The following example returns the value set for $ZREFERENCE as if it were the last global reference. In this case, it is ^a(1,1).

   SET ^a(1,1)\="abcdefghijklmnopqrstuvwxyz"
   SET ^b(1,1)\="1234567890"
   WRITE !,^(1)
             ; Naked reference uses last global
   SET $ZREFERENCE\="^a(1,1)"
   WRITE !,$ZREFERENCE
   WRITE !,^(1)
             ; Naked reference uses $ZREFERENCE
             ; value, rather than last global.

 

The following example sets an extended global reference. Note the doubled quotation marks:

   KILL ^x
   WRITE !,$ZREFERENCE
   SET $ZREFERENCE\="^\[""samples""\]a(1,2)"
   WRITE !,$ZREFERENCE

 

See Also

*   [ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace) command
    
*   [$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace) special variable
    
*   [$ZNSPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vznspace) special variable
    
*   [Configuring Namespaces](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_config) in Caché System Administration Guide
    
*   [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals
    
*   [Naked Global Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpos "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzreference.xml**