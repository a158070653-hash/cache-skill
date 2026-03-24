# $ZSTORAGE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzstorage

---

 

Caché ObjectScript Reference

$ZSTORAGE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzreference "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZSTORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage "$ZSTORAGE") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the maximum available memory for a process.

Synopsis

$ZSTORAGE
$ZS

Description

$ZSTORAGE contains the maximum amount of memory, in Kbytes, for a job’s process-private memory. This memory is available for local variables, stacks, and other tables. This memory limit does not include space for the routine object code. This memory is allocated to the process as needed, for example when allocating an array.

Once this memory is allocated to the process, it is generally not deallocated until the process exits. However, when a large amount of memory is used (for example greater than 32MB) and then freed, Caché attempts to release deallocated memory back to the operating system, where possible.

You can also use $ZSTORAGE to set the maximum memory size. For example, the following statement sets the job’s maximum process-private memory to 524288 Kbytes:

   SET $ZSTORAGE\=524288

Changing $ZSTORAGE changes the initial value for the [$STORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage) special variable, which contains the current available memory (in bytes) for the process.

The maximum value for $ZSTORAGE is 2147483647. The $ZSTORAGE default is 262144. The minimum value for $ZSTORAGE is 128. $ZSTORAGE values larger than the maximum or smaller than the minimum automatically default to the maximum or minimum value. $ZSTORAGE is set to an integer value; Caché truncates any fractional portion (if specified).

You can change the $ZSTORAGE default by changing the Maximum per Process Memory (KB) system configuration setting. In the Management Portal, go to \[Home\] > \[Configuration\] > \[Memory and Startup\]. You can increase Maximum per Process Memory (KB) as desired, up to a maximum of 2147483647 Kbytes. Changing Maximum per Process Memory (KB) changes the $ZSTORAGE value for subsequently initiated processes; it has no effect on the $ZSTORAGE value for current processes.

Example

The following example sets $ZSTORAGE to its maximum and minimum values. Attempting to set $ZSTORAGE to a value (16) that is less than the minimum value automatically sets $ZSTORAGE to its minimum value (128):

  SET $ZS\=128
  WRITE "minimum storage=",$ZS,!
  SET $ZS\=16
  WRITE "less than minimum storage=",$ZS,!
  SET $ZS\=2147483647
  WRITE "maximum storage=",$ZS,!

 

See Also

[SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command

[KILL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill) command

[$STORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage) special variable

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzreference "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzstorage.xml**