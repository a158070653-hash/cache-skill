# $STORAGE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vstorage

---

 

Caché ObjectScript Reference

$STORAGE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$STORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage "$STORAGE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the number of bytes available for local variable storage.

Synopsis

$STORAGE
$S

Description

$STORAGE returns the number of bytes available for local variable storage in the current process partition. The initial value of $STORAGE is established by the value of $ZSTORAGE, the maximum amount of memory available to the process. The larger the $ZSTORAGE value (in kilobytes), the larger the $STORAGE value (in bytes). However, this relationship between $ZSTORAGE and $STORAGE is not a simple 1:1 ratio.

The $STORAGE value is affected by the following operations:

*   $STORAGE decreases as local variables are defined in the local variable space, for example, by using the SET command. The decrease in $STORAGE corresponds to the amount of space required to store the value of the local variable; the size of the name of the local variable has no effect on $STORAGE, but the number of subscript levels does affect $STORAGE. The $STORAGE value increases as local variables are removed, for example, by using the KILL command.
    
*   $STORAGE decreases when you issue a [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command. NEW establishes a new execution level; space set aside for local variables (whether or not used) at the previous execution level is not available at the new execution level. The initial NEW decreases $STORAGE by approximately 15000; each subsequent NEW decreases $STORAGE by 12288. The $STORAGE value increases when you issue a QUIT command to exit an execution level.
    
*   $STORAGE decreases when you define a flow-of-control statement, such as IF or FOR, or a block structure such as TRY and CATCH. Storage is allocated to compile these structures, not to execute them. Therefore, a FOR statement consumes the same amount of storage regardless whether it loops or how many times it loops; each IF, ELSEIF, and ELSE clause consumes a set amount of storage, regardless of how many branches are executed. The space is allocated from the process that compiled the code. Note that a FOR loop commonly defines a local variable as a counter.
    

The $STORAGE value is not affected by setting [process-private variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls), global variables, or special variables. The $STORAGE value is not affected by changing namespaces. The $STORAGE value is not affected by enabling long strings because long string storage is not allocated in the process partition. For further details, refer to [Long Strings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) in the “Data Types and Values” chapter of Using Caché ObjectScript.

The $STORAGE special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Low Memory and <STORE> Errors

The $STORAGE value may be a positive or negative number. A value of zero does not indicate no available storage, but indicates that storage is in extremely short supply. If $STORAGE decreases to less than zero, at some point a <STORE> error occurs. For example, if $STORAGE decreases to -7000, allocating storage for another local variable might fail due to a <STORE> error, indicating insufficient available storage space to store a local variable value, or to establish a new execution level.

The first <STORE> error occurs when $STORAGE is some value less than zero; the exact negative $STORAGE value threshold depends upon context. This <STORE> error indicates that you must get additional storage, either by increasing [$ZSTORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage), or by freeing some allocated storage through KILL or QUIT operations. When this first <STORE> error occurs, Caché automatically makes 1Mb of additional memory available to the process to enable error processing and recovery. Caché does not change $ZSTORAGE; it allows $STORAGE to go further into negative number values.

When this first <STORE> error occurs, Caché internally designates the process as being in a low memory state. While in this low memory state the process may continue to allocate memory and the value of $STORAGE may continue to decrease into lower negative numbers. While in this low memory state the process may free some allocated memory, causing the value of $STORAGE to rise. Thus, the value of $STORAGE may rise or fall within a range of values without issuing additional <STORE> errors. Also, after the first <STORE> error you may see a small rise in $STORAGE caused by Caché freeing some internal memory.

This first <STORE> error provides some memory cushion that allows your process to call diagnostics, perform saves to disk, exit gracefully, free memory, and continue.

A process remains in a low memory state until either of the following occurs:

*   The process makes available sufficient memory. Your process can do this by increasing the [$ZSTORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage) allocation, and/or by freeing allocated storage through KILL or QUIT operations. When the value of $STORAGE exceeds 256K (or 25% of $ZSTORAGE, whichever is smaller), Caché removes the process from low memory state. At that point the process can again issue a <STORE> error if the available memory decreases into negative numbers.
    
*   The process consumes the additional memory. When the value of $STORAGE reaches -1048576, a second <STORE> error occurs. If your process arrives at this point, no more memory is available to the process and further process operations become unpredictable. It is likely the process will immediately terminate.
    

You can determine the reason for a <STORE> error by calling the [$SYSTEM.Process.MemoryAutoExpandStatus()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#MemoryAutoExpandStatus) method.

Examples

The following example shows how $STORAGE becomes smaller when $ZSTORAGE is set to a smaller value. Note that the relationship (ratio) between these two values is variable:

  SET $ZS\=262144
  FOR i\=1:1:10 {
          WRITE "$ZS=",$ZS," $S=",$S," ratio=",$NORMALIZE($S/$ZS,3),!
          IF $ZS\>30000 {SET $ZS\=$ZS\-30000 }
  }

 

The following example shows how $STORAGE decreases as local variables are assigned, and increases when local variables are killed:

  WRITE "$STORAGE=",$S," initial value",!
  FOR i\=1:1:30 {SET a(i)\="abcdefghijklmnopqrstuvwxyz"
    WRITE "$STORAGE=",$S,! }
  KILL a
  WRITE !,"$STORAGE=",$S," after KILL",!

 

The following example shows how the number of subscript levels of an assigned local variable affect $STORAGE:

  WRITE "No subscripts:",!
  SET before\=$S
  SET a\="abcdefghijklmnopqrstuvwxyz"
  WRITE " memory allocated ",before\-$S,!
  KILL a
  WRITE "One subscript level:",!
  SET before\=$S
  SET a(1)\="abcdefghijklmnopqrstuvwxyz"
  WRITE " memory allocated ",before\-$S,!
  KILL a(1)
  WRITE "Nine subscript levels:",!
  SET before\=$S
  SET a(1,2,3,4,5,6,7,8,9)\="abcdefghijklmnopqrstuvwxyz"
  WRITE " memory allocated ",before\-$S,!
  KILL a(1,2,3,4,5,6,7,8,9)

 

The following example shows how $STORAGE decreases (becomes unavailable at that level) as NEW establishes a new execution level:

   WRITE "increasing levels:",!
   FOR i\=1:1:10 {WRITE "$STORAGE=",$S,! NEW }

 

The following example shows how $STORAGE decreases as local variables are assigned until it enters low memory state, issuing a <STORE> error. The <STORE> error is caught by a CATCH block that invokes the StoreErrorReason() method to determine what caused the error. Note that entering the CATCH block consumes a significant amount of storage. Once in the CATCH block, this example allocates one more variable.

  TRY {
    WRITE !,"TRY block",!
    SET init\=$ZSTORAGE
    SET $ZSTORAGE\=456
    WRITE "Initial $STORAGE=",$STORAGE,!
    FOR i\=1:1:1000 {
       SET pre\=$STORAGE
       SET var(i)\="1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
       IF $STORAGE<0 {WRITE "var(",i,") negative memory=",$STORAGE,! }
       ELSEIF pre<$STORAGE {WRITE "var(",i,") new allocation $S=",$STORAGE,! }
       ELSE {WRITE "var(",i,") $S=",$STORAGE,! }
    }
  }
  CATCH myexp {
      WRITE !,"CATCH block exception handler",!!
      WRITE "Name: ",$ZCVT(myexp.Name,"O","HTML"),!
        IF myexp.Name\="<STORE>" {WRITE "store error reason=",
                                 $SYSTEM.Process.StoreErrorReason(),! }
      WRITE "$S=",$STORAGE,!
      SET j\=i
      SET var(j)\="1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
      WRITE "var(",j,") added one more variable $S=",$STORAGE,!
      SET $ZSTORAGE\=init
      RETURN
  }

 

See Also

*   [$ZSTORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage) special variable
    
*   [Local variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local) in the “Variables” chapter of Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vstorage.xml**