# $SYSTEM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vsystem

---

 

Caché ObjectScript Reference

$SYSTEM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$SYSTEM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem "$SYSTEM") \]

Go to: Description Flags and Qualifiers Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains system information about system objects.

Synopsis

$SYSTEM
$SY

$SYSTEM.class.method()

Description

$SYSTEM can be invoked as either a special variable or as a class which invokes methods that return system information.

$SYSTEM Special Variable

$SYSTEM as a special variable contains the local system name and the name of the current instance of Caché, separated by a colon (:). The name of the machine follows the case conventions of the local operating system and the name of the instance is in uppercase. If Caché is part of the name of the instance, there is no accent on the final letter. For example:

MyComputer:CACHE2

You can also determine your local system name using the [LocalHostName()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.INetInfo#LocalHostName) method:

  WRITE $SYSTEM,!
  WRITE $SYSTEM.INetInfo.LocalHostName() 

 

The abbreviation $SY can only be used for $SYSTEM as a special variable.

$SYSTEM Class

$SYSTEM as a class provides access to a variety of system objects. You can invoke a method that returns information, or a method that performs some operation such as upgrading or loading and returns status information. Caché supports several classes of system objects, including the following:

*   Version: for version numbers of Caché and its components
    
*   SYS: for the system itself
    
*   OBJ: for Objects
    
*   SQL: for SQL queries
    
*   CSP: for Caché Server Pages
    

Note that object class names and method names are case-sensitive. Specifying the wrong case for these names results in a <CLASS DOES NOT EXIST> or <METHOD DOES NOT EXIST> error. If you do not specify parentheses with the method name, it issues a <SYNTAX> error.

For further information on using dot syntax with $SYSTEM to access these objects, refer to the chapter “[Working with Registered Objects](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_objapi)” of Using Caché Objects. For further information on using [%SYSTEM.OBJ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.OBJ), refer to [Flags and Qualifiers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem#RCOS_vsystem_flags_qualifiers). $SYSTEM can access the System API classes in the %SYSTEM class library, described in the [Caché Class Library](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_intro) section of Using Caché Objects.

Flags and Qualifiers

These are arguments which can be used to control the import of external sources into Caché, compile existing applications, and export these to external destinations. In the class documentation for [%SYSTEM.OBJ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.OBJ), these are often supplied as the value of the parameter, qspec. The available settings for each can be displayed by the commands:

  DO $SYSTEM.OBJ.ShowFlags()

 

and

  DO $SYSTEM.OBJ.ShowQualifiers()

 

Flags

Of the two, the flags are the earlier. They were modeled on UNIX® command-line parameters and thus are one- or two-character sequences. The existing flags and their meanings (excluding deprecated flags) are:

Existing Flags

Flag

Meaning

Default

b

Includes subclasses and classes that reference the current class in SQL usage.

 

c

Compiles the class definitions after loading.

 

d

Display.

X

e

Deletes the extent definition that describes the global storage used by the extent, and deletes the data.

 

h

Generates help.

 

i

Validates XML export format against schema on Load.

 

k

Keep source. When this flag is set, source code of generated routines will be kept.

 

l

Locks classes while compiling.

X

p

Includes classes whose names begin with the “%” character.

 

r

Recursive. Compiles all the classes that are dependency predecessors.

 

s

System. Processes system messages or application messages.

 

u

Update only. Skips compilation of classes that are already up-to-date.

 

y

Includes classes that are related to the current class; classes that either reference the current class in SQL usage, or are referenced by the current class in SQL usage.

 

o1, o2, o3, o4

Optimization specifiers. Deprecated and ignored by the class compiler.

 

Note:

Flags may be turned off by preceding them with a dash (-).

Qualifiers

During the development of Caché version 5.1, it became clear that flags would be insufficient to provide for the emerging needs of import, export and compilation. Rather than replace the flags mechanism, a newer, more extensible set of controls was implemented: qualifiers. To preserve backward compatibility, the flag mechanism remains fully supported. In addition, a qualifier exists whose meaning is the same as each existing flag, and the two may be used in the same specifier.

Since there are so many more qualifiers, they are organized into groups according to the function they control as shown in the following tables. Deprecated qualifiers are not shown.

Compiler Qualifiers

Flag

Meaning

Default

/autoinclude

Automatically includes any classes that are not up to date that are required to compile this class.

1

/checkschema

Validates imported XML files against the schema definition.

1

/checksysutd

Checks system classes for up-to-dateness.

0

/checkuptodate

Skips classes or expanded classes that are up-to-date.

expandedonly

/compile

Causes classes loaded to be compiled as well.

0

/cspcompileclass

Causes classes created by CSP or CSR load to be compiled.

1

/cspdeployclass

When CSP page loaded deploys the class generated.

0

/csphidden

Classes generated from CSP and CSR compilation are marked as hidden.

1

/defines

Comma separated list of macros to define and, optionally, their values.

—

/deleteextent

Deletes the extent definition that describes the global storage used by the extent, and deletes the data.

0

/diffexport

Does not include any time or platform information in export so the files can be run through diff/merge tools.

0

/display

Alias qualifier for /displaylog and displayerror.

—

/displayerror

Displays error information.

1

/displaylog

Displays log information.

1

/expand

Alias qualifier for /predecessorclasses, /subclasses and /relatedclasses.

—

/exportgenerated

When exporting classes also exports generated classes where the class generating them is also included.

0

/exportselectivity

Exports the selectivity values stored in the storage definition for this class.

1

/filterin

Alias qualifier for /application, /system and /percent.

—

/force

Forces an action.

0

/generated

Determines when expanding patterns or lists of classes in a package whether to include generated items (routines, classes, etc.).

1

/generatemap

Generates the map file.

1

/importselectivity

0: Do not import selectivity values from the XML file. 1: Import the selectivity values stored in the storage definition when importing XML file. 2: Keep the existing class selectivity values, but if the existing class does not have selectivity specified for something that is present in the XML file then use the selectivity value from the XML file.

2

/includesubpackages

Includes sub-packages.

1

/incremental

Allows incremental compilation.

0

/journal

Journaling enabled while performing a class compile.

1

/keepsource

Keeps the source code of generated routines.

0

/lock

Uses LOCK command while compiling classes.

1

/mapped

Includes classes mapped from another database.

0

/mergeglobal

If importing a global from XML file merges the global with existing data.

0

/multicompile

Enables multiple users’ jobs to compile classes.

1

/percent

Includes percent classes.

0

/predecessorclasses

Recursively includes dependency predecessor classes.

0

/relatedclasses

Recursively includes related classes.

0

/subclasses

Recursively includes sub-classes.

0

/system

Includes system classes.

0

Export Qualifiers

Flag

Meaning

Default

/checksysutd

Checks system classes for up-to-dateness.

0

/checkuptodate

Checks if classes are up-to-date when projecting.

expandedonly

/createdirs

Creates directories if they do not exist.

0

/cspdeployclass

When CSP page loaded deploys the class generated.

0

/csphidden

Classes generated from CSP and CSR compilation are marked as hidden.

1

/diffexport

Does not include any time or platform information in export so the files can be run through diff/merge tools.

0

/display

Alias qualifier for /displaylog and displayerror.

—

/displayerror

Displays error information.

1

/displaylog

Displays log information.

1

/documatichost

Host that is used in JavaDoc generation.

—

/documaticnamespace

Namespace that is used in JavaDoc generation.

—

/documaticport

Port that is used in JavaDoc generation.

—

/exportgenerated

When exporting classes also exports generated classes where the class generating them is also included.

0

/exportselectivity

Exports the selectivity values stored in the storage definition for this class.

1

/exportversion

Specifies the target Caché version to load this export as a three-part release version, for example 2012.2.2. Cache uses the /exportversion value to handle changes in the export format across versions of Caché by removing class keywords that were not implemented in the earlier Caché version. Specifying /exportversion does not guarantee compatibility of code between the exporting and importing systems.

The current version of Caché

/force

Forces a rebuild of the meta information.

0

/generatemap

Generates the map file.

1

/generationtype

Generation mode.

—

/genserialuid

Generates serialVersionUID.

1

/importselectivity

0: do not import selectivity values from the XML file; 1: import the selectivity values stored in the storage definition when importing XML file; 2: keep any existing selectivity values but if a property does not have an existing value then use the selectivity from the XML file.

2

/includesubpackages

Includes sub-packages.

1

/javadoc

Does not create javadoc.

1

/make

Only generates dependency or class if timestamp of last compilation is greater than timestamp of last generation.

0

/mapped

Includes classes mapped from another database.

0

/mergeglobal

If importing a global from XML file merges the global with existing data.

0

/newcollections

Uses native Java collections.

1

/percent

Includes percent classes.

0

/pojo

POJO generation mode.

0

/predecessorclasses

Recursively includes dependency predecessor classes.

0

/primitivedatatypes

Uses Java primitives for %Integer, %Boolean, %BigInt, %Float.

0

/projectabstractstream

Projects classes that contain methods whose arguments are abstract streams or whose return type is an abstract stream.

0

/projectbyrefmethodstopojo

Projects byref methods to pojo implementation.

0

/recursive

Exports classes recursively.

1

/relatedclasses

Recursively includes related classes.

0

/skipstorage

Does not export the class storage information.

0

/subclasses

Recursively includes sub-classes.

0

/system

Includes system classes.

0

/unconditionallyproject

Projects regardless of problems that may prevent code from compiling or working correctly.

0

/unicode

Exports UNICODE files.

0

/usedeepestbase

Uses deepest base in which method or property is defined for method or property definition. If P is defined in A,B, and C and A extends B extends C, then C is a deeper base for P.

0

ShowClassAndObject Qualifiers

Flag

Meaning

Default

/detail

Shows detailed information.

0

/diffexport

Does not include any time or platform information in export so the files can be run through diff/merge tools.

0

/hidden

Shows hidden classes.

0

/system

Shows system classes.

0

UnitTest

Flag

Meaning

Default

/autoload

Specifies the directory to be auto-loaded.

—

/debug

Causes the Asserts to BREAK if they fail.

0

/delete

Determines if loaded classes should be deleted.

1

/display

Alias qualifier for /displaylog and displayerror.

—

/displayerror

Displays error information.

1

/displaylog

Displays log information.

1

/load

Determines if classes should be loaded; if not, then only classnames are obtained from the directories.

1

/recursive

Determines if tests in subdirectories should run recursively.

1

/run

Determines if tests should run.

1

These qualifiers are given in the qspec as they appear, for example, “/compile/displayerror/force/subclasses”. No spaces are allowed between qualifiers.

Note:

Qualifiers may be negated by preceding the qualifier with “no” as in “/nodisplaylog”. Alternatively, the value of the qualifier can be specified explicitly as in “/displaylog=0”.

Qualifiers For Flags

The following table gives the existing flag and the equivalent qualifier. Some flags map into multiple qualifiers, and also have different meanings when used for differing purposes.

Flag Qualifier Mapping

Flag

Group

Qualifier

Default

a

Compiler

/application

1

b

Compiler

/subclasses

0

c

Compiler

/compile

0

d

Compiler

/displayerror

1

d

Compiler

/displaylog

1

d

UnitTest

/displayerror

1

d

UnitTest

/displaylog

1

e

Compiler

/deleteextent

0

f

Compiler

/force

Deprecated.

f

Export

/force

0

g

Compiler

/exportselectivity

0

i

Compiler

/checkschema

1

k

Compiler

/keepsource

0

l

Compiler

/lock

1

p

Compiler

/percent

0

q

Compiler

/sqlonly (deprecated)

0

r

Compiler

/predecessorclasses

0

r

Compiler

/includesubpackages

1

s

Compiler

/system

0

u

Compiler

/incremental

0

v

Compiler

/keeporefvalid

Deprecated

y

Compiler

/relatedclasses

0

a

Export

/application

1

b

Export

/subclasses

0

d

Export

/displayerror

1

d

Export

/displaylog

1

g

Export

/exportselectivity

0

n

Export

/unicode

0

p

Export

/percent

0

r

Export

/includesubpackages

1

r

Export

/recursive

1

r

Export

/predecessorclasses

0

s

Export

/system

0

y

Export

/relatedclasses

0

d

ShowClassAndObject

/detail

0

h

ShowClassAndObject

/hidden

0

s

ShowClassAndObject

/system

0

Order Of Processing

The qspec is processed from left to right. The setting for a given flag or qualifier overrides the current setting whether it came from the environment defaults, or from an occurrence earlier in the qspec.

When both flags and qualifiers appear, the flags must be placed before (to the left of) the qualifiers. This means that qualifier settings always override any flag settings.

Examples

The following is an example of using $SYSTEM to invoke a method that returns system information:

   WRITE $SYSTEM.OBJ.Version()

 

returns a string such as the following:

Cache Objects Version 2010.2.0.307

Note that this objects version is not formatted the same as the system version number contained in the $ZVERSION special variable.

You can list all of the methods for the OBJ class as follows. (By changing the class name, you can use this method to get a list for any system class):

   DO $SYSTEM.OBJ.Help()

 

To list information about just one method in a class, specify the method name in the Help argument list, as shown in the following example:

   DO $SYSTEM.OBJ.Help("Load")

 

The following are a few more examples of $SYSTEM that invoke methods:

   DO $SYSTEM.OBJ.Upgrade()
   WRITE !,"\* \* \* \* \* \* \* \* \* \* \* "
   DO $SYSTEM.CSP.DisplayConfig()
   WRITE !,"\* \* \* \* \* \* \* \* \* \* \* "
   WRITE !,$SYSTEM.Version.GetPlatform()
   WRITE !,"\* \* \* \* \* \* \* \* \* \* \* "
   WRITE !,$SYSTEM.SYS.TimeStamp()

 

The following example calls the same methods as the previous example, using the ##class(%SYSTEM) syntax form:

   DO ##class(%SYSTEM.OBJ).Upgrade()
   DO ##class(%SYSTEM.CSP).DisplayConfig()
   WRITE !,##class(%SYSTEM.Version).GetPlatform()
   WRITE !,##class(%SYSTEM.SYS).TimeStamp()

 

The previous two examples requires that UnknownUser have assigned the %DB\_CACHESYS role.

See Also

*   [$ISOBJECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisobject) function
    
*   [$ZVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzversion) special variable
    
*   [The Caché Class Library](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_intro) in Using Caché Objects
    
*   [Working with Registered Objects](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_objapi) in Using Caché Objects
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vsystem.xml**