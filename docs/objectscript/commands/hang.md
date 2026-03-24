# HANG

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_chang

---

 

Caché ObjectScript Reference

HANG

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [HANG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chang "HANG") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Suspends execution for a specified number of seconds.

Synopsis

HANG:pc hangarg
H:pc hangarg

where hangarg can be:

hangtime,...

Arguments

pc

Optional — A postconditional expression.

hangtime

The amount of time to wait, in seconds. An expression that resolves to a positive numeric value, or a comma-separated list of numeric expressions.

Description

HANG suspends the executing routine for the specified time period. If there are multiple arguments, Caché suspends execution for the duration of each argument in the order presented. The HANG time is calculated using the system clock, which determines its precision.

HANG has the same minimum abbreviation (H) as the HALT command. HANG is distinguished by its required hangtime argument.

Arguments

pc

An optional postconditional expression. Caché executes the HANG command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

hangtime

The amount of time to wait, in seconds. This time can be expressed as any numeric expression. You can specify hangtime as an integer to specify whole seconds, or as fractional number to specify fractional seconds. You can use exponentiation (\*\*), arithmetic expressions, and other numeric operators.

You can set hangtime to 0 (zero), in which case no hang is performed. Setting hangtime to a negative number or a nonnumeric value is the same as setting it to 0.

You can specify multiple hangtime arguments as a comma-separated list, as described below.

Examples

The following example suspends the process for 10 seconds:

   WRITE !,$ZTIME($PIECE($HOROLOG,",",2))
   HANG 10
   WRITE !,$ZTIME($PIECE($HOROLOG,",",2))

 

The following example suspends the process for 1/2 second. $ZTIMESTAMP, unlike $HOROLOG, can return fractional seconds if the precision parameter of the $ZTIME function is specified.

   WRITE !,$ZTIME($PIECE($ZTIMESTAMP,",",2),1,2)
   HANG .5
   WRITE !,$ZTIME($PIECE($ZTIMESTAMP,",",2),1,2)

 

Returns values such as the following:

14:34:19.75
14:34:20.25

Notes

Multiple HANG Arguments

You can specify hangtime as a comma-separated list of numeric expressions. Caché suspends execution for the duration of each argument in the order presented. Negative numbers are treated as zero. Therefore, a hangtime of 16,-15 would hang for 16 seconds.

That each hangtime argument is separately executed can affect operations that use the current time in hang calculations, as shown in the following example:

  SET start\=$ZHOROLOG
  SET a\=$ZHOROLOG+5
  HANG 4,a\-$ZHOROLOG
  SET end\=$ZHOROLOG
  WRITE !,"elapsed hang=",end\-start

 

In this example, HANG first suspends execution for 4 seconds, then suspends execution for the current time before the hang plus 5 seconds, minus the current time when the second hang argument is parsed. Because HANG executes each argument in turn, the total hang time in this example is (roughly) 5 seconds, rather than the (roughly) 9 seconds one might otherwise expect.

HANG Compared with Timed READ

You can use HANG to pause the routine while the user reads an output message. However, you can handle this type of pause more effectively with a timed READ command. A timed READ allows the user to continue when ready, but a HANG does not because it is set to a fixed duration.

See Also

*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_chang.xml**