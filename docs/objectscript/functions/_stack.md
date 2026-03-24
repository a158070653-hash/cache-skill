# $STACK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fstack

---

 

Caché ObjectScript Reference

$STACK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortend "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftext "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack "$STACK") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns information about active contexts saved on the process call stack.

Synopsis

$STACK(context\_level,code\_string)
$ST(context\_level,code\_string)

Parameters

context\_level

An integer specifying the zero-based context level number of the context for which information is requested. Supported values include 0, positive integers, and -1.

code\_string

Optional — A keyword string that specifies the kind of context information that is requested. supported values are “PLACE”, “MCODE”, and “ECODE”

Description

The $STACK function returns information on either the current execution stack or the current error stack, depending on the value of the $ECODE special variable. $STACK is most commonly used to return information on the current execution stack (also known as the process call stack).

Each time a routine invokes a DO command, an XECUTE command, or a user-defined function (but not a GOTO command), the context of the currently executing routine is saved on the call stack and execution starts in the newly created context of the called routine. The called routine, in turn, can call another routine, and so on, causing more saved contexts to be placed on the call stack.

The $STACK function returns information about these active contexts saved on your process call stack. $STACK also can return information about the currently executing context. However, during error processing, $STACK returns a snapshot of all the context information that is available when an error occurs in your application.

You can use the [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable to determine the current context level.

$ECODE and $STACK

The values returned by $STACK are dependent on the [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode) special variable. If $ECODE is clear (set to the null string), $STACK returns the current execution stack. If $ECODE contains a non-null value, $STACK returns the current error stack.

Error stack context information is only available when the $ECODE special variable contains a non-null value. This can occur either when an error has occurred or when $ECODE is explicitly set to a non-null value. In this case, $STACK returns information about the error stack context rather than an active stack context at the specified context level.

When error stack context information is not available ($ECODE="") and you specify the current context level with the two-argument form of $STACK, Caché returns information about the currently executing command. To ensure consistent behavior when accessing the current execution stack, specify SET $ECODE="" before calling $STACK.

See [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript for more detailed information about error processing and your error process stack.

The One-Argument Form of $STACK

$STACK(context\_level) returns a string that indicates how the specified context level was established. The following table describes the string values that can be returned:

DO

Returned when the specified context was established by a DO command.

XECUTE

Returned when the specified context was established by an XECUTE command or a BREAK command.

$$

Returned when the specified context was established by a user-defined function reference.

An ECODE string

The error code value of the error that caused the specified context to be added to the error stack. For example, ,M26,. When an error occurs at a context level where an error has already occurred, the context information is placed at the next higher error stack level; it is only returned when context information at the specified error stack context level is relocated information.

When the specified context level is zero (0) or is undefined, $STACK returns the null string.

You can also specify a -1 for the context level in the one-argument form of the $STACK function. In this case, $STACK returns the maximum context level for the information that is available that, during normal processing, is the context level number of the currently executing context. However, during error processing, $STACK(-1) returns whichever is greater:

*   The maximum context level of your process error stack
    
*   The context level number of the currently executing context
    

The Two-Argument Form of $STACK

$STACK(context\_level,code\_string) returns information about the specified context level according to the code\_string you specify. A code\_string must be specified as a quoted string; code\_string values are not case-sensitive. For example, $STACK(1,"PLACE") or $STACK(1,"place").

The following describes the code strings and the information returned when you specify each.

*   PLACE — Returns the entry reference and command number of the last command executed at a specified context level. The value is returned in the following format for DO and user-defined function contexts: "label\[+offset\]\[^routine name\] +command". For XECUTE contexts, the following format is used: "@ +command".
    
*   MCODE — Returns the source routine line, XECUTE string, or $ETRAP string that contains the last command executed at the specified context level. (Routine lines are returned in the same manner as those returned by the $TEXT function.)
    
    Note:
    
    During error processing, if memory is low while the error stack is being built or updated, you may not have enough memory to store the source line. In this case, the return value for the MCODE code string is the null string. However, the return value for the PLACE code string indicates the location.
    
*   ECODE — The error code of any error that occurred at the specified context level (available only in error stack contexts).
    

When the requested information is not available at the specified context level, the two argument form of $STACK returns the null string.

Example

The following example demonstrates some of the information that $STACK can return:

STAC  ;
      SET $ECODE\=""
      XECUTE "DO First"
      QUIT
First SET varSecond\=$$Second()
      QUIT
Second()  FOR loop\=0:1:$STACK(\-1) { 
          WRITE !,"Context level:",loop,?25,"Context type: ",$STACK(loop)
          WRITE !,?5,"Current place: ",$STACK(loop,"PLACE")
          WRITE !,?5,"Current source: ",$STACK(loop,"MCODE")
          WRITE ! }
      QUIT 1

\>DO  ^STAC
Context level: 0      Context type:
  Current place: @ +1
  Current source: DO ^STAC
Context level: 1      Context type: DO
  Current place: STAC+2^STAC +1
  Current source: XECUTE "DO First"
Context level: 2      Context type: XECUTE
  Current place: @ +1
  Current source: DO First
Context level: 3      Context type: DO
  Current place: First^STAC +1
  Current source: First SET Second=$$Second
Context level: 4      Context type: $$
  Current place: Second+2^STAC +4
  Current source: WRITE !,?5,"Current source: ",$STACK(loop,"MCODE")

Notes

Cross-Namespace Routine Calls

If a routine calls a routine in a different namespace, $STACK returns the namespace name as part of the routine name. For example, if a routine in the USER namespace calls a routine in the SAMPLES namespace, $STACK returns ^|"SAMPLES"|routinename.

$STACK uses the caret (^) character as a delimiter. Therefore, if an implied namespace name includes the caret (^) character, Caché displays this namespace name character as the @ character.

$STACK Counts Multiple-Argument Commands

When you specify a multiple-argument command, the command count includes command keywords and all command arguments beyond the first. Consider the following multiple-argument command:

TEST
  SET X\=1,Y\=Z

In Caché, the $STACK statement, $STACK(1,"PLACE") returns "TEST^TEST +2" because the Y=Z argument counts as a separate command.

$STACK with <STORE> Errors or Low Memory Conditions

After a <STORE> error or under low-memory conditions, the information available normally through the application of the two-argument form of $STACK may not be available.

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute) command
    
*   [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode) special variable
    
*   [$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack) special variable
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsortend "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftext "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fstack.xml**