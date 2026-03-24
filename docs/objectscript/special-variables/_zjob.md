# $ZJOB

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzjob

---

 

Caché ObjectScript Reference

$ZJOB

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzio "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzmode "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZJOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob "$ZJOB") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains job status information.

Synopsis

$ZJOB
$ZJ

Description

$ZJOB contains a number in which each bit represents one particular aspect of the job’s status. $ZJOB returns an integer that consists of the total of the set status bits. For example, if $ZJOB = 5, this means that the 1 bit and the 4 bit are set.

To test individual $ZJOB bit settings, you can use the [integer divide](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_intdiv) (\\) and [modulo](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_modulo) (#) operators. For example, $ZJOB\\x#2, where x is the bit number. The following table shows the layout of the bits (by bit positional value), their settings and meanings:

Bit

Set to

Meaning

1

1

Job started in programmer mode.

0

Job started in application mode.

2

1

Job started by the JOB command.

0

Job started by signing on either in application or programmer mode.

4

1

<INTERRUPT> enabled. A CTRL-C can interrupt a running program. Refer to [BREAK flag](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_flag) for details.

0

<INTERRUPT> disabled except for terminal lines for which <INTERRUPT> has been explicitly enabled by OPEN or USE commands.

8

1

<INTERRUPT> received and pending.

0

<INTERRUPT> not received. The value 8 is cleared by the OPEN and USE commands and by an error trap caused by a CTRL-C.

1024

1

Journaling is disabled regardless of other conditions.

0

Journaling is enabled for this job if other conditions indicate journaling.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Examples

The following example returns $ZJOB as an integer:

  WRITE $ZJOB

 

The following example returns each $ZJOB bit value:

  WRITE "   bit 1=",$ZJOB\\1#2,!
  WRITE "   bit 2=",$ZJOB\\2#2,!
  WRITE "   bit 4=",$ZJOB\\4#2,!
  WRITE "   bit 8=",$ZJOB\\8#2,!
  WRITE "bit 1024=",$ZJOB\\1024#2

 

Bit 1 can also be returned using $ZJOB#2.

See Also

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [$JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vjob) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzio "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzmode "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzjob.xml**