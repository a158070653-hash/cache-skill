# $ZF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzf

---

 

Caché ObjectScript Reference

$ZF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdchar "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1 "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf "$ZF") \]

Go to: Description Parameters Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Invokes non-Caché ObjectScript programs or functions from Caché ObjectScript routines.

Synopsis

$ZF("function\_name",args)

Parameters

function\_name

The name of the function you want to call.

args

Optional — A set of argument values passed to the function.

Description

The various forms of the $ZF function allow you to invoke non-Caché ObjectScript programs (such as shell or operating system commands) or functions from Caché ObjectScript routines. You can define interfaces or links to functions written in other languages into Caché and call them from Caché ObjectScript routines using $ZF.

$ZF can also be used to:

*   Spawn a child process to execute a program or command: [$ZF(–1)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1) and [$ZF(–2)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2).
    
*   Load a Dynamic Link Library (DLL) then execute functions from that library: [$ZF(–3)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3), [$ZF(–4)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-4), [$ZF(–5)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5), and [$ZF(–6)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-6).
    

These implementations of $ZF take a [negative number as the first parameter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf#RCOS_fzf78). They are described in their own reference pages.

Parameters

function\_name

The name of the function you want to call enclosed in quotation marks, or a [negative number](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf#RCOS_fzf78).

args

The args parameters are in the form: arg1, arg2, arg3, ...argn. The arguments can consist of such items as descriptions of how arguments are passed and the entry point to the C function you are calling.

Notes

Calling UNIX® System Services with $ZF

Caché supports error checking functions for use with UNIX® system calls from $ZF. These calls allow you to check for asynchronous events and to set an alarm handler in $ZF. By using these UNIX® functions you can distinguish between real errors, <CTRL-C> interrupts, and calls that should be restarted.

The function declarations are included in cdzf.h and are described in the following table:

Declaration

Purpose

Notes

int sigrtclr();

Clears retry flag.

Should be called once before using sigrtchk()

int dzfalarm();

Establishes new SIGALRM handler.

On entry to $ZF, the previous handler is automatically saved. On exit, it is restored automatically. A user program should not alter the handling of any other signal.

int sigrtchk();

Checks for asynchronous events.

Should be called whenever one of the following system calls fails: open(), close(), read(), write(), ioctl(), pause(), any call that fails when the process receives a signal. It returns a code indicating the action the user should take:

\-1 = Not a signal. Check for I/O error. See contents of errno variable.

0 = Other signal. Restart operation from point at which it was interrupted.

1 = SIGINT/. Exit from $ZF with a SIGTERM "return 0." The System traps these signals appropriately.

In a typical $ZF function used to control some device, you would code something like this:

  IF ((fd \= open(DEV\_NAME, DEV\_MODE)) < 0) {
     ; Set some flags
     ; Call zferror
     ; return 0;
  }

The open system call can fail if the process receives a signal. Usually this situation is not an error and the operation should be restarted. Depending on the signal, however, you might take other actions. So, to take account of all the possibilities, consider using the following C code:

sigrtclr();
   WHILE (TRUE) {
      IF (sigrtchk() == 1) { return 1 or 0; }
      IF ((fd = open(DEV\_NAME, DEV\_MODE)) < 0) {
         switch (sigrtchk()) {
         case -1:
           /\* This is probably a real device error \*/
           ; Set some flags
           Call zferror
           return 0;
         case 0:
           /\* A innocuous signal was received. Restart. \*/
           ; continue;
         case 1:
           /\* Someone is trying to terminate your job. \*/
           Do cleanup work
           return 1 or 0;
         }
      }
      ELSE { break; }
   /\* Code to handle the normal situation: \*/
   /\* open() system call succeeded         \*/

Remember you must not set any signal handler except through dzfalarm().

Calling OpenVMS System Services with $ZF

You can also call OpenVMS system services with $ZF. You define an interface to each non-Caché ObjectScript routine in a file named CZF.M64. You can also use $ZF to call OpenVMS System Service Routines, invoke DCL Command Procedures, or other high-level language routines.

$ZF adheres to the OpenVMS Procedure Calling and Condition Handling Standard. While all OpenVMS compilers are supposed to adhere to this convention, some compilers may not.

You can use $ZF to call DSM intrinsic functions which emulate DSM $ZCalls for OpenVMS System Services. For example, you can call $ZF(“GETSYI”) to get system information for a DSM cluster node member.

The available DSM intrinsic functions are: GETJPI (job and process information); GETDVI (device characteristics); GETSYI (system information); SETSYM (sets DCL symbol value); GETSYM (returns DCL symbol value); DELSYM (deletes DCL symbol value); CRELOG (creates logical name); TRNLNM (translates logical name); DELLOG (deletes logical name); GETUAI (returns account authorization parameters); GETMSG (returns status code message text); SETPRN (sets name of the calling process); SETPRI (sets base priority for a process); OPCOM (sends message to the operator); MOUNT (mounts a device); DISMOUNT (dismounts a device); DIRECTORY (returns the default directory); PARSE (parses a filename); GETFILE (returns file information). For specific function calls for DSM compatibility and conversion, see “[$ZF Function Calls for DSM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_vms#BGCL_vms_dsm)” in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface).

Translating Strings between Encoding Systems

Caché supports input-output translation via a $ZF argument type, t (or T), which can be specified in the following formats:

Argument

Purpose

t

Specifies the current process I/O translation object.

t//

Specifies the default process I/O translation table name.

t/name/

Specifies a particular I/O translation table name.

$ZF conveys the translated string to the external procedure via a counted-byte string placed in the following C structure:

typedef struct zarray {
  unsigned short len;
  unsigned char data\[1\]; /\* 1 is a dummy value \*/
  } \*ZARRAYP;

This is also the structure used for the b (or B) argument type.

The following $ZF sample function performs a round trip conversion:

#include cdzf.h
extern    int trantest();
ZFBEGIN
ZFENTRY("TRANTEST","t/SJIS/ T/SJIS/",trantest)

ZFEND

int trantest(inbuf,outbuf);

ZARRAYP inbuf;         /\* Buffer containing string that was converted from 
        internal Caché encoding to SJIS encoding before it
        was passed to this function \*/
ZARRAYP outbuf;        /\* Buffer containing string in SJIS encoding that will
        be converted back to internal Caché encoding before
        it is passed back into the Caché environment \*/
{
  int  i;
  /\* Copy data one byte at a time from the input argument buffer 
     to the output argument buffer \*/

  for (i = 0; i < inbuf->len; i++)
     outbuf->data\[i\] = inbuf->data\[i\];

  /\* Set number of bytes of data in the output argument buffer \*/
       outbuf->len = inbuf->len;

  return 0;  /\* Return success \*/
}

Note:

Conceptually speaking, data flows to and from a $ZF external procedure, as if the external procedure were a device. The output component of an I/O translation is used for data that is passed to an external procedure because the data is “leaving” the Caché environment. The input component of an I/O translation is used for data that is received from an external procedure because the data is “entering” the Caché environment.

If the output component of an I/O translation is undefined and your application attempts to pass anything but the null string using that I/O translation, Caché returns an error, because it does not know how to translate the data.

If the input component of an I/O translation is undefined and an argument of type string associates that I/O translation with a $ZF output argument, Caché returns an error, because an output argument with an undefined translation is purposeless.

Zero-Terminated and Counted Unicode Strings

The $ZF function supports argument types for zero-terminated Unicode strings and counted Unicode strings. These are supported even in versions of Caché that do not use Unicode characters internally.

The argument types for zero-terminated Unicode strings and counted Unicode strings have the following codes:

Argument

Purpose

w

Pointer to a zero-terminated Unicode character string.

s

Pointer to a counted Unicode character string.

For both argument types, the C data type of the Unicode character is an unsigned short. A pointer to a zero-terminated Unicode string is declared as follows:

unsigned short \*p;

A pointer to a counted Unicode string is declared as a pointer to the following C structure:

typedef struct zwarray {
  unsigned short len;
  unsigned short data\[1\]; /\* 1 is a dummy value \*/
  } \*ZWARRAYP;

For example:

ZWARRAYP \*p;

The len field contains the length of the Unicode character array.

The data field contains the characters in the counted Unicode string. The maximum size of a Unicode string is the maximum $ZF string size, which is an updateable configuration parameter that defaults to 32767.

Each Unicode character is two bytes long. This is important to consider when declaring Unicode strings as output arguments, because Caché reserves space for the longest string that may be passed back. When using the default string size, the total memory consumption for a single Unicode string argument is calculated as follows:

32767 maximum characters \* 2 bytes per character = 65534 total bytes.

This is close to the default maximum memory area allocated for all $ZF arguments, which is 67584. This maximum $ZF heap area is also an updateable configuration parameter.

Error Messages

When the $ZF heap area is exhausted, $ZF issues an <OUT OF $ZF HEAP SPACE> error. When the $ZF String Stack is exhausted, $ZF issues a <STRINGSTACK> error. When $ZF is unable to allocate a buddyblock, it issues a <STORE> error.

Execution from Child Processes and DLLs

The $ZF function can take a negative number as its first parameter. These negative numbers specify functions that support spawned child processes and Dynamic-Link Libraries (DLLs). Each of these $ZF functions is described in a separate reference page.

See Also

*   [$ZF(-1)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1) function
    
*   [$ZF(-2)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2) function
    
*   [$ZF(-3)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-3) function
    
*   [$ZF(-4)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-4) function
    
*   [$ZF(-5)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-5) function
    
*   [$ZF(-6)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-6) function
    
*   [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    
*   [$ZF Function Calls for DSM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_vms#BGCL_vms_dsm) in [Using the Caché Callout Gateway](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=BGCL_preface)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdchar "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1 "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzf.xml**