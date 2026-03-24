# CONVERT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_convert

---

 

Caché SQL Reference

CONVERT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_concat "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cos "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert "CONVERT") \]

Go to: Description CONVERT(datatype,expression,format-code) {fn CONVERT(expression,datatype)} CONVERT Class Method Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that converts a given expression to a specified data type.

Synopsis

CONVERT(datatype,expression\[,format-code\])

{fn CONVERT(expression,datatype)}

Arguments

expression

The expression to be converted.

datatype

The data type to which expression is to be converted.

format-code

Optional — An integer code that specifies date and time formats, used to convert between date/time/timestamp data types and character data types. This argument is only used with the general scalar syntax form.

Description

Two different implementations of the CONVERT function are described here. Both convert an expression in one data type to a corresponding value in another data type. Both perform date and time conversions.

Note:

The arguments in these two implementations of CONVERT are presented in a different order. The first is a general Caché scalar function compatible with MS SQL Server, which takes three arguments. The second is a Caché ODBC scalar function with two arguments. These two forms of CONVERT are handled separately in the text that follows.

If an expression does not have a defined data type (for example a host variable supplied by Caché ObjectScript) its data type defaults to the string data type.

Specifying an invalid value to either version of CONVERT results in an SQLCODE -141.

For a list of the data types supported by Caché SQL, see [Data Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype). For other data type conversions, refer to the [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) function.

CONVERT(datatype,expression,format-code)

This is the MS SQL Server compatible function. It takes as datatype any valid Caché SQL data type. For a list of the data types supported by Caché SQL, see [Data Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype).

You can truncate a string by performing a VARCHAR-to-VARCHAR conversion, specifying an output string length shorter than the expression string length.

When using CONVERT (or CAST), if a character data type (such as CHAR or VARCHAR) has no specified length, the default maximum length is 30 characters. If a binary data type (such as BINARY or VARBINARY) has no specified length, the default maximum length is 30 characters. Otherwise, these data types with no specified length are mapped to a MAXLEN of 1 character, as shown in the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) table.

You can perform a BIT data type conversion. The permitted values are 1, 0, or NULL. If you specify any other value, Caché issues an SQLCODE -141 error. In the following Embedded SQL example, both are BIT conversions of a NULL:

  SET a\=""
  &sql(SELECT CONVERT(BIT,:a),
       CONVERT(BIT,NULL)
       INTO :x,:y)
  WRITE !,"SQLCODE=",SQLCODE
  WRITE !,"the host variable is:",x
  WRITE !,"the NULL keyword is:",y

 

The optional format-code argument specifies a date, datetime, or time format. This format can either be used to define the output when converting from a date/time/timestamp data type to a character string, or to define the input when converting from a character string to a date/time/timestamp data type. The following format codes are supported; format codes that output a two-digit year are listed in the first column; formats that either output a four-digit year or do not output a year at all are listed in the second column:

Two-digit year codes

Four-digit year codes

Format

 

0 or 100

Mon dd yyyy hh:mmAM (or PM)

1

101

mm/dd/yy

2

102

yy.dd.mm

3

103

dd/mm/yy

4

104

dd.mm.yy

5

105

dd-mm-yy

6

106

dd Mon yy

7

107

Mon dd, yy (no leading zero when dd < 10)

 

8 or 108

hh:mm:ss

 

9 or 109

Mon dd yyyy hh:mm:ss.nnnAM (or PM)

10

110

mm-dd-yy

11

111

yy.mm.dd

12

112

yymmdd

 

13 or 113

dd Mon yyyy hh:mm:ss:nnn (24 hour)

 

14 or 114

hh:mm:ss.nnn (24 hour)

 

20 or 120

yyyy-mm-dd hh:mm:ss (24 hour)

 

21 or 121

yyyy-mm-dd hh:mm:ss.nnn (24 hour)

 

126

yyyy-mm-ddThh:mm:ss:nnn (24 hour)

 

130

dd Mon yyyy hh:mm:ss:nnnAM (or PM)

 

131

dd/mm/yyyy hh:mm:ss:nnnAM (or PM)

The following are features of date and time conversions:

*   Range of Values: The range of permitted dates is 1840-12-31 through 9999-12-31.
    
*   Default Values: When performing a DATETIME or SMALLDATETIME conversion, the date defaults to 1900-01-01. The time defaults to 00:00:00.
    
*   Default Format: If format-code is not specified, CONVERT attempts to determine the format from the specified value. If it cannot, it defaults to format-code 100.
    
*   Two-digit Years: Two-digit years from 00 through 49 are converted to 21st century dates (2000 through 2049); two-digit years from 50 through 99 are converted to 20th century dates (1950 through 1999).
    
*   Fractional Seconds: Fractional seconds can be preceded by either a period (.) or a colon (:). The symbols have different meanings. A period indicates a standard fraction; thus 12:00:00.4 indicates four-tenths of a second, and 12:00:00.004 indicates four-thousandth of a second. A colon indicates that what follows is in thousandths of a second; thus 12:00:00:4 indicates four-thousandth of a second.
    

{fn CONVERT(expression,datatype)}

This is the ODBC scalar function. It supports the following ODBC explicit data type conversions. You must use the “SQL\_” keywords for specifying data types with this form of CONVERT:

From

To

SQL\_TIMESTAMP (%TimeStamp)

SQL\_TIME, SQL\_DATE, SQL\_VARCHAR, SQL\_TIMESTAMP

SQL\_VARCHAR (%String)

SQL\_TIME, SQL\_DATE, SQL\_TIMESTAMP

SQL\_TIME (%Time)

SQL\_VARCHAR, SQL\_INTEGER, SQL\_BIGINT, SQL\_SMALLINT, SQL\_TINYINT, SQL\_TIMESTAMP, SQL\_TIME

SQL\_DATE (%Date)

SQL\_VARCHAR, SQL\_INTEGER, SQL\_BIGINT, SQL\_SMALLINT, SQL\_TINYINT, SQL\_TIMESTAMP, SQL\_DATE

Any non-BLOB data type

SQL\_INTEGER, SQL\_BIGINT, SQL\_SMALLINT, SQL\_TINYINT

Any non-BLOB data type

SQL\_DOUBLE

[Character stream data](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs)

SQL\_VARCHAR

Any numeric data type

SQL\_VARCHAR

SQL\_VARCHAR is the standard ODBC representation. When converting to SQL\_VARCHAR, dates and times are converted to their appropriate ODBC representations; numeric datatype values are converted to a string representation. When converting from SQL\_VARCHAR, the value must be a valid ODBC Time, Timestamp, or Date representation.

When converting to an integer data type or the SQL\_DOUBLE data type, data values (including dates and times) are converted to a numeric representation. For SQL\_DATE, this is the number of days since January 1, 1841. For SQL\_TIME, this is the number of seconds since midnight. Input strings are truncated when a nonnumeric character is encountered. The integer data types also truncates decimal digits, returning the integer portion of the number.

A NULL converted to any data type remains NULL.

An empty string (''), or any nonnumeric string value converts as follows:

*   SQL\_VARCHAR and SQL\_TIMESTAMP return the supplied value.
    
*   Numeric data types convert to 0 (zero).
    
*   SQL\_DATE and SQL\_TIME convert to NULL.
    

For other data type conversions, refer to the [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) function.

CONVERT Class Method

You can also perform data type conversions using the [CONVERT()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CONVERT) method call, using “SQL\_” keywords for specifying data types:

$SYSTEM.SQL.CONVERT(expression,convert-to-type,convert-from-type)

as shown in the following example:

  WRITE $SYSTEM.SQL.CONVERT(60945,"SQL\_VARCHAR","SQL\_DATE")

 

Examples

CONVERT() Examples

The following examples uses the Caché scalar syntactical form of CONVERT.

The following example compares the conversion of a fractional number using the DECIMAL and DOUBLE data types:

SELECT CONVERT(DECIMAL,\-123456789.0000123456789) AS DecimalVal,
       CONVERT(DOUBLE,\-123456789.0000123456789) AS DoubleVal

 

The following example shows several conversions of the date-of-birth field (DOB) to a formatted character string:

SELECT DOB,
       CONVERT(VARCHAR(20),DOB) AS DOBDefault,
       CONVERT(VARCHAR(20),DOB,100) AS DOB100,
       CONVERT(VARCHAR(20),DOB,107) AS DOB107,
       CONVERT(VARCHAR(20),DOB,114) AS DOB114,
       CONVERT(VARCHAR(20),DOB,126) AS DOB126
FROM Sample.Person

 

The default format and the code 100 format are the same. Because the DOB field does not contain a time value, formats that display time (here including the default, 100, 114, and 126) supply a zero value, which represents 12:00AM (midnight). The code 126 format provides a date and time string that contains no spaces.

{fn CONVERT()} Examples

The following examples uses the ODBC syntactical form of CONVERT.

The following embedded SQL example converts a mixed string to an integer. Caché truncates the string at the first nonnumeric character and then converts the resulting numeric to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical):

  SET a\="007 James Bond"
  &sql(SELECT {fn CONVERT(:a,SQL\_INTEGER)} INTO :x)
  WRITE !,"SQLCODE=",SQLCODE
  WRITE !,"the host variable is:",x

 

returns the integer 7.

The following example converts dates in the "DOB" (Date Of Birth) column to the SQL\_TIMESTAMP data type.

SELECT DOB,{fn CONVERT(DOB,SQL\_TIMESTAMP)} AS DOBtoTstamp
     FROM Sample.Person

 

The resulting timestamp is in the format: yyyy-mm-dd hh:mm:ss.

The following example converts dates in the "DOB" (Date Of Birth) column to the SQL\_INTEGER data type.

SELECT DOB,{fn CONVERT(DOB,SQL\_INTEGER)} AS DOBtoInt
     FROM Sample.Person

 

The resulting integer is the $HOROLOG count of days since December 31, 1840.

The following example converts dates in the "DOB" (Date Of Birth) column to the SQL\_VARCHAR data type.

SELECT DOB,{fn CONVERT(DOB,SQL\_VARCHAR)} AS DOBtoVChar
     FROM Sample.Person

 

The resulting string is in the format: yyyy-mm-dd.

See Also

*   [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) function
    
*   [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_concat "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cos "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_convert.xml**