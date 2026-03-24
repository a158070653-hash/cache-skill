# $ZNAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzname

---

 

Caché ObjectScript Reference

$ZNAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzposition "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname "$ZNAME") \]

Go to: Description Parameters Examples SQL Identifiers See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates the specified name string as a legal identifier.

Synopsis

$ZNAME(string,type,lang)

Parameters

string

The name to evaluate, specified as a quoted string.

type

Optional — An integer code specifying the type of name validation to perform. Valid values are 0 through 6. The default is 0.

lang

Optional — An integer code specifying the language mode to use when validating string. Valid values are 0 through 12. The default is to use the current language mode.

Description

$ZNAME returns 1 (true) if the string parameter is a legal identifier. Otherwise, $ZNAME returns 0 (false). The optional type parameter determines what type of name validation to perform on the string. If this parameter is omitted, the validation defaults to local variable naming conventions. The optional lang parameter specifies what language mode conventions to apply to the validation.

Your locale may not permit the use of an identifier that $ZNAME validates as a legal identifier. The valid identifier characters for your locale are defined in the National Language Support (NLS) Identifier locale setting; they are not user-modifiable. For further details on NLS, refer to the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.

$ZNAME only performs character validation; it does not perform string length validation for identifiers.

Parameters

string

A quoted string to validate as a legal identifier name. The characters a valid string can contain depend both on the type of identifier to validate (specified by type), the language mode (lang), and the definition of your locale. The string specifies only the identifier name; it should not include prefix characters, such as the caret (^) prefix and the optional delimited namespace name prefix for a global, or suffix characters, such an array subscript or parameter parentheses. By default, the following are valid identifier characters in Caché:

*   Uppercase letters: A through Z (ASCII 65–90)
    
*   Lowercase letters: a through z (ASCII 97–122)
    
*   Letters with accent marks: (ASCII 192–255, exclusive of ASCII 215 and 247)
    
*   Unicode installs: Letters in other character sets. For example, the Greek letters
    
*   Digits: 0 through 9 (ASCII 48–57) subject to positional restrictions for some identifiers
    
*   The percent sign: % (ASCII 37) subject to positional restrictions for some identifiers
    

Note:

The Japanese locale does not support accented Latin letter characters in identifiers. Japanese identifiers may contain (in addition to Japanese characters) the Latin letter characters A-Z and a-z (65–90 and 97–122), and the Greek capital letter characters (913–929 and 931–937).

type

An integer code specifying the type of name validation to perform:

Value

Meaning

Restricted Characters

0

Validate a [local variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local) name.

First character only: %

Subsequent characters only: digits 0–9

1

Validate a routine name.

First character only: %

Subsequent characters only: digits 0–9 and the period (.) character. A period cannot be the first or last character in a routine name.

2

Validate a [label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) (tag) name.

First character only: %

3

Validate a [global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global) or [process-private global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls) name.

First character only: %

Subsequent characters only: digits 0–9 and the period (.) character. A period cannot be the first or last character in a global name.

4

Validate a fully qualified [class name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_classes#GOBJ_classes_namingconventions).

First character only: %

Subsequent characters only: digits 0–9 and the period (.) character. A period cannot be the first or last character in a routine name. (See below.)

5

Validate a method name.

First character only: %

Subsequent characters only: digits 0–9

6

Validate a property name

First character only: %

Subsequent characters only: digits 0–9

If type = 0 (or not specified), an identifier that passes validation may be used for a local variable name, or for any other type of Caché ObjectScript name. This is the most restrictive form of validation. The first character of a valid identifier must be either a percent sign (%) or a valid letter character. The second and subsequent characters of a valid identifier must be either a valid letter character or a digit.

If type = 2, an identifier that passes validation may be used for a line label. This is the only type of identifier that allows a digit (0–9) as the first character. Specify only the label name; do not specify a colon prefix (used in triggers) or parameter parentheses following the label name.

If type = 3, an identifier that passes validation may be used for global and process-private global names. However, global and process-private global names cannot include wide characters; $ZNAME considers wide-character letters to be valid identifier characters for all name validation types. Therefore, if type\=3, an identifier containing wide character letters passes $ZNAME validation, but generates a <WIDE CHAR> error when used as a global name or process-private global name.

If type = 4, an identifier that passes validation may be used for a class name. A class name can contain periods, with the following restrictions: a period may not be immediately followed by a number character or by another period. These restrictions on the use of periods do not apply to type\=1 and type\=3 validation. No valid identifier of any type may have a period as the first or last character of string.

lang

An integer code specifying the language mode to use for validation. Caché applies the conventions of the specified language mode to the validation without changing the current language mode. (For a list of available current language modes, see the [LanguageMode()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#LanguageMode) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class.) The default is for $ZNAME to use the language mode conventions of the current language mode. Because all Caché language modes use the same naming conventions, lang can be omitted and take the default, except to specify lang\=11.

You use lang\=11 to validate Caché MVBasic local variables. If you specify $ZNAME(string,0,11), $ZNAME follows the MultiValue Basic naming conventions for local variables, which differ from the Caché ObjectScript local variable naming conventions. For further details, see [User Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RVBS_gvariables) in the Caché MultiValue Basic Reference.

Examples

The following example shows the $ZNAME function validating the expressions as true (1). Note that the last two examples contain periods, which are permitted in routine names (type\=1) and global names (type\=3):

   WRITE !,$ZNAME("A")
   WRITE !,$ZNAME("A1")
   WRITE !,$ZNAME("%A1",0)
   WRITE !,$ZNAME("%A1",1)
   WRITE !,$ZNAME("A.1",1)
   WRITE !,$ZNAME("A.1",3)

 

In the following example, the first $ZNAME fails validation (returns 0) because (by default) it validates for a local variable name, and the first character of a local variable name cannot be a digit. The second $ZNAME passes validation (returns 1) because type\=2 specifies label validation, and first character of a label name can be a digit.

   WRITE "local var: ",$ZNAME("1A"),!
   WRITE "label: ",$ZNAME("1A",2)

 

The following example fails validation for all type values. Caché names of all types cannot contain a percent sign unless it is the first character of the name:

   FOR i\=0:1:6 {
     WRITE "type ",i," is ",$ZNAME("A%1",i),!
   }

 

The following example shows the full set of valid 8-bit identifier characters for local variable names. These valid identifier characters include the letter characters ASCII 192 through ASCII 255, with the exceptions of ASCII 215 and ASCII 247, which are arithmetic symbols:

   FOR n\=1:1:255  {
   IF $ZNAME("A"\_$CHAR(n),0) & $ZNAME($CHAR(n),0){
      WRITE !,$ZNAME($CHAR(n))," ASCII code=",n," Char.=",$CHAR(n) }
   ELSEIF $ZNAME($CHAR(n),0){
      WRITE !,$ZNAME($CHAR(n))," ASCII code=",n," 1st Char.=",$CHAR(n) }
   ELSEIF $ZNAME("A"\_$CHAR(n),0){
      WRITE !,$ZNAME("A"\_$CHAR(n))," ASCII code=",n," Subseq. Char.=",$CHAR(n) }
   ELSE { }
  }
   WRITE !,"All done"

 

The following example passes validation on a Unicode installation of Caché. The Greek letters specified are valid Unicode letters and thus pass $ZNAME validation. However, this name cannot be used for a global or process-private global (type\=3), and may not be usable with some locales (such as the Japanese locale):

   IF $SYSTEM.Version.IsUnicode() {
     WRITE $C(913)\_$C(961)\_$C(947)\_$C(959),!
     FOR i\=0:1:6 {
       WRITE "type ",i," is ",$ZNAME($C(913)\_$C(961)\_$C(947)\_$C(959),i),!
     }
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

SQL Identifiers

SQL identifiers may include punctuation characters (underscore (\_), at sign (@), pound sign (#), and dollar sign ($)) that are not valid characters in Caché ObjectScript identifiers. SQL routine names may not include the percent sign (%) at any location other than the first character. For further details, see [Identifiers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) in the Using Caché SQL.

See Also

*   Cache ObjectScript [Symbols table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_symbols)
    
*   Cache SQL [Symbols table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_symbols)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzposition "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzname.xml**