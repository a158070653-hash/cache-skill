# $COMPILE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fcompile

---

 

Caché ObjectScript Reference

$COMPILE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$COMPILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcompile "$COMPILE") \]

Go to: Description Parameters Interrupting a Compile Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Compiles source code, producing executable object code.

Synopsis

$COMPILE(source,language,errors,object)

$COMPILE(source,language,errors,,,,rname)

Parameters

source

An array variable containing the source code to be compiled.

language

An integer flag specifying the programming language of the source code.

errors

A local variable that receives any errors that occur during compilation. This variable is a List structure, with one element for each error reported. Each error is itself a List structure, specifying error location and type (see below).

object

Optional — An array used to hold the compiled object code.

rname

Optional — (Second syntactic form only) a string specifying a routine name used to store the compiled object code in the ^rOBJ global.

Description

$COMPILE compiles source code and produces object code (the executable form of the routine). $COMPILE reports compilation errors, and can be used to check source code for compilation errors without actually producing object code. Before compiling, any macros in the source code must be processed by a preprocessor such as the Caché ObjectScript macro preprocessor.

Note:

Caché provides several powerful tools for compiling source code, [Caché Studio](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTD) being one of them. Commonly, source code compilation is performed using these tools, rather than this $COMPILE function.

$COMPILE has two syntactic forms:

*   The first $COMPILE syntax form returns the object code in the object array. It first kills the object variable. After the compilation the object array is set to the size of the compiled object code.
    
    The object array contains the object code in the same format as it would be in the ^rOBJ global. The object code in ^rOBJ can be replaced with the new object code by the command  MERGE ^rOBJ(rname)=object(1). However, the [MERGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmerge) command is not atomic when setting multiple nodes, so this operation could cause unpredictable results if another process is concurrently loading the same routine.
    
    If you omit the object parameter, the source code is compiled and checked for errors, but no object code is created.
    
*   The second $COMPILE syntax form returns the object code directly into ^rOBJ(rname). The $COMPILE operation internally locks ^rOBJ(rname), preventing any other process from loading the routine object code until the new object code is completely stored.
    
    Commonly, the object argument is omitted with this syntactic form. If you specify the object parameter, the object variable is killed, but is not set. The other omitted arguments (represented by placeholder commas) are for internal use and should not be specified.
    

$COMPILE returns a status code as follows: 0 = no errors were detected and object code was created. 1 = errors were detected and object code was created. -1 = errors were detected and no object code was created.

When the Caché ObjectScript compiler detects an error, it creates object code at that point which throws an error when that line is executed. When the Basic compilers detect an error they do not produce any object code.

To compile a class, use the [Compile()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.OBJ#Compile) method of the [%SYSTEM.OBJ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.OBJ) class.

Parameters

source

An array containing the source code to be compiled. The source can be in the format of an INT routine for Caché ObjectScript, a BAS routine for Caché Basic, or an MVI routine for Caché MVBasic. The array element source(0) must contain the number of lines of source code, and each source(n) contains line number n of the source code. The source lines must be numbered consecutively from 1 through n with no omitted lines. The source parameter can be an unsubscripted local variable name, or a possibly subscripted global name.

If source(0) is undefined, Caché generates an <UNDEFINED> error, regardless of the [%SYSTEM.Process.Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method setting.

If a source(0) value is larger than the number of lines of source code, or a consecutive source code line is missing, Caché generates an <UNDEFINED> error, followed by the name of the missing source code line. This behavior can be changed by setting the [%SYSTEM.Process.Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method. These types of errors are shown in the following examples:

  SET src(0)\=4,src(1)\="TestA ",src(2)\=" WRITE 123",src(3)\=" WRITE 456,!"
  SET stat\=$COMPILE(src,0,errs,TestA)    /\* generates <UNDEFINED> \*src(4) \*/

  SET src(0)\=4,src(1)\="TestA ",src(3)\=" WRITE 123",src(4)\=" WRITE 456,!"
  SET stat\=$COMPILE(src,0,errs,TestA)    /\* generates <UNDEFINED> \*src(2) \*/

  SET src(0)\=3,src(1)\="TestA ",src(3)\=" WRITE 123",src(4)\=" WRITE 456,!"
  SET stat\=$COMPILE(src,0,errs,TestA)    /\* generates <UNDEFINED> \*src(2) \*/

language

The language mode specifying the type of source to be compiled. Common values are:

*   0 = Caché ObjectScript
    
*   9 = Caché Basic
    
*   11 = Caché MVBasic
    

Other values specify legacy Caché ObjectScript modes and should be used only after consultation with InterSystems support.

errors

An unsubscripted local variable that is set to any errors detected by the compiler. Any existing value is killed. If no errors are detected, the variable is set to the empty string (""). If errors are detected, the errors variable is set to a [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) structure with one element for each error. Each error is itself a $LIST structure with the format $LISTBUILD(line,offset,errnum,text) where:

*   line = the line number where the error was detected
    
*   offset = the offset in the source line of the error
    
*   errnum = an error number for the type of error
    
*   text = text describing the error
    

The Basic compilers add two more elements to the list, with text describing the location of the error, and the source line itself.

object

An array that receives the object code output of the compiler. The object parameter can be an unsubscripted local variable name, or a possibly subscripted global name. The contents of the object array are described above.

rname

The routine name that specifies where the object code should saved in the ^rOBJ subscripted global. $COMPILE kills any existing contents of ^rOBJ(rname) before saving the new object code.

Interrupting a Compile

You can issue a Ctrl-C or invoke the [^RESJOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt#RCOS_chalt_resjob) utility to interrupt a compile in progress. These compile interrupts are supported for all language modes.

Examples

The following example compiles a four-line Caché ObjectScript program using the first $COMPILE format:

SourceCode
  SET src(0)\=4
  SET src(1)\="TestA "
  SET src(2)\=" WRITE ""Hello "" "
  SET src(3)\=" WRITE ""World"",!"
  SET src(4)\=" QUIT"
CompileSource
  SET stat\=$COMPILE(src,0,errs,TestA)
  IF stat\=0 {WRITE "Compile successful" }
  ELSE {WRITE "status=",stat,!
        WRITE "number of compile errors=",$LISTLENGTH(errs) }

 

The following example compiles the same four-line Caché ObjectScript program using the second $COMPILE format:

SourceCode
  SET src(0)\=4
  SET src(1)\="TestB "
  SET src(2)\=" WRITE ""Hello "" "
  SET src(3)\=" WRITE ""World"",!"
  SET src(4)\=" QUIT"
CompileSource
  SET stat\=$COMPILE(src,0,errs,,,,"TestB")
  IF stat\=0 {WRITE "Compile successful",!
             DO ^TestB }
  ELSE {WRITE "status=",stat,!
        WRITE "number of compile errors=",$LISTLENGTH(errs) }

 

The following example performs compilation error checking on a seven-line Caché ObjectScript program. Note that this $COMPILE only tests for errors; it does not provide a variable to receive the object code from a successful compile. In this example every line of source code contains an error; $COMPILE only returns the compile-time errors in lines 1, 3, 5, 6, and 7, not runtime errors such as a divide-by-zero error (line 2) or an undefined variable error (line 4):

SourceCode
  SET src(0)\=7
  SET src(1)\="?TestC "
  SET src(2)\=" SET a=2/0"
  SET src(3)\=" SET b=3+#2"
  SET src(4)\=" SET c=xxx"
  SET src(5)\=" SET? d=5"
  SET src(6)\=" SET 123=""abc"""
  SET src(7)\=" SETT f=7"
CompileSource
  SET stat\=$COMPILE(src,0,errs)
  IF stat {WRITE $LISTLENGTH(errs)," Compile Errors ",!
    FOR i\=1:1:$LISTLENGTH(errs) {
      WRITE !,i,": " 
      SET errn\=$LIST(errs,i)
      FOR j\=1:1:$LISTLENGTH(errn) {
        WRITE $LIST(errn,j)," "
        }
    }
  }
  ELSE {WRITE "Compile successful",!
        WRITE "but no object code generated" }

 

See Also

*   “[ObjectScript Macros and the Macro Preprocessor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros)” in Using Caché ObjectScript
    
*   [Using Caché Studio](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTD)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fcompile.xml**