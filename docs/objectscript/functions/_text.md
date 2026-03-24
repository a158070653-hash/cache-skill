# $TEXT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ftext

---

 

Caché ObjectScript Reference

$TEXT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftranslate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$TEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftext "$TEXT") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns a line of source code found at the specified location.

Synopsis

$TEXT(label+offset^routine)
$TEXT(@expr\_atom)
$T(label+offset^routine)
$T(@expr\_atom)

Parameters

label

Optional — A line label in a routine. Must be a literal value; a variable cannot be used to specify label. Line labels are case sensitive. If omitted, +offset is counted from the beginning of the routine.

+offset

Optional — An expression that resolves to a positive integer that identifies the line to be returned as an offset number of lines. If omitted, the line identified by label is returned.

^routine

Optional — The name of a routine that resides on disk. The system loads the routine from disk and begins execution at the first executable line of the routine. Must be a literal value; a variable cannot be used to specify routine. (Note that the ^ character is a separator character, not part of the routine name.) If the routine is not in the current namespace, you can specify the namespace that contains the routine using an [extended routine reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_namespaces_extendedrefs), as follows: ^|"namespace"|routine. If omitted, defaults to the currently loaded routine.

@expr\_atom

An expression atom that uses indirection to supply a location. Resolves to some form of label+offset^routine.

Description

$TEXT returns a line of source code found at the specified location. The source code is returned as text and is not executed at the reference point. If $TEXT does not find source code at the specified location, it returns the null string.

To identify a single line of source code, you must specify either a label, an +offset, or both. By default, $TEXT accesses the currently loaded routine, the routine most recently loaded using the [ZLOAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czload). You can use ^routine to specify a routine location other than the currently loaded routine. You can use indirection (@expr\_atom) to specify a location.

In the returned source code, if the first whitespace character in the line is a tab, $TEXT replaces it with a single space character. All other tabs and space characters are returned unchanged. Thus $PIECE($TEXT(line)," ",1) always returns a label, and $PIECE($TEXT(line)," ",2,99) returns all code except a label.

$TEXT does not recognize the Return character that terminates the line.

If you invoke $TEXT from the Terminal command line for the currently loaded routine, it can return comment lines of any type. However, entirely blank lines, including those within a multiline comment, are neither counted nor returned.

If you invoke $TEXT from within a compiled program, $TEXT returns ;; comments. The ;; comment is the only comment type retained in object code, and are thus available to the $TEXT function. For a ;; comment to be returned by $TEXT, it must either appear on its own line, or on the same line as a label. It cannot appear on a line containing a command, or a line declaring a function or subroutine. For further details on the different types of Caché comments, refer to [Comments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_comments) in Using Caché ObjectScript.

You can use the [PRINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cprint) or [ZPRINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czprint) commands to display a single line (or multiple lines) of source code from the currently loaded routine.

Parameters

label

The label within the current routine or, if the routine parameter is also supplied, a label in the specified routine. Must be specified as a literal, without quotes. [Label names](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) are case-sensitive, and may contain Unicode characters. A label may be longer than 31 characters, but must be unique within the first 31 characters. $TEXT matches only the first 31 characters of a specified label.

If you omit the offset option, or specify label+0, Caché prints the label line. Note that with this form, offset actually evaluates to offset+1 because the label itself is counted as line 0. For example, label+1 prints the line after the label. If label is not found in the routine, $TEXT returns the empty string.

offset

A positive integer specifying a line count, or as an expression that evaluates to a positive integer. The leading plus sign (+) is mandatory. If specified alone, the +offset specifies a line count from the beginning of the routine, with +offset\=1 being the first line of the routine. If specified with the label parameter, the line count is calculated from the label location, with +offset\=0 being the label line itself, and +offset\=1 being the line after the label. If +offset is larger than the number of lines in the routine (or the number of lines from label to the end of the routine) $TEXT returns the empty string.

You can specify an offset of +0. When label is specified, $TEXT(mylabel+0) is the same as $TEXT(mylabel). If you invoke $TEXT(+0), it returns the name of the currently loaded routine.

Caché resolves an +offset value to a canonical integer: it deletes leading zeros, performs arithmetic and plus/minus sign evaluation, truncates a fractional number to its integer portion, truncates a numeric string at the first non-numeric character. A negative integer offset value generates a <NOLINE> error.

Note that Caché resolves numbers and numeric strings to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical), which involves removal of the leading plus sign (+). For this reason, you must specify the plus sign in the $TEXT function to use it as an offset, as shown in the following example:

  SET x\="+7"
  WRITE $TEXT(x)    /\* because the + was removed from the
                       numeric string x, $TEXT searches for
                       a label named x, not the offset +7  \*/
  WRITE $TEXT(+x)   /\* locates the offset +7 code line \*/

routine

If specified alone, it indicates the first line of code in the routine. If specified with only the label parameter, the line found at that specified label within the routine is returned. If specified with only the offset parameter, the line at the specified offset within the routine is returned. If both label and offset are supplied, the line found at the specified offset within the specified label within the routine is returned.

The routine argument must be specified as a literal, without quotes. You cannot use a variable to specify the routine name. The leading caret (^) is mandatory.

By default, Caché searches for the routine in the current namespace. If the desired routine resides in another namespace, you can specify that namespace using [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). For example, $TEXT(mylabel+2^|"SAMPLES"|myroutine). Note that only vertical bars can be used here; square brackets cannot be used. You can specify the namespace portion of ^routine as a variable.

$TEXT returns the empty string if the specified routine or namespace does not exists, or if the user does not have access privileges for the namespace.

expression atom (@expr\_atom)

An indirect argument that evaluates to a $TEXT argument (label+offset^routine). For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

Examples

In the following four examples, the first two examples return the referenced line, including the ;; comment. The third and fourth examples return the null string:

Start ;; this comment is on a label line
  WRITE $TEXT(Start)

 

Start 
  ;; this comment is on its own line
  WRITE $TEXT(Start+1)

 

Start 
  SET x\="fred" ;; this comment is on a command line
  WRITE $TEXT(Start+1)

 

MyFunc() ;; this comment is on a function declaration line
  WRITE $TEXT(MyFunc)

 

The following example shows that only the first 31 characters of label are matched with the specified label:

StartabcdefghijklmnopqrstuvwxyzA ;; 32-character label
  WRITE $TEXT(StartabcdefghijklmnopqrstuvwxyzB)

 

The following example shows the $TEXT(label) form, which returns the line found at the specified label within the current routine. The label is also returned. If the user enters "?", the Info text is written out, along with the line label, and control returns to the initial prompt:

Start()
  READ !,"Array name or ? for Info: ",ary QUIT:ary\=""
  IF ary\="?" {
    WRITE !,$TEXT(Info),! 
    DO Start }
  ELSE { DO ArrayOutput(.ary) }
Info()  ;; This routine outputs the first-level subscripts of a variable.
ArrayOutput(val)
   SET i\=1
   WHILE $DATA(val(i)) {
     WRITE !,"subscript ",i," is ",val(i)
     SET i\=i+1
   }

The following example shows the $TEXT(label+offset) form, which returns the line found at the offset within the specified label, which must be within the current routine. If the offset is 0, the label line, with the label, is returned. This example uses a FOR loop to access multiline text, avoiding displaying the label or the multiline comment delimiters:

Start()
  READ !,"Array name or ? for Info: ",ary QUIT:ary\=""
  IF ary\="?" {
    DO Info
    DO Start }
  ELSE { DO ArrayOutput(.ary) }
Info()  FOR loop\=2:1:6 { WRITE !,$TEXT(Info+loop) }
     /\* 
    This routine outputs the first-level subscripts of a variable.
    Specifically, it asks you to supply the name of the variable
    and then writes out the current values for each subscript
    node that contains data. It stops when it encounters a node
    that does not contain data.
    \*/
ArrayOutput(val)
   SET i\=1
   WHILE $DATA(val(i)) {
     WRITE !,"subscript ",i," is ",val(i)
     SET i\=i+1
   }

The following example uses [extended routine reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_namespaces_extendedrefs) to access a line of code from a routine in the SAMPLES namespace. It accesses the first line of code after the ErrorTest label in the routine named myroutine. It can be executed from any namespace:

    WRITE $TEXT(ErrorTest+1^|"SAMPLES"|myroutine)

Notes

Argument Indirection

Indirection of the entire $TEXT argument is a convenient way to make an indirect reference to both the line and the routine. For example, if the variable ENTRYREF contains both a line label and a routine name, you can reference the variable:

$TEXT(@ENTRYREF)

rather than referencing the line and the routine separately:

$TEXT(@$PIECE(ENTRYREF,"^",1)^@$PIECE(ENTRYREF,"^",2))

Edit Pointer

If you specify a routine in $TEXT other than the current routine, Caché resets the edit pointer (current line location) to +0. This can affect execution of the ZINSERT command. You can access the [$ZNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzname) special variable to determine the current routine.

See Also

*   [PRINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cprint) command
    
*   [ZINSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czinsert) command
    
*   [ZLOAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czload) command
    
*   [ZPRINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czprint) command
    
*   [ZREMOVE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czremove) command
    
*   [ZSAVE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czsave) command
    
*   [ZZPRINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzprint) command
    
*   [Comments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_comments) in Using Caché ObjectScript
    
*   [Labels](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) in Using Caché ObjectScript
    
*   [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript
    
*   [The Spool Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_spool) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftranslate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ftext.xml**