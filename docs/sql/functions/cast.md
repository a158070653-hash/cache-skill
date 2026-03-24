# CAST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_cast

---

 

Caché SQL Reference

CAST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_atan "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ceiling "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast "CAST") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that converts a given expression to a specified data type.

Synopsis

CAST(expr AS CHAR | CHARACTER | VARCHAR | NCHAR | NVARCHAR)
CAST(expr AS CHAR(n) | CHARACTER(n) | VARCHAR(n))
CAST(expr AS CHAR VARYING | CHARACTER VARYING)
CAST(expr AS INT | INTEGER | BIGINT | SMALLINT | TINYINT)
CAST(expr AS DEC | DECIMAL | NUMERIC)
CAST(expr AS DEC(p\[,s\]) | DECIMAL(p\[,s\]) | NUMERIC(p\[,s\])
CAST(expr AS FLOAT | REAL)
CAST(expr AS DOUBLE)
CAST(expr AS MONEY | SMALLMONEY)
CAST(expr AS DATE)
CAST(expr AS TIME)
CAST(expr AS TIMESTAMP | DATETIME | SMALLDATETIME)
CAST(expr AS BIT)
CAST(expr AS BINARY | BINARY VARYING | VARBINARY)
CAST(expr AS BINARY(n) | BINARY VARYING(n) | VARBINARY(n))

Arguments

expr

An SQL expression.

n

An integer, indicating the maximum number of characters to return.

p,s

Optional — p\=Precision (maximum number of total digits), expressed as an integer. s\=Scale (maximum number of decimal digits), expressed as an integer. If scale is not specified, it defaults to 15.

Description

The SQL CAST function converts the data type of an expression to another data type.

You can cast an expression to any of the following types:

*   CHAR or CHARACTER: represent a numeric or a string by its initial character. VARCHAR with no n defaults to a length of 30 characters when specified to CAST or CONVERT. Otherwise, the VARCHAR data type (with no specified size) is mapped to a MAXLEN of 1 character, as shown in the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) table. NCHAR is equivalent to CHAR; NVARCHAR is equivalent to VARCHAR.
    
*   CHAR(n), CHARACTER(n), or VARCHAR(n): represent a numeric or a string by the number of characters specified by n.
    
*   CHAR VARYING or CHARACTER VARYING: represent a numeric or a string by the number of characters in the original value.
    
*   INT, INTEGER, BIGINT, SMALLINT, and TINYINT: represent a numeric by its integer portion. Decimal digits are truncated.
    
*   DEC, DECIMAL, and NUMERIC: represent a numeric by the number of digits in the original value. Converts using the Caché [$DECIMAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdecimal) function, which converts $DOUBLE values to $DECIMAL values. The p (precision), if specified, is retained as part of the defined data type, but does not affect the value returned by CAST. If you specify a s (scale) value of a positive integer, the decimal value is rounded to the specified number of digits. (The appropriate number of trailing zeros are included for Display mode, but are truncated for Logical mode and ODBC mode.) If you specify s\=0, the numeric value is rounded to an integer. If you specify s\=-1, the numeric value is truncated to an integer.
    
*   FLOAT and REAL: represent a numeric by the number of digits in the original value, with a precision of 18 and a scale of 9.
    
*   DOUBLE represents the IEEE floating point standard. For further details, refer to the Caché ObjectScript [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function.
    
*   MONEY and SMALLMONEY are currency numeric data types. The scale for currency data types is always 4.
    
*   DATE: represents a date. Dates can be represented in any of the following formats, depending on context: the display date format for your locale (for example, MM/DD/YYYY); the ODBC date format (YYYY-MM-DD); or the $HOROLOG integer date storage format (nnnnn).
    
*   TIME: represents a time. Times can be represented in any of the following formats, depending on context: the display time format for your locale (for example, hh:mm:ss); the ODBC date format (hh:mm:ss); or the $HOROLOG integer time storage format (nnnnn).
    
*   TIMESTAMP, DATETIME, and SMALLDATETIME: represents a date and time stamp with the format YYYY-MM-DD hh:mm:ss. This corresponds to the Caché ObjectScript [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable.
    
*   BIT represents a single binary value.
    
*   BINARY, BINARY VARYING, and VARBINARY represent a value of data type %Library.Binary (xDBC data type BINARY). The optional n length defaults to 1 for BINARY, 30 for BINARY VARYING and VARBINARY. No conversion of the data is actually performed when casting to a binary value. Caché does truncate the length of the value at the specified n length.
    

For a list of the data types supported by Caché SQL, see [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype). For other data type conversions, refer to the [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) function. If you specify a CAST with an unsupported data type, Caché issues an SQLCODE -376.

Casting Numerics

A numeric value can be cast to a numeric data type or to a character data type.

When casting a numeric results in a shortened value, the numeric is truncated, not rounded. For example, casting 98.765 to INT returns 98, to CHAR returns 9, and to CHAR(4) returns 98.7. Note that casting a negative number to CHAR returns just the negative sign, and casting a fractional number to CHAR returns just the decimal point.

A numeric value can consist of the digits 0 through 9, a decimal point, one or more leading signs (+ or –), and the exponent sign (the letter E or e) followed by, at most, one + or – sign. A numeric cannot contain group separator characters (commas). For further details, see the [literals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_literals) section of “Language Elements” in Using Caché SQL.

Before a cast is performed, Caché SQL resolves a numeric to its canonical form: Exponentiation is performed. Caché strips leading and trailing zeros, a leading plus sign, and a trailing decimal point. Multiple signs are resolved before casting a numeric. However, SQL treats double negative signs as a [comment](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_comments) indicator; encountering double negative signs in a number results in Caché processing the remainder of that line of code as a comment.

A Caché floating point number can take a DEC, DECIMAL, NUMERIC, or FLOAT data type. The DOUBLE data type represents floating point numbers according to the IEEE floating point standard. The Caché floating point data types have greater precision than the DOUBLE data type, and are preferable for most applications. You cannot use CAST to cast a floating point number to the DOUBLE data type; instead, use the Caché ObjectScript [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function.

When a numeric value is cast to a date or time data type, it displays in SQL as zero (0); however, when a numeric cast as a date or time is passed out of embedded SQL to Caché ObjectScript, it displays as the corresponding $HOROLOG value.

Casting Character Strings

You can cast a character string to another character data type, returning either a single character, the first n characters, or the entire character string.

Before a cast is performed, Caché SQL resolves embedded quote characters ('can''t'=can't) and string concatenation ('can'||'not'=cannot). Leading and trailing blanks are retained.

When a character string is cast to a numeric type, it always returns the single digit zero (0).

You can cast a character string to the DATE, TIME, or TIMESTAMP data type. The following operations result in a valid value:

*   A string of the format 'nnnn-nn-nn' can be cast to DATE. (This string format corresponds to ODBC date format.) Value and range checking is performed; the input date value must be a valid date within the range of years 1841 through 9999. In SQL, this cast displays as the locale's date display format. For example, '2004–11–23' might display as '11/23/2004'. In embedded SQL, this cast is returned as the corresponding $HOROLOG date integer. A non-numeric string is treated as 0 when cast to DATE; DATE returns the zero date: 12/31/1840.
    
*   A string of the format 'nn:nn:nn' or 'nn:nn:nn.nn' can be cast to TIME (where nn is a one-digit or two-digit integer within the range of valid time values). This string format corresponds to ODBC time format. In SQL, this cast displays as the locale's time display format (with leading zeros added and fractional seconds truncated); for example, '2:33:25.99' would display as '02:33:25'. In embedded SQL, this cast is returned as the corresponding $HOROLOG time integer; fractional seconds are not truncated. A non-numeric string is treated as 0 when cast to TIME; TIME returns 00:00:00.
    
*   Normally, a DATE or TIME data type is cast to TIMESTAMP. A character string cast to TIMESTAMP displays as specified; no value or range checking is performed, leading zeros are not added and fractional seconds are not truncated.
    
*   A string of the format 'nnnnn' can be cast to DATE. (This integer string corresponds to $HOROLOG date format.) In SQL this cast displays as 0. In embedded SQL, this cast is returned as the corresponding $HOROLOG date integer.
    
*   A string of the format 'nnnnn' can be cast to TIME. (This integer string corresponds to $HOROLOG time format.) In SQL this cast displays as 0. In embedded SQL, this cast is returned as the corresponding $HOROLOG time integer.
    

Input values not described above return a value of 0.

Casting NULL and the Empty String

NULL can be cast to any data type and returns NULL.

The empty string ('') casts as follows:

*   All character data types return NULL.
    
*   All numeric data types return 0 (zero), with the appropriate number of trailing fractional zeros. The DOUBLE data type returns zero with no trailing fractional zeros.
    
*   The DATE data type returns 12/31/1840.
    
*   The TIME data type returns 00:00:00.
    
*   The TIMESTAMP, DATETIME, and SMALLDATETIME data types return NULL.
    
*   The BIT data type returns 0.
    
*   All binary data types return NULL.
    

Casting Dates

You can cast a date to a date data type, to a numeric data type, or to a character data type.

Casting a date to the TIMESTAMP, DATETIME, or SMALLDATETIME data type returns a timestamp with the format YYYY-MM-DD hh:mm:ss. Since a date does not have a time portion, the time portion of the resulting timestamp is always 00:00:00. If the expr value is not a valid date in the locale's date display format, CAST returns NULL.

Casting a date to a numeric data type returns the $HOROLOG value for the date. This is an integer value representing the number of days since Dec. 31, 1840.

Casting a date to a character data type returns either the complete date, or as much of the date as the length of the data type permits. However, the display format is not the same for all character data types. The CHAR VARYING and CHARACTER VARYING data types return the complete date in display format. For example, if a date displays as MM/DD/YYYY, these data types return the date as a character string with the same format. The other character data types return the date (or a part thereof) as a character string in ODBC date format. For example, if a date displays as mm/dd/yyyy, these data types return the date as a character string with the format YYYY-MM-DD. Thus for the date 04/24/2004, the CHAR data type returns '2' (the first character of the year), and a CHAR(8) returns '2004–04–'.

Casting a Bit Value

You can cast an expr value AS BIT to return a 0 or 1. If expr is 1 or any other non-zero numeric value, it returns 1. If expr is “TRUE”, “True”, or “true”, it returns 1. (The word “True” can be represented in any combination of uppercase and lowercase, but cannot be abbreviated as “T”.) If expr is any other non-numeric value, it returns 0. If expr is 0, it returns 0.

In the following example, the first five CAST operations return 1, the second five CAST operations return 0:

SELECT CAST(1 AS BIT) AS One, 
       CAST(7 AS BIT) AS Num,      
       CAST(743.6 AS BIT) AS Frac,  
       CAST(0.3 AS BIT) AS Zerofrac,
       CAST('tRuE' AS BIT) AS TrueWord,
       CAST(0 AS BIT) AS Zero,  
       CAST('FALSE' AS BIT) AS FalseWord, 
       CAST('T' AS BIT) AS T,    
       CAST('F' AS BIT) AS F,   
       CAST(0.0 AS BIT) AS Zerodot     

 

Examples

The following example uses the CAST function to present an average as an integer, not a floating point. Note that the CAST truncates the number, rather than rounding it:

SELECT DISTINCT AVG(Age) AS AvgAge,
   CAST(AVG(Age) AS INTEGER) AS IntAvgAge
      FROM Sample.Person

 

The following example shows how the CAST function converts pi (a floating point number) to different numeric data types:

SELECT 
   CAST({fn PI()} As INTEGER) As IntegerPi,
   CAST({fn PI()} As SMALLINT) As SmallIntPi,
   CAST({fn PI()} As DECIMAL) As DecimalPi,
   CAST({fn PI()} As NUMERIC) As NumericPi,
   CAST({fn PI()} As FLOAT) As FloatPi,
   CAST({fn PI()} As DOUBLE) As DoublePi

 

Note in the following example that the precision and scale values are parsed, but do not change the value returned by CAST:

SELECT 
   CAST({fn PI()} As DECIMAL) As DecimalPi,
   CAST({fn PI()} As DECIMAL(6,3)) As DecimalPSPi

 

The following example shows how the CAST function converts pi (a floating point number) to different character data types:

SELECT 
   CAST({fn PI()} As CHAR) As CharPi,
   CAST({fn PI()} As CHAR(4)) As CharNPi,
   CAST({fn PI()} As CHAR VARYING) As CharVaryingPi,
   CAST({fn PI()} As VARCHAR(4)) As VarCharNPi

 

The following example shows how the CAST function converts Name (a character string) to different character data types:

SELECT DISTINCT 
   CAST(Name As CHAR) As CharName,
   CAST(Name As CHAR(4)) As CharNName,
   CAST(Name As CHAR VARYING) As CharVaryingName,
   CAST(Name As VARCHAR(4)) As VarCharNName
      FROM Sample.Person

 

The following example shows what happens when you use the CAST function to converts Name (a character string) to different numeric data types. In every case, the value returned is 0 (zero):

SELECT DISTINCT 
   CAST(Name As INT) As IntName,
   CAST(Name As SMALLINT) As SmallIntName,
   CAST(Name As DEC) As DecName,
   CAST(Name As NUMERIC) As NumericName,
   CAST(Name As FLOAT) As FloatName
      FROM Sample.Person

 

The following example casts a date field (DOB) to a numeric data type and several character data types. Casting a date to a numeric returns the $HOROLOG integer equivalent. Casting a date to a character data type returns either a date string in input format (CHAR VARYING or CHARACTER VARYING) or the date (partial or full) in ODBC date string format:

SELECT DISTINCT DOB,
   CAST(DOB As INT) AS IntDate,
   CAST(DOB As CHAR) AS CharDate,
   CAST(DOB As CHAR(6)) AS CharNDate,
   CAST(DOB As CHAR VARYING) AS CharVaryDate,
   CAST(DOB As VARCHAR(10)) AS VarCharNDate
      FROM Sample.Person

 

The following example casts character strings to the DATE and TIME data types:

SELECT CAST('1936-11-26' As DATE) AS StringToDate,
       CAST('14:33:45.78' AS TIME) AS StringToTime

 

Only a string with the format YYYY-MM-DD can be converted to a date. Strings with other formats return 0. Note that fractional seconds are truncated (not rounded) when converting a string to the TIME data type.

The following example casts a date to the TIMESTAMP data type:

SELECT DISTINCT DOB,
   CAST(DOB As TIMESTAMP) AS DateToTstamp
      FROM Sample.Person

 

The resulting timestamp is in the format: YYYY-MM-DD hh:mm:ss.

The following example casts a character string to the TIME data type, then casts the resulting time to the TIMESTAMP data type:

SELECT CAST(CAST('14:33:45.78' AS TIME) As TIMESTAMP) AS TimeToTstamp

 

The resulting timestamp is in the format: YYYY-MM-DD hh:mm:ss. The time portion is supplied by the nested CAST; the date portion is the current system date.

See Also

*   [Data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert)
    
*   [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber) [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_atan "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ceiling "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_cast.xml**