# $JUSTIFY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_justify

---

 

Caché SQL Reference

$JUSTIFY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonobject "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lastday "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$JUSTIFY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_justify "$JUSTIFY") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that right-aligns a value within a specified width, optionally rounding to a specified number of fractional digits.

Synopsis

$JUSTIFY(expression,width\[,decimal\])

Arguments

expression

The value that is to be right-aligned. It can be a numeric value, a string literal, or an expression that resolves to a numeric or string.

width

The number of characters within which expression is to be right-aligned. A positive integer or an expression that evaluates to a positive integer.

decimal

Optional — The number of fractional digits. A positive integer or an expression that evaluates to a positive integer. Caché rounds or pads the number of fractional digits in expression to this value. If you specify decimal, Caché treats expression as a numeric.

Description

$JUSTIFY returns the value specified by expression right-aligned within the specified width. You can include the decimal argument to decimal-align numbers within width.

*   $JUSTIFY(expression,width): the 2-argument syntax right-justifies expression within width. It does not perform any conversion of expression. The expression can be a numeric or a nonnumeric string.
    
*   $JUSTIFY(expression,width,decimal): the 3-argument syntax converts expression to a [canonical number](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numbers), rounds or zero pads fractional digits to decimal, then right-justifies the resulting numeric value within width. If expression is a nonnumeric string or NULL, Caché converts it to 0, pads it, then right-justifies it.
    

$JUSTIFY recognizes the DecimalSeparator character for the current locale. It adds or deletes a DecimalSeparator character as needed. The DecimalSeparator character depends upon the locale; commonly it is either a period (.) for American-format locales, or a comma (,) for European-format locales. To determine the DecimalSeparator character for your locale, invoke the following method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")

 

Commonly, $JUSTIFY is used to format numbers with fractional digits: every number is given the same number of fractional digits, and the numbers are right-aligned so that the DecimalSeparator characters align in a column of numbers.

$JUSTIFY and LPAD

The two-argument form of LPAD and the two-argument form of $JUSTIFY both right-align a string by padding it with leading spaces. These two-argument forms differ in how they handle an output width that is shorter than the length of the input expression: LPAD truncates the input string to fit the specified output length. $JUSTIFY expands the output length to fit the input string. This is shown in the following example:

SELECT '>'||LPAD(12345,10)||'<' AS lpadplus,
       '>'||$JUSTIFY(12345,10)||'<' AS justifyplus,
       '>'||LPAD(12345,3)||'<' AS lpadminus,
       '>'||$JUSTIFY(12345,3)||'<' AS justifyminus

 

The three-argument form of LPAD allows you to left pad with characters other than spaces.

Arguments

expression

The value to be right-justified, and optionally expressed as a numeric with a specified number of fractional digits.

*   If string justification is desired, do not specify decimal. The expression can contain any characters. $JUSTIFY right-justifies expression, as described in width.
    
*   If numeric justification is desired, specify decimal. If decimal is specified, $JUSTIFY converts expression to a [canonical number](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numbers). It resolves leading plus and minus signs and removes leading and trailing zeros. It truncates expression at the first nonnumeric character. If expression begins with a nonnumeric character (such as a currency symbol), $JUSTIFY converts the expression value to 0. For further details on how Caché converts a numeric to a canonical number, and Caché handling of a numeric string containing nonnumeric characters, refer to the [Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numbers) section of the “Data Types and Values” chapter of Using Caché ObjectScript.
    
    After $JUSTIFY converts expression to a canonical number, it zero-pads or rounds this canonical number to decimal number of fractional digits, then right-justifies the result, as described in width. $JUSTIFY does not recognize NumericGroupSeparator characters, currency symbols, multiple DecimalSeparator characters, or trailing plus or minus signs.
    

width

The width in which to right-justify the converted expression. If width is greater than the length of expression (after numeric and fractional digit conversion), Caché right-justifies to width, left-padding as needed with blank spaces. If width is less than the length of expression (after numeric and fractional digit conversion), Caché sets width to the length of the expression value.

Specify width as a positive integer. A width value of 0, the empty string (''), NULL, or a nonnumeric string is treated as a width of 0, which means that Caché sets width to the length of the expression value.

decimal

The number of fractional digits. If expression contains more fractional digits, $JUSTIFY rounds the fractional portion to this number of fractional digits. If expression contains fewer fractional digits, $JUSTIFY pads the fractional portion with zeros to this number of fractional digits, adding a Decimal Separator character, if needed. If decimal\=0, $JUSTIFY rounds expression to an integer value and deletes the Decimal Separator character.

If the expression value is less than 1, $JUSTIFY inserts a leading zero before the DecimalSeparator character.

The [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) values INF, -INF, and NAN are returned unchanged by $JUSTIFY, regardless of the decimal value.

Examples

The following Dynamic SQL example performs right-justification on strings. No numeric conversion is performed:

  ZNSPACE "SAMPLES"
  SET myquery \= "SELECT TOP 20 Age,$JUSTIFY(Name,18),DOB FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following Dynamic SQL example performs numeric right-justification with a specified number of fractional digits:

  ZNSPACE "SAMPLES"
  SET myquery \= 2
  SET myquery(1) \= "SELECT TOP 20 $JUSTIFY(Salary,10,2) AS FullSalary,"
  SET myquery(2) \= "$JUSTIFY(Salary/7,10,2) AS SeventhSalary FROM Sample.Employee"
   SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following Dynamic SQL example performs numeric right-justification with a specified number of fractional digits, and string right-justification of the same numeric value:

  SET myquery \= 2
  SET myquery(1) \= "SELECT $JUSTIFY({fn ACOS(-1)},8,3) AS ArcCos3,"
  SET myquery(2) \= "$JUSTIFY({fn ACOS(-1)},8) AS ArcCosAll"
   SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

The following Dynamic SQL example performs numeric right-justification with the $DOUBLE values INF and NAN:

  DO ##class(%SYSTEM.Process).IEEEError(0)
  SET x\=$DOUBLE(1.2e500)
  SET y\=x\-x
  SET myquery \= 2
  SET myquery(1) \= "SELECT $JUSTIFY(?,12,2) AS INFtest,"
  SET myquery(2) \= "$JUSTIFY(?,12,2) AS NANtest"
   SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(x,y)
  DO rset.%Display()

 

See Also

*   [LPAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lpad) function
    
*   [ROUND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_round) function
    
*   [TRUNCATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonobject "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lastday "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_justify.xml**