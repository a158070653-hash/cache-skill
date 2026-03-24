# $ECODE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vecode

---

 

Caché ObjectScript Reference

$ECODE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vdevice "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode "$ECODE") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the current error code string.

Synopsis

$ECODE
$EC

Description

When an error occurs, Caché sets the $ECODE special variable to a comma-surrounded string containing the error code corresponding to the error. For example, when a reference is made to an undefined global variable, Caché sets the $ECODE special variable to the following string:

,M7,

$ECODE can contain ISO 11756-1999 standard M error codes, with the form M#, where # is an integer. For example, M6 and M7 are “undefined local variable” and “undefined global variable,” respectively. (M7 is issued for both globals and process-private globals.) For a complete list, see [ISO 11756-1999 standard M programming language error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system#RERR_system_ansi) in the Caché Error Reference.

$ECODE can also contain error codes that are the same as Caché General System error codes (the error codes returned at the terminal prompt and to the $ZERROR special variable). However, $ECODE appends a “Z” to these error codes, and removes the angle brackets. Thus the $ECODE error ZSYNTAX is a <SYNTAX> error, ZILLEGAL VALUE is an <ILLEGAL VALUE> error, and ZFUNCTION is a <FUNCTION> error. $ECODE does not retain any additional error info for those error codes that provide it; thus ZPROTECT is a <PROTECT> error; the additional info component is kept in $ZERROR, but not in $ECODE. For more information about Caché error codes, see [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror); for a complete list, see [General System Error Messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system) in the Caché Error Reference.

If an error occurs when $ECODE already contains previous error codes, the existing error stack is cleared when the new error occurs. The new error stack will contain only entries that show the state at the time of the current error. (This is a change from earlier $ECODE behavior, where the old error stack would persist until explicitly cleared.)

If there are multiple error codes, Caché appends the code for each error to the current $ECODE value as a new piece in a string delimited by commas, as follows:

,ZSTORE,M6,ZILLEGAL VALUE,ZPROTECT,

In the above case, the most recent error is a <PROTECT> error.

You can also explicitly clear or set $ECODE. $ECODE is always cleared when you terminate the current process.

Clearing $ECODE

You can clear $ECODE by setting it to the empty string (""), as follows:

   SET $ECODE\=""

Setting $ECODE to the empty string has the following effects:

*   It clears all existing $ECODE values. It has no effect on an existing $ZERROR value.
    
*   It clears the error stack for your job. This means that a subsequent call to the $STACK function returns the current execution stack, rather than the last error stack.
    
*   It affects error processing flow of control for $ETRAP error handlers. See [Error Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript for more details.
    

You cannot NEW the $ECODE special variable. Attempting to do so generates a <SYNTAX> error.

Setting $ECODE

You can force an error by setting $ECODE to an value other than the empty string. Setting $ECODE to any non-null value forces an interpreter error during the execution of a Caché ObjectScript routine. After Caché sets $ECODE to the non-null value that you specify, Caché takes the following steps:

1.  Writes the specified value to $ECODE, overwriting any previous values.
    
2.  Generates an <ECODETRAP> error. (This sets $ZERROR to the value <ECODETRAP>).
    
3.  Passes control to any error handlers you have established. Your error handlers can check for the $ECODE string value you chose and take steps to handle the condition appropriately.
    

$ECODE String Overflow

If the length of the accumulated string in $ECODE exceeds 512 characters, the error code that causes the string overflow clears and replaces the current list of error codes in $ECODE. In this case, the list of errors in $ECODE is the list of errors since the most recent string overflow, beginning with the error that caused the overflow. See [Using Caché ObjectScript](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS) for more information about the maximum string data length.

Notes

Creating Your Own Error Codes

The format for the $ECODE special variable is a comma-surrounded list of one or more error codes. Error codes starting with the letter U are reserved for the user. All other error codes are reserved for Caché.

User–defined $ECODE values should be distinguishable from the values Caché automatically generates. To ensure this, always prefix your error text with the letter U. Also remember to delineate your error code with commas. For example:

   SET $ECODE\=",Upassword expired!,"

Check $ZERROR Rather Than $ECODE for Caché Errors

Your error handlers should check $ZERROR rather than $ECODE for the most recent Caché error.

See Also

*   [ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap) command
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack) function
    
*   [$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack) special variable
    
*   [$ZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof) special variable
    
*   [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable
    
*   [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    
*   [System Error Messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system) in Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vdevice "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vecode.xml**