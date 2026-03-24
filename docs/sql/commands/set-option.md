# SET OPTION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_setoption

---

 

Caché SQL Reference

SET OPTION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption "SET OPTION") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Sets an execution option.

Synopsis

SET OPTION option\_keyword = value

Description

The SET OPTION statement is used to set execution options, such as the compile mode, SQL configuration settings, and the locale settings governing date, time, and numeric conventions. Only one keyword option can be set by each SET OPTION statement.

SET OPTION can be used in [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) or [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql). SET OPTION cannot be issued from the [SQL Shell](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_shell).

Other SET OPTION arguments (not documented here) are parsed by Caché for SQL compatibility, but perform no operation.

Because SET OPTION prepares and executes quickly, and is generally run only once, Caché does not create a cached query for SET OPTION in ODBC, JDBC, or Dynamic SQL.

The following options are supported by Caché:

COMPILEMODE

The COMPILEMODE option sets the compile mode to DEFERRED, IMMEDIATE, INSTALL, or NOCHECK for the current namespace. The default is IMMEDIATE. Changing from DEFERRED to IMMEDIATE compile mode causes any classes in the Deferred Compile Queue to be compiled immediately. If all class compilations are successful, Caché sets SQLCODE to 0. If there are any errors, SQLCODE is set to -400. Class compilation errors are logged in the ^mtemp2 ("Deferred Compile Mode","Error"). If SQLCODE is set to -400, you should view this global structure for more precise error messages. The INSTALL compile mode is similar to the DEFERRED compile mode, but it should only be used for DDL installations where there is no data in the tables.

The NOCHECK compile mode is similar to IMMEDIATE, except that it skips checking of the following constraints when compiling: If a table is dropped, Caché does not check foreign key constraints in other tables that reference the dropped table. If a foreign key constraint is added, Caché does not check existing data to ensure that it is valid for this foreign key. If a NOT NULL constraint is added, Caché does not check existing data for NULLs or assign the field’s default value. If a UNIQUE or Primary Key constraint is deleted, Caché does not check if a foreign key in this table or another table references the dropped key.

LOCK\_TIMEOUT

The LOCK\_TIMEOUT numeric option lets you set the default lock timeout for the current process. The LOCK\_TIMEOUT value is the number of seconds to wait when trying to establish a lock during SQL execution. This lock timeout is used when a locking conflict prevents the current process from immediately locking a record, table, or other entity for a LOCK, INSERT, UPDATE, DELETE, or SELECT operation. Caché SQL continue to try to establish the lock until the timeout expires, at which point an SQLCODE -110 or -114 error is generated.

Available values are positive integers and zero. The timeout setting is per process. You can determine the lock timeout setting for the current process using the [GetProcessLockTimeout()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetProcessLockTimeout) method.

If you do not set the lock timeout for the current process, it defaults to the current system-wide lock timeout setting. If your ODBC connection disconnects and reconnects, the reconnected process uses the current system-wide lock timeout setting. The default system-wide lock timeout is 10 seconds.

For further details on locking conflicts and per-process and system-wide SQL lock timeout settings, refer to the [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock) command.

PKEY\_IS\_IDKEY

The PKEY\_IS\_IDKEY boolean option specifies whether primary keys are also ID keys system-wide. Available values are TRUE and FALSE. If TRUE, the primary key is created as an ID key. (That is, the primary key of the table also becomes the IDKey index in the class definition.) This may improve performance, but has the limitation that a primary key thus created cannot be subsequently modified. Once set, you cannot change the value assigned to a primary key, nor can you assign a different key as the primary key. Use of this option also changes the primary key collation default; primary key string values default to EXACT collation. If FALSE, the primary key and ID key are defined as independent, primary key values are changeable, and primary key string values default to SQLUPPER collation.

To set the PKEY\_IS\_IDKEY option, you must have the %Admin\_Manage:USE privilege. Otherwise, you receive an SQLCODE -99 error (Privilege Violation). Once set, this option takes effect system-wide for all processes. The system-wide default for this option can be set using:

*   The [$SYSTEM.SQL.SetDDLPKeyNotIDKey()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLPKeyNotIDKey) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings).
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Are Primary Keys Created through DDL not ID Keys. If set to “Yes” (1), when a Primary Key constraint is specified through DDL it does not automatically become the IDKey index in the class definition. If “No” (0), it does become the IDKey index. Setting this value to “No” can give better performance, but means that the Primary Key fields cannot be updated. “Yes” is the default.
    

The PKEY\_IS\_IDKEY setting remains in effect until reset through another SET OPTION PKEY\_IS\_IDKEY or until the Caché Configuration is reactivated, which resets this parameter to the Caché System Configuration setting.

SUPPORT\_DELIMITED\_IDENTIFIERS

The SUPPORT\_DELIMITED\_IDENTIFIERS boolean option specifies whether delimited identifiers are supported system-wide. Available values are TRUE and FALSE. If TRUE, a string delimited by double quotation marks is considered an identifier within an SQL statement. If FALSE, a string delimited by double quotation marks is considered a string literal within an SQL statement.

To set the SUPPORT\_DELIMITED\_IDENTIFIERS option, you must have the %Admin\_Manage:USE privilege. Otherwise, you receive an SQLCODE -99 error (Privilege Violation). Once set, this option takes effect system-wide for all processes. The SUPPORT\_DELIMITED\_IDENTIFIERS setting remains in effect until reset through another SET OPTION SUPPORT\_DELIMITED\_IDENTIFIERS or until the Caché Configuration is reactivated, which will reset this parameter to the System Configuration setting in the Management Portal.

The system-wide default for this option can be set as follows:

*   The [$SYSTEM.SQL.SetDelimitedIdentifiers()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDelimitedIdentifiers) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings).
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Support Delimited Identifiers.
    

The default is “Yes” (1). If set to “Yes”, delimited identifiers are supported system-wide. For further details on [delimited identifiers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers), see the “Identifiers” chapter of Using Caché SQL.

Locale Options

Locale options are keyword options used to set your Caché Locale settings for date, time, and numeric conventions for the current process. The available keyword options are AM, DATE\_FORMAT, DATE\_MAXIMUM, DATE\_MINIMUM, DATE\_SEPARATOR, DECIMAL\_SEPARATOR, MIDNIGHT, MINUS\_SIGN, MONTH\_ABBR, MONTH\_NAME, NOON, NUMERIC\_GROUP\_SEPARATOR, NUMERIC\_GROUP\_SIZE, PM, PLUS\_SIGN, TIME\_FORMAT, TIME\_PRECISION, TIME\_SEPARATOR, WEEKDAY\_ABBR, WEEKDAY\_NAME, and YEAR\_OPTION. All of these options can be set to a literal, and all take a default (American English conventions). The TIME\_PRECISION option is configurable (see below). If you set any of these options to an invalid value, Caché issues an SQLCODE -129 error (Illegal value for SET OPTION locale property). See the Caché ObjectScript [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function for an explanation of date and time formats and options.

Date/Time Option Keyword

Description

AM

String. Default is 'AM'

DATE\_FORMAT

Integer. Default is 1. Available values are 0 through 15. For an explanation of these date formats, see the Caché ObjectScript [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) function.

DATE\_MAXIMUM

Integer. Default is 2980013 (12/31/9999). Can be set to an earlier date, but not to a later date.

DATE\_MINIMUM

Positive Integer. Default is 0 (12/31/1840). Can be set to a later date, but not to an earlier date.

DATE\_SEPARATOR

Character. Default is '/'

DECIMAL\_SEPARATOR

Character. Default is '.'

MIDNIGHT

String. Default is 'MIDNIGHT'

MINUS\_SIGN

Character. Default is '-'

MONTH\_ABBR

String. Default is ' Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec'. (Note that this string begins with a space character, which is the default separator character.)

MONTH\_NAME

String. Default is ' January February March April May June ... November December'. (Note that this string begins with a space character, which is the default separator character.)

NOON

String. Default is 'NOON'

NUMERIC\_GROUP\_SEPARATOR

Character. Default is ','

NUMERIC\_GROUP\_SIZE

Integer. Default is 3.

PM

String. Default is 'PM'

PLUS\_SIGN

Character. Default is '+'

TIME\_FORMAT

Integer. Default is 1. Available values are 1 through 4. For an explanation of these time formats, see the Caché ObjectScript [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function.

TIME\_PRECISION

Integer from 0 through 9 (inclusive). Default is 0. The number of digits of fractional seconds. Configurable, as described below.

TIME\_SEPARATOR

Character. Default is ':'

WEEKDAY\_ABBR

String. Default is ' Sun Mon Tue Wed Thu Fri Sat'. (Note that this string begins with a space character, which is the default separator character.)

WEEKDAY\_NAME

String. Default is ' Sunday Monday Tuesday Wednesday Thursday Friday Saturday'. (Note that this string begins with a space character, which is the default separator character.)

YEAR\_OPTION

Integer. Default is 0. Available values are 0 through 6. For an explanation of these ways of representing 2-digit and 4-digit years, see the Caché ObjectScript [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) function.

To configure TIME\_PRECISION system-wide, go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Default time precision for GETDATE(), CURRENT\_TIME, and CURRENT\_TIMESTAMP. This specifies the number of digits of precision for fractional seconds. The default is 0. The range of allowed values is 0 through 9 digits of precision. The actual number of meaningful digits of fractional seconds is platform-dependent.

See Also

*   SQL date and time functions: [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp) [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) [GETDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate) [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now)
    
*   SQL date functions: [DAYNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname) [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek) [DAYOFMONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth) [DAYOFYEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear) [WEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_week) [MONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month) [MONTHNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_monthname) [QUARTER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_quarter) [YEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_year) [CURDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curdate) [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    
*   SQL time functions: [HOUR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour) [MINUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minute) [SECOND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_second) [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime) [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime)
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    
*   Caché ObjectScript functions: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_setoption.xml**