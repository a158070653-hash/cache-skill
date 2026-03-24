# $ZCHILD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzchild

---

 

Caché ObjectScript Reference

$ZCHILD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZCHILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild "$ZCHILD") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the ID of the last child process.

Synopsis

$ZCHILD
$ZC 

Description

$ZCHILD contains the ID of the last child process that the current process created with the JOB command. If your process has not used JOB to create a child process, $ZCHILD returns 0 (zero).

In MSM language mode, the $ZC special variable (spelled as shown) has a different use. It is used for determining end-of-file in sequential file reads.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Notes

$ZCHILD and the Successful Starting of Jobs

$ZCHILD being set does not mean that the job was successfully started. It only means that the process was created and the parameters were passed successfully.

For example, if you use JOB to spawn a routine that does not exist, both $TEST and $ZCHILD report that the JOB command succeeded, although that new job immediately dies with a <NOROUTINE> error.

$ZC in MSM Language Mode

MSM language mode supports a special use of the $ZC special variable.

If you have set the current language mode to MSM using the [LanguageMode(8)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#LanguageMode) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class, the $ZC special variable is set during sequential file reads. This provides compatibility with the MSM $ZC variable. (In all other language modes, $ZC is not set during file reads; $ZC is an abbreviation for $ZCHILD and has a completely different functionality.)

In MSM language mode, a successful sequential file read sets $ZC\=0.

In MSM language mode, an end-of-file condition sets $ZC\=–1 (negative 1). An <ENDOFFILE> error does not occur.

However, Caché $ZC is not identical to the MSM $ZC:

MSM sets its $ZC\=–1 (negative 1) if the last line of the file is not terminated with the delimiter character(s). Caché does not check for delimiter characters; it sets $ZC\=0 instead of –1 in this case.

MSM sets its $ZC\=1 if an I/O error occurs during a read. Caché does not support this functionality; instead, Caché issues a <READ> error.

See Also

Child processes:

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [^$JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_sjob) structured system variable
    
*   [$JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vjob) special variable
    
*   [$TEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest) special variable
    
*   [$ZPARENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzparent) special variable
    

MSM language mode:

*   [$ZB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb) special variable
    
*   [$ZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof) special variable
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzchild.xml**