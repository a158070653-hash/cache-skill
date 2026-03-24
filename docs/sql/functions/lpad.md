# LPAD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_lpad

---

 

Caché SQL Reference

LPAD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lower "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ltrim "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [LPAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lpad "LPAD") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns a string left-padded to a specified length.

Synopsis

LPAD(string-expression,length\[,padstring\])

Arguments

string-expression

A string expression, which can be the name of a column, a string literal, a host variable, or the result of another scalar function. Can be of any data type convertible to a VARCHAR data type. string-expression cannot be a stream.

length

An integer specifying the number of characters in the returned string.

padstring

Optional — A string consisting of a character or a string of characters used to pad the input string-expression. The padstring character or characters are appended to the left of string-expression to supply as many characters as need to create an output string of length characters. padstring may be a string literal, a column, a host variable, or the result of another scalar function. If omitted, the default is a blank space character.

Description

LPAD pads a string expression with leading pad characters. It returns a copy of the string padded to length number of characters. If the string expression is longer than length number of characters, the return string is truncated to length number of characters.

If string-expression is NULL, LPAD returns NULL. If string-expression is the empty string ('') LPAD returns a string consisting entirely of pad characters. The returned string is type VARCHAR.

LPAD does not remove leading or trailing blanks; it pads the string including any leading or trailing blanks. To remove leading or trailing blanks before padding a string, use LTRIM, RTRIM, or TRIM.

LPAD and $JUSTIFY

The two-argument form of LPAD and the two-argument form of $JUSTIFY both right-align a string by padding it with leading spaces. These two-argument forms differ in how they handle an output length that is shorter than the length of the input string-expression: LPAD truncates the input string to fit the specified output length. $JUSTIFY expands the output length to fit the input string. This is shown in the following example:

SELECT '>'||LPAD(12345,10)||'<' AS lpadplus,
       '>'||$JUSTIFY(12345,10)||'<' AS justifyplus,
       '>'||LPAD(12345,3)||'<' AS lpadminus,
       '>'||$JUSTIFY(12345,3)||'<' AS justifyminus

 

Examples

The following example left pads column values with ^ characters (when needed) to return strings of length 16. Note that some Name strings are left padded, some Name strings are right truncated to return strings of length 16.

   SELECT TOP 15 Name,LPAD(Name,16,'^') AS Name16
   FROM Sample.Person 

 

The following example left pads column values with the ^=^ pad string (when needed) to return strings of length 20. Note that the pad name string is repeated as many times as needed, and that some return strings contain partial pad strings:

   SELECT TOP 15 Name,LPAD(Name,20,'^=^') AS Name20
   FROM Sample.Person 

 

See Also

*   [$JUSTIFY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_justify) function
    
*   [RPAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rpad) function
    
*   [LTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ltrim) function
    
*   [RTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rtrim) function
    
*   [TRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lower "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ltrim "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_lpad.xml**