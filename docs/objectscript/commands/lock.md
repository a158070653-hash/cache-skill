# LOCK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_clock

---

 

Caché ObjectScript Reference

LOCK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmerge "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock "LOCK") \]

Go to: Description Arguments Using the Lock Table to View and Delete Locks System-wide Incremental Locking and Unlocking Locks on Global Variables Local Variable Locks See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Enables a process to apply and release locks to control access to data resources.

Synopsis

LOCK:pc
L:pc

LOCK:pc +lockname#locktype:timeout,...
L:pc +lockname#locktype:timeout,...

LOCK:pc +(lockname#locktype,...):timeout,...
L:pc +(lockname#locktype,...):timeout,...

Arguments

pc

Optional — A postconditional expression.

+

–

Optional — The [lock operation indicator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_operation) (a + character, – character, or no character) to apply or remove a lock. A + (plus sign) applies the specified lock(s) without unlocking any prior locks. This can be used to apply an [incremental lock](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_incremental). A – (minus sign) unlocks (or decrements) a lock. If you omit the lock operation indicator (no character), Caché unlocks all prior locks and applies the specified lock(s).

lockname

A [lock name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_lockname) associated with the resource(s) to be locked or unlocked. Must be a valid identifier, following the same naming conventions as local variables or globals.

#locktype

Optional — A [letter code](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_locktype) specifying the type of lock to lock or unlock, specified in quotation marks. Available values are “S” (shared lock), ”E” (escalating lock), “I” (immediate unlock), and “D” (deferred unlock). When specifying, the preceding # symbol is mandatory. For example, #"S". You can specify more than one letter code. For example, #"SEI". “S” and “E” are specified for both locking and unlocking operations; “I” and “D” are only specified for unlocking operations. If omitted, the lock type defaults to an exclusive lock (non-S) that does not escalate (non-E) and that always defers releasing an unlocked lock to the end of the current transaction (non-I / non-D).

:timeout

Optional — The time to wait before the attempted lock operation times out. Can be specified with or without the optional #locktype. When specifying, the preceding : symbol is mandatory. For example, LOCK ^a(1):10 or LOCK ^a(1)#"E":10. Specify [timeout](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_timeout) as an integer number of seconds. A value of 0 means to make one attempt, then time out. Fractional seconds are truncated to the integer portion. If omitted, Caché waits indefinitely.

Description

There are two basic forms of the LOCK command:

*   [Without arguments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_argumentless)
    
*   [With arguments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_with_argument)
    

LOCK without Arguments

The argumentless LOCK releases (unlocks) all locks currently held by the process. This includes exclusive and shared locks, both local and global. It also includes all accumulated incremental locks. For example, if there are three incremental locks on a given lock name, Caché releases all three locks and removes the lock name entry from the lock table.

If you issue an argumentless LOCK during a transaction, Caché places all locks currently held by the process in a Delock state until the end of the transaction. When the transaction ends, Caché releases the locks and removes the corresponding lock name entries from the lock table.

The following example applies various locks during a transaction, then issues an argumentless LOCK to release all of these locks. The locks are placed in a Delock state until the end of the transaction. The HANG commands give you time to check the lock’s ModeCount in the Lock Table:

  TSTART
  LOCK +^a(1)      // ModeCount: Exclusive
  HANG 2
  LOCK +^a(1)#"E"  // ModeCount: Exclusive/1+1e
  HANG 2
  LOCK +^a(1)#"S"  // ModeCount: Exclusive/1+1e,Shared
  HANG 2
  LOCK             // ModeCount: Exclusive/1+1e->Delock,Shared->Delock
  HANG 10
  TCOMMIT          // ModeCount: locks removed from table

 

Argumentless LOCK releases all locks held by the process without applying any locks. Completion of a process also releases all locks held by that process.

LOCK with Arguments

LOCK with arguments specifies one or more lock names on which to perform locking and unlocking operations. What lock operation Caché performs depends on the [lock operation indicator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_operation) argument you use:

*   LOCK lockname unlocks all locks previously held by the process and applies a lock on the specified lock name(s).
    
*   LOCK +lockname applies a lock on the specified lock name(s) without unlocking any previous locks. This allows you to accumulate different locks, and allows you to apply [incremental locks](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_incremental) to the same lock.
    
*   LOCK \-lockname performs an unlock operation on the specified lock name(s). Unlocking decrements the lock count for the specified lock name; when this lock count decrements to zero, the lock is released.
    

A lock operation may immediately apply the lock, or it may place the lock request on a wait queue pending the release of a conflicting lock by another process. A waiting lock request may time out (if you specify a [timeout](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_timeout)) or may wait indefinitely (until the end of the process).

LOCK with Multiple Lock Names

You can specify multiple locks with a single LOCK command in either of two ways:

*   Without Parentheses: By specifying multiple lock arguments without parentheses as a comma-separated list, you can specify multiple independent lock operations, each of which can have its own [timeout](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_timeout). (This is functionally identical to specifying a separate LOCK command for each lock argument.) Lock operations are performed in strict left-to-right order. For example:
    
      LOCK var1(1):10,+var2(1):15
    
     
    
    Multiple lock arguments without parentheses each can have their own [lock operation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_operation) indicator and their own [timeout](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_timeout) argument. However, if you use multiple lock arguments, be aware that a lock operation without a plus sign [lock operation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_operation) indicator unlocks all prior locks, including locks applied by an earlier part of the same LOCK command. For example, the command LOCK ^b(1,1), ^c(1,2,3), ^d(1) would be parsed as three separate lock commands: the first releasing the processes’ previously held locks (if any) and locking ^b(1,1), the second immediately releasing ^b(1,1) and locking ^c(1,2,3), the third immediately releasing ^c(1,2,3) locking ^d(1). As a result, only ^d(1) would be locked.
    
*   With Parentheses: By enclosing a comma-separated list of lock names in parentheses, you can perform these locking operations on multiple locks as a single atomic operation. For example:
    
      LOCK +(var1(1),var2(1)):10
    
     
    
    All lock operations in a parentheses-enclosed list are governed by a single [lock operation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_operation) indicator and a single [timeout](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_timeout) argument; either all of the locks are applied or none of them are applied. A parentheses-enclosed list without a plus sign lock operation indicator unlocks all prior locks then locks all of the listed lock names.
    

Arguments

pc

An optional postconditional expression that can make the command conditional. Caché executes the LOCK command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). You can specify a postconditional expression on an argumentless LOCK command or a LOCK command with arguments. For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

lock operation indicator

The lock operation indicator is used to apply (lock) or remove (unlock) a lock. It can be one of the following values:

No character

Unlock all prior locks belonging to the current process and attempt to apply the specified lock. For example, LOCK ^a(1) performs the following atomic operation: it releases all locks previously held by the process (whether local or global, exclusive or shared, escalating or non-escalating) and it attempts to lock ^a(1). This can result in one of two outcomes: (1) all prior locks unlocked and ^a(1) locked; (2) all prior locks unlocked and ^a(1) placed in a lock waiting state pending the release of a conflicting lock held by another process.

Plus sign (+)

Attempt to apply the specified lock without performing any unlocks. This allows you to add a lock to the locks held by the current process. One use of this option is to perform [incremental locking](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_incremental) of a lock name.

Minus sign (\-)

Unlocks the specified lock. If the lock name has a lock count of 1, an unlock remove the lock from the lock table. If the lock name has a lock count of more than 1, an unlock removes one of its incremental locks (decrements the lock count). By default, this unlocks an exclusive, non-escalating lock. To remove a shared lock and/or an escalating lock, you must specify the corresponding #[locktype](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock#RCOS_clock_locktype).

If your LOCK command contains multiple comma-separated lock arguments, each lock argument can have its own lock operation indicator. Caché parses this as multiple independent LOCK commands.

lockname

A lockname is the name of a lock for a data resource; it is not the data resource itself. That is, your program can specify a lock named ^a(1) and a variable named ^a(1) without conflict. The relationship between the lock and the data resource is a programming convention; by convention, processes must acquire the lock before modifying the corresponding data resource.

Lock names are case sensitive. Lock names follow the same naming conventions as the corresponding local variables and global variables. A lock name can be subscripted or unsubscripted. Lock subscripts have the same naming conventions and maximum length and number of levels as [variable subscripts](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_subscripts). In Caché, the following are all valid and unique lock names: a, a(1), A(1), ^a, ^a(1,2), ^A(1,1,1). For further details, see the [Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables) chapter of Using Caché ObjectScript.

Note:

For performance reasons, it is recommended you specify lock names with subscripts whenever possible. For example, ^a(1) rather than ^a. Subscripted lock names are used in documentation examples.

Lock names can be local or global. A lock name such as A(1) is a local lock name. It applies only to that process, but does apply across namespaces. A lock name that begins with a caret (^) character is a global lock name; the mapping for this lock follows the same mapping as the corresponding global, and thus can apply across processes, controlling their access to the same resource. (See [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.)

Note:

Process-private global names can not be used as lock names. Attempting to use a process-private global name as a lock name performs no operation and completes without issuing an error.

A lock name can represent a local or global variable, subscripted or unsubscripted. It can be an implicit global reference, or an extended reference to a global on another computer. (See [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.)

The data resource corresponding to a lock name does not need to exist. For example, you may lock the lock name ^a(1,2,3) whether or not a global variable with the same name exists. Because the relationship between locks and data resources is an agreed-upon convention, a lock may be used to protect a data resource with an entirely different name.

locktype

A letter code specifying the type of lock to apply or remove. locktype is an optional argument; if you omit locktype, the lock type defaults to an exclusive non-escalating lock. If you omit locktype, you must omit the pound sign (#) prefix. If you specify locktype, the syntax for lock type is a mandatory pound sign (#), followed by quotation marks enclosing one or more lock type letter codes. Lock type letter codes can be specified in any order and are not case sensitive. The following are the lock type letter codes:

*   S: Shared lock
    
    Allows multiple processes to simultaneously hold nonconflicting locks on the same resource. For example, two (or more) processes may simultaneously hold shared locks on the same resource, but an exclusive lock limits the resource to one process. An existing shared lock prevents all other processes from applying an exclusive lock, and an existing exclusive lock prevents all other processes from applying a shared lock on that resource. However, a process can first apply a shared lock on a resource and then the same process can apply an exclusive lock on the resource, upgrading the lock from shared to exclusive. Shared and Exclusive lock counts are independent. Therefore, to release such a resource it is necessary to release both the exclusive lock and the shared lock. All locking and unlocking operations that are not specified as shared (“S”) default to exclusive.
    
    A shared lock may be incremental; that is, a process may issue multiple shared locks on the same resource. You may specify a shared lock as escalating (“SE”) when locking and unlocking. When unlocking a shared lock, you may specify the unlock as immediate (“SI”) or deferred (“SD”). To view the current shared locks with their increment counts for escalating and non-escalating lock types, refer to the system-wide lock table, described in the “[Lock Management](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_locktable)” chapter of Using Caché ObjectScript.
    
*   E: Escalating lock
    
    Allows you to apply a large number of concurrent locks without overflowing the lock table. By default, locks are non-escalating. When applying a lock, you can use locktype “E” to designate that lock as escalating. When releasing an escalating lock, you must specify locktype “E” in the unlock statement. You can designate both exclusive locks and shared (“S”) locks as escalating.
    
    Commonly, you would use escalating locks when applying a large number of concurrent locks at the same subscript level. For example LOCK +^mylock(1,1)#"E",+^mylock(1,2)#"E",+^mylock(1,3)#"E"....
    
    The same lock can be concurrently applied as a non-escalating lock and as an escalating lock. For example, ^mylock(1,1) and ^mylock(1,1)#"E". Caché counts locks issued with locktype “E” separately in the lock table. For information on how escalating and non-escalating locks are represented in the lock table, refer to the “[Lock Management](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_locktable)” chapter of Using Caché ObjectScript.
    
    When the number of “E” locks at a subscript level reaches a threshold number, the next “E” lock requested for that subscript level automatically attempts to lock the parent node (the next higher subscript level). If it cannot, no escalation occurs. If it successfully locks the parent node, it establishes one parent node lock with a lock count corresponding to the number of locks at the lower subscript level, plus 1. The locks at the lower subscript level are released. Subsequent “E” lock requests to the lower subscript level further increment the lock count of this parent node lock. You must unlock all “E” locks that you have applied to decrement the parent node lock count to 0 and de-escalate to the lower subscript level. The default lock threshold is 1000 locks; lock escalation occurs when the 1001st lock is requested.
    
    Note that once locking is escalated, lock operations preserve only the number of locks applied, not what specific resources were locked. Therefore, failing to unlock the same resources that you locked can cause “E” lock counts to get out of sync.
    
    In the following example, lock escalation occurs when the program applies the lock threshold + 1 “E” lock. This example shows that lock escalation both applies a lock on the next-higher subscript level and releases the locks on the lower subscript level:
    
    Main
      TSTART
      SET thold\=$SYSTEM.SQL.GetLockThreshold()
      WRITE "lock escalation threshold is ",thold,!
      SET almost\=thold\-5
      FOR i\=1:1:thold+5 { LOCK +dummy(1,i)#"E"
         IF i\>almost {
           IF ^$LOCK("dummy(1,"\_i\_")","OWNER") '= "" {WRITE "lower level lock applied at ",i,"th lock ",! }
           ELSEIF ^$LOCK("dummy(1)","OWNER") '= ""   {WRITE "lock escalation",!
                                                      WRITE "higher level lock applied at ",i,"th lock ",!
                                                      QUIT }
           ELSE {WRITE "No locks applied",! }
         }
      }
      TCOMMIT
    
     
    
    Note that only “E” locks are counted towards lock escalation. The following example applies both default (non-“E”) locks and “E” locks on the same variable. Lock escalation only occurs when the total number of “E” locks on the variable reaches the lock threshold:
    
    Main
      TSTART
      SET thold\=$SYSTEM.SQL.GetLockThreshold()
      WRITE "lock escalation threshold is ",thold,!
      SET noE\=17
      WRITE "setting ",noE," non-escalating locks",!
      FOR i\=1:1:thold+noE { IF i < noE {LOCK +a(6,i)}
                      ELSE {LOCK +a(6,i)#"E"}
        IF ^$LOCK("a(6)","OWNER") '= "" {
           WRITE "lock escalation on lock a(6,",i,")",!
           QUIT }
      }
      TCOMMIT
    
     
    
    Unlocking “E” locks is the reverse of the above. When locking is escalated, unlocks at the child level decrement the lock count of the parent node lock until it reaches zero (and is unlocked); these unlocks decrementing a count, they are not matched to specific locks. When the parent node lock count reaches 0, the parent node lock is removed and “E” locking de-escalates to the lower subscript level. Any subsequent locks at the lower subscript level create specific locks at that level.
    
    The “E” locktype can be combined with any other locktype. For example, “SE”, “ED”, “EI”, “SED”, “SEI”. When combined with the “I” locktype it permits unlocks of “E” locks to occur immediately when invoked, rather than at the end of the current transaction. This “EI” locktype can minimize situations where locking is escalated.
    
    Commonly, “E” locks are automatically applied for SQL [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert), [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update), and [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) operations within a transaction. However, there are specific limitations on SQL data definition structures that support “E” locking. Refer to the specific SQL commands for details.
    
*   I: Immediate unlock
    
    Immediately releases a lock, rather than waiting until the end of a transaction:
    
    *   Specifying “I” when unlocking a non-incremented (lock count 1) lock immediately releases the lock. By default, an unlock does not immediately release a non-incremented lock. Instead, when you unlock a non-incremented lock Caché maintains that lock in a delock state until the end of the transaction. Specifying “I” overrides this default behavior.
        
    *   Specifying “I” when unlocking an incremented lock (lock count > 1) immediately releases the incremental lock, decrementing the lock count by 1. This is the same behavior as a default unlock of an incremented lock.
        
    
    The “I” locktype is used when performing an unlock during a transaction. It has the same effect on Caché unlock behavior whether the lock was applied within the transaction or outside of the transaction. The “I” locktype performs no operation if the unlock occurs outside of a transaction. Outside of a transaction, an unlock always immediately releases a specified lock.
    
    “I” can only be specified for an unlock operation; it cannot be specified for a lock operation. “I” can be specified for an unlock of a shared lock (#"SI") or an exclusive lock (#"I"). Locktypes “I” and “D” are mutually exclusive. “IE” can be used to immediately unlock an escalating lock.
    
    This immediate unlock is shown in the following example:
    
      TSTART
      LOCK +^a(1)      // apply lock ^a(1)
      LOCK \-^a(1)      // remove (unlock) ^a(1)
                       // An unlock without a locktype defers the unlock
                       // of a non-incremented lock to the end of the transaction.
      WRITE "Default unlock within a transaction.",!,"Go look at the Lock Table",!
      HANG 10          // This HANG allows you to view the current Lock Table
      LOCK +^a(1)      // reapply lock ^a(1)
      LOCK \-^a(1)#"I"  // remove (unlock) lock ^a(1) immediately
                       // this removes ^a(1) from the lock table immediately
                       // without waiting for the end of the transaction
      WRITE "Immediate unlock within a transaction.",!,"Go look at the Lock Table",!
      HANG 10          // This HANG allows you to view the current Lock Table
                       // while still in the transaction
      TCOMMIT
    
     
    
*   D: Deferred unlock
    
    Controls when an unlocked lock is released during a transaction. The unlock state is deferred to the state of the previous unlock of that lock. Therefore, specifying locktype “D” when unlocking a lock may result in either an immediate unlock or a lock placed in delock state until the end of the transaction, depending on the history of the lock during that transaction. The behavior of a lock that has been locked/unlocked more than once differs from the behavior of a lock that has only been locked once during the current transaction.
    
    The “D” unlock is only meaningful for an unlock that releases a lock (lock count 1), not an unlock that decrements a lock (lock count > 1). An unlock that decrements a lock is always immediately released.
    
    “D” can only be specified for an unlock operation. “D” can be specified for a shared lock (#"SD") or an exclusive lock (#"D"). “D” can be specified for a escalating (“E”) lock, but, of course, the unlock must also be specified as escalating (“ED”). Lock types “D” and “I” are mutually exclusive.
    
    This use of “D” unlock within a transaction is shown in the following examples. The HANG commands give you time to check the lock’s ModeCount in the Lock Table.
    
    If the lock was only applied once during the current transaction, a “D” unlock immediately releases the lock. This is the same as “I” behavior. This is shown in the following example:
    
      TSTART
      LOCK +^a(1)      // Lock Table ModeCount: Exclusive
      LOCK \-^a(1)#"D"  // Lock Table ModeCount: null (immediate unlock)
      HANG 10
      TCOMMIT
    
     
    
    If the lock was applied more than once during the current transaction, a “D” unlock reverts to the prior unlock state.
    
    *   If the last unlock was a standard unlock, the “D” unlock reverts unlock behavior to that prior unlock’s behavior — to defer unlock until the end of the transaction. This is shown in the following examples:
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)      // Lock Table ModeCount: Exclusive
             WRITE "1st unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: Exclusive->Delock
             WRITE "2nd unlock",! HANG 5
          TCOMMIT
        
         
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)      // Lock Table ModeCount: Exclusive->Delock
             WRITE "1st unlock",! HANG 5
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: Exclusive->Delock
             WRITE "2nd unlock",! HANG 5
          TCOMMIT
        
         
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)#"I"  // Lock Table ModeCount: Exclusive/2
             WRITE "1st unlock",! HANG 5
          LOCK \-^a(1)      // Lock Table ModeCount: Exclusive
             WRITE "2nd unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: Exclusive->Delock
             WRITE "3rd unlock",! HANG 5
          TCOMMIT
        
         
        
    *   If the last unlock was an “I” unlock, the “D” unlock reverts unlock behavior to that prior unlock’s behavior — to immediately unlock the lock. This is shown in the following examples:
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)#"I"  // Lock Table ModeCount: null (immediate unlock)
             WRITE "1st unlock",! HANG 5
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: null (immediate unlock)
             WRITE "2nd unlock",! HANG 5
          TCOMMIT
        
         
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)#"I"  // Lock Table ModeCount: Exclusive
             WRITE "1st unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: null (immediate unlock)
             WRITE "2nd unlock",! HANG 5
          TCOMMIT
        
         
        
    *   If the last unlock was a “D” unlock, the “D” unlock follows the behavior of the last prior non-“D” lock:
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: Exclusive
             WRITE "1st unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: null (immediate unlock)
             WRITE "2nd unlock",! HANG 5
          TCOMMIT
        
         
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)      // Lock Table ModeCount: Exclusive/2
             WRITE "1st unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: Exclusive
             WRITE "2nd unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: Exclusive->Delock
             WRITE "3rd unlock",! HANG 5
          TCOMMIT
        
         
        
          TSTART
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK +^a(1)      // Lock Table ModeCount: Exclusive
          LOCK \-^a(1)#"I"  // Lock Table ModeCount: Exclusive/2
             WRITE "1st unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: Exclusive
             WRITE "2nd unlock",! HANG 5
          LOCK \-^a(1)#"D"  // Lock Table ModeCount: null (immediate unlock)
             WRITE "3rd unlock",! HANG 5
          TCOMMIT
        
         
        

timeout

The number of seconds to wait for a lock request to succeed before timing out. timeout is an optional argument. If omitted, the LOCK command waits indefinitely for a resource to be lockable; if the lock cannot be applied, the process will hang. The syntax for timeout is a mandatory colon (:), followed by an integer value or an expression that evaluates to an integer value. A value of zero permits one locking attempt before timing out. A negative number is equivalent to zero.

Commonly, a lock will wait if another process has a conflicting lock that prevents this lock request from acquiring (holding) the specified lock. The lock request waits until either a lock is released that resolves the conflict, or the lock request times out. Terminating the process also ends (deletes) pending lock requests. Lock conflict can result from many situations, not just one process requesting the same lock held by another process. A detailed explanation of lock conflict and lock request wait states is provided in the “[Lock Management](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_locktable)” chapter of Using Caché ObjectScript.

If you use timeout and the lock is successful, Caché sets the $TEST special variable to 1 (TRUE). If the lock cannot be applied within the timeout period, Caché sets $TEST to 0 (FALSE). Issuing a lock request without a timeout has no effect on the current value of $TEST. Note that $TEST can also be set by the user, or by a JOB, OPEN, or READ timeout.

The following example applies a lock on lock name ^abc(1,1), and unlocks all prior locks held by the process:

  LOCK ^abc(1,1)

This command requests an exclusive lock: no other process can simultaneously hold a lock on this resource. If another process already holds a lock on this resource (exclusive or shared), this example must wait for that lock to be released. It can wait indefinitely, hanging the process. To avoid this, specifying a timeout value is strongly recommended:

  LOCK ^abc(1,1):10

If a LOCK specifies multiple lockname arguments in a comma-separated list, each lockname resource may have its own timeout (syntax without parentheses), or all of the specified lockname resources may share a single timeout (syntax with parentheses).

*   Without Parentheses: each lockname argument can have its own timeout. Caché parses this as multiple independent LOCK commands, so the timeout of one lock argument does not affect the other lock arguments. Lock arguments are parsed in strict left-to-right order, with each lock request either completing or timing out before the next lock request is attempted.
    
*   With Parentheses: all lockname arguments share a timeout. The LOCK must successfully apply all locks (or unlocks) within the timeout period. If the timeout period expires before all locks are successful, none of the lock operations specified in the LOCK command are performed, and control returns to the process.
    

Caché performs multiple operations in strict left-to-right order. Therefore, in LOCK syntax without parentheses, the $TEST value indicates the outcome of the last (rightmost) of multiple lockname lock requests.

In the following examples, the current process cannot lock ^a(1) because it is exclusively locked by another process. These examples use a timeout of 0, which means they make one attempt to apply the specified lock.

The first example locks ^x(1) and ^z(1). It sets $TEST=1 because ^z(1) succeeded before timing out:

  LOCK +^x(1):0,+^a(1):0,+^z(1):0

The second example locks ^x(1) and ^z(1). It sets $TEST=0 because ^a(1) timed out. ^z(1) did not specify a timeout and therefore had no effect on $TEST:

  LOCK +^x(1):0,+^a(1):0,+^z(1):0

The third example applies no locks, because a list of locks in parentheses is an atomic (all-or-nothing) operation. It sets $TEST=0 because ^a(1) timed out:

  LOCK +(^x(1),^a(1),^z(1)):0

Using the Lock Table to View and Delete Locks System-wide

Caché maintains a system-wide lock table that records all locks that are in effect and the processes that have applied them. The system manager can display the existing locks in the Lock Table or remove selected locks using the Management Portal interface or the [^LOCKTAB utility](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_locktable#GCOS_locktable_locktab), as described in the “[Lock Management](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_locktable)” chapter of Using Caché ObjectScript. You can also use the [%SYS.LockQuery](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.LockQuery) class to read lock table information. From the %SYS namespace you can use the SYS.Lock class to manage the lock table.

You can use the Management Portal to view held locks and pending lock requests system-wide. Go to the Management Portal, select System Operation, select Locks, then select View Locks (\[Home\] > \[View Locks\]). For further details on the View Locks table refer to the “[Lock Management](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_locktable)” chapter of Using Caché ObjectScript.

You can use the Management Portal to remove (delete) locks currently held on the system. Go to the Management Portal, select System Operation, select Locks, then select Manage Locks (\[Home\] > \[Manage Locks\]). For the desired process (Owner) click either “Remove” or “Remove All Locks for Process”.

Removing a lock releases all forms of that lock: all increment levels of the lock, all exclusive, exclusive escalating, and shared versions of the lock. Removing a lock immediately causes the next lock waiting in that lock queue to be applied.

You can also remove locks using the [SYS.Lock.DeleteOneLock()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=SYS.Lock#DeleteOneLock) and [SYS.Lock.DeleteAllLocks()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=SYS.Lock#DeleteAllLocks) methods.

Removing a lock requires WRITE permission. Lock removal is logged in the audit database (if enabled); it is not logged in cconsole.log.

Incremental Locking and Unlocking

Incremental locking permits you to apply the same lock multiple times: to increment the lock. An incremented lock has a lock count of > 1. Your process can subsequently increment and decrement this lock count. The lock is released when the lock count decrements to 0. No other process can acquire the lock until the lock count decrements to 0. The lock table maintains separate lock counts for exclusive locks and shared locks, and for escalating and non-escalating locks of each type. The maximum incremental lock count is 32,766. Attempting to exceed this maximum lock count results in a <MAX LOCKS> error.

You can increment a lock as follows:

*   Plus sign: Specify multiple lock operations on the same lock name with the plus sign lock operation indicator. For example: LOCK +^a(1) LOCK +^a(1) LOCK +^a(1) or LOCK +^a(1),+^a(1),+^a(1) or LOCK +(^a(1),^a(1),^a(1)). All of these would result in a lock table ModeCount of Exclusive/3. Using the plus sign is the recommended way to increment a lock.
    
*   No sign: It is possible to increment a lock without using the plus sign lock operation indicator by specifying an atomic operation performing multiple locks. For example, LOCK (^a(1),^a(1),^a(1)) unlocks all prior locks and incrementally locks ^a(1) three times. This too would result in a lock table ModeCount of Exclusive/3. While this syntax works, it is not recommended.
    

Unlocking an incremented lock when not in a transaction simply decrements the lock count. Unlocking an incremented lock while in a transaction has the following default behavior:

*   Decrementing Unlocks: each decrementing unlock immediately release the incremental unlock until the lock count is 1. By default, the final unlock puts the lock in delock state, deferring release of the lock to the end of the transaction. This is always the case when you delock with the minus sign lock operation indicator, whether or not the operation is atomic. For example: LOCK -^a(1) LOCK -^a(1) LOCK -^a(1) or LOCK -^a(1),-^a(1),-^a(1) or LOCK -(^a(1),^a(1),^a(1)). All of these begin with a lock table ModeCount of Exclusive/3 and end with Exclusive->Delock.
    
*   Unlocking Prior Resources: an operation that unlocks all prior resources immediately puts an incremented lock into a delock state until the end of the transaction. For example, either LOCK x(3) (lock with no lock operation indicator) or an argumentless LOCK would have the following effect: the incremented lock would begin with a lock table ModeCount of Exclusive/3 and end with Exclusive/3->Delock.
    

Note that separate lock counts are maintained for the same lock as an Exclusive lock, a Shared lock, a Exclusive escalating lock and a Shared escalating lock. In the following example, the first unlock decrements four separate lock counts for lock ^a(1) by 1. The second unlock must specify all four of the ^a(1) locks to remove them. The HANG commands give you time to check the lock’s ModeCount in the Lock Table.

   LOCK +(^a(1),^a(1)#"E",^a(1)#"S",^a(1)#"SE")
   LOCK +(^a(1),^a(1)#"E",^a(1)#"S",^a(1)#"SE")
   HANG 10
   LOCK \-(^a(1),^a(1)#"E",^a(1)#"S",^a(1)#"SE")
   HANG 10
   LOCK \-(^a(1),^a(1)#"E",^a(1)#"S",^a(1)#"SE")

 

If you attempt to unlock a lock name that has no current locks applied, no operation is performed and no error is returned.

Automatic Unlock

When a process terminates, Caché performs an implicit argumentless LOCK to clear all locks that were applied by the process. It removes both held locks and lock wait requests.

Locks on Global Variables

Locking is typically used with global variables to synchronize the activities of multiple processes that may access these variables simultaneously. Global variables differ from local variables in that they reside on disk and are available to all processes. The potential exists, then, for two processes to write to the same global at the same time. In fact, Caché processes one update before the other, so that one update overwrites and, in effect, discards the other.

Global lock names begin with a caret (^) character.

To illustrate locking with global variables, consider the case in which two data entry clerks are concurrently running the same student admissions application to add records for newly enrolled students. The records are stored in a global array named ^student. To ensure a unique record for each student, the application increments the global variable ^index for each student added. The application includes the LOCK command to ensure that each student record is added at a unique location in the array, and that one student record does not overwrite another.

The relevant code in the application is shown below. In this case, the LOCK controls not the global array ^student but the global variable ^index. ^index is a scratch global that is shared by the two processes. Before a process can write a record to the array, it must lock ^index and update its current value (SET ^index=^index+1). If the other process is already in this section of the code, ^index will be locked and the process will have to wait until the other process releases the lock (with the argumentless LOCK command).

 READ !,"Last name: ",!,lname QUIT:lname\=""  SET lname\=lname\_"," 
 READ !,"First name: ",!,fname QUIT:fname\=""  SET fname\=fname\_"," 
 READ !,"Middle initial: ",!,minit QUIT:minit\=""  SET minit\=minit\_":" 
 READ !,"Student ID Number: ",!,sid QUIT:sid\=""
 SET rec \= lname\_fname\_minit\_sid
 LOCK ^index
 SET ^index \= ^index + 1
 SET ^student(^index)\=rec
 LOCK

The following example recasts the previous example to use locking on the node to be added to the ^student array. Only the affected portion of the code is shown. In this case, the ^index variable is updated after the new student record is added. The next process to add a record will use the updated index value to write to the correct array node.

 LOCK ^student(^index)
 SET ^student(^index) \= rec
 SET ^index \= ^index + 1
 LOCK  /\* release all locks \*/

Note that the lock location of an array node is where the top level global is mapped. Caché ignores subscripts when determining lock location. Therefore, ^student(name) is mapped to the namespace of ^student, regardless of where the data for ^student(name) is stored.

Locks in a Network

In a networked system, one or more servers may be responsible for resolving locks on global variables.

You can use the LOCK command with any number of servers, up to 255. Remote locks held by a client job on a remote server system are released when you call the [^RESJOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt#RCOS_chalt_resjob) utility to remove the client job.

Local Variable Locks

The behavior of LOCK on local (non-careted) variables is the same on Caché, Open M \[DTM\], and DSM for OpenVMS. The Config.Miscellaneous class provides a [ZaMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#ZaMode) property that you can use to set the locking mode system-wide for compatibility with the older [DSM–11](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_mcompat#GCOS_mc_dsm11) ZALLOCATE (ZA) and ZDEALLOCATE (ZD) locking commands.

In Open M \[DTM\] and DSM for OpenVMS, local locks are always taken out in the manager's dataset regardless of the first character. If an application ported to Caché uses local locks that do not begin with a % character, the application may experience deadlock conditions if it uses the same, local, non-% lock names in different datasets. The locks now collide because they resolve to the same item. If this is part of a hierarchical locking system, a deadly embrace may occur that can hang an application.

The current behavior is:

*   Local (non-careted) locks acquired in the context of a specific namespace, either because the default namespace is an explicit namespace or through an explicit reference to a namespace, are taken out in the manager's dataset on the local machine. This occurs regardless of whether the default mapping for globals is a local or a remote dataset.
    
*   Local (non-careted) locks acquired in the context of an implied namespace or through an explicit reference to an implied namespace on the local machine, are taken out using the manager's dataset of the local machine. An implied namespace is a directory path or OpenVMS file specification preceded by two caret characters: "^^dir".
    

Referencing explicit and implied namespaces is further described in [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

See Also

*   [$TEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest) special variable
    
*   [^$LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_slock) structured system variable
    
*   [Lock Management](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_locktable) in Using Caché ObjectScript
    
*   [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript
    
*   The [Monitoring Locks](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCM_dashboard#GCM_dashboard_locks) section of the “Monitoring Caché Using the Management Portal” chapter in Caché Monitoring Guide
    
*   The article [Locking and Concurrency Control](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ALOCK)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmerge "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_clock.xml**