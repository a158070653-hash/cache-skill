# WRITE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cwrite

---

 

Caché ObjectScript Reference

WRITE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite "WRITE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Displays output to current device.

Synopsis

WRITE:pc writeargument,...
W:pc writeargument,...

where writeargument can be:

expression
f
\*integer
\*-integer

Arguments

pc

Optional — A postconditional expression.

expression

Optional — The value to write to the output device. Any valid Caché ObjectScript expression, including literals, variables, object methods, and object properties that evaluates to either a numeric or a quoted string.

f

Optional — One or more [format control characters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_format) that position the output on the target device. Format control characters include !, #, ?, and /.

\*integer

Optional — An [integer code representing a character](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_star) to write to the output device. For ASCII, integers in the range 0 to 255; for Unicode, integers in the range 0 to 65534. Any valid Caché ObjectScript expression that evaluates to an integer in the appropriate range. The asterisk is mandatory.

\*-integer

Optional — A negative integer code specifying a [device control operation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_starminus). The asterisk is mandatory.

Description

The WRITE command displays the specified output on the current I/O device. (To set the current I/O device, use the USE command, which sets the value of the $IO special variable.) WRITE has two forms:

*   WRITE [without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_noarg)
    
*   WRITE [with arguments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_withargs)
    

WRITE without an Argument

WRITE without an argument lists the names and values of all defined variables in the local environment. It lists all defined [local variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local) in collation sequence (alphabetical) order. It does not list process-private global or global variables. It presents these local variables as follows:

varname1=value1
varname2=value2

Note that an argumentless WRITE automatically provides formatting to separate the listed variables, one variable per line. It lists string values enclosed in double quotation marks and lists canonical numbers without quotation marks, as shown in the following example:

  SET str\="fred"
  SET num\=+123.40
  SET canonstr\="456.7"
  SET noncanon1\="789.0"
  SET noncanon2\="+999"
  WRITE

canonstr=456.7
noncanon1="789.0"
noncanon2="+999"
num=123.4
str="fred"

Argumentless WRITE traverses an array in subscript tree order, as shown in the following WRITE output example:

a(1)="United States"
a(1,1)="Northeastern Region"
a(1,1,1)="Maine"
a(1,1,2)="New Hampshire"
a(1,2)="Southeastern Region"
a(1,2,1)="Florida"
a(2)="Canada"
a(2,1)="Maritime Provinces"

An argumentless WRITE displays control characters as an empty string. When issued from a terminal, these control characters are executed. Therefore, local variables that define control characters would display as shown in the following example:

  SET name\="fred"
  SET number\=123
  SET bell\=$CHAR(7)
  SET formfeed\=$CHAR(10)
  SET backspace\=$CHAR(8)
  WRITE

backspace="
bell=""
formfeed="
             "
name="fred"
number=123

An argumentless WRITE must be separated by at least two blank spaces from a command following it on the same line. If the command that follows it is a WRITE with arguments, you must provide the WRITE with arguments with the appropriate line return f format control arguments. This is shown in the following example:

   SET myvar\="fred"
   WRITE  WRITE          ; note two spaces following argumentless WRITE
   WRITE  WRITE myvar    ; formatting needed
   WRITE  WRITE !,myvar  ; formatting provided

Argumentless WRITE listing can be interrupted by issuing a CTRL-C, generating an <INTERRUPT> error.

WRITE with Arguments

WRITE can take a single writeargument or a comma-separated list of writearguments. A WRITE command can take any combination of expression, f, \*integer, and \*-integer arguments.

*   WRITE expression displays the sequence of characters identified by the expression. An expression can be a quoted string, a numeric literal, a variable, or any expression that evaluates to number or a string.
    
*   WRITE f provides any desired [output formatting](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_format). Because the argumented form of WRITE provides no automatic formatting to separate argument values or indicate strings, expression values will display as a single string unless separated by f formatting.
    
*   WRITE \*integer displays [the character represented by the integer code](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_star).
    
*   WRITE \*-integer provides [device control operations](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_starminus).
    

WRITE arguments are separated by commas. For example:

   WRITE "numbers",1,2,3
   WRITE "letters","ABC"

 

displays as:

numbers123lettersABC

Note that WRITE does not append a line return to the end of its output string. In order to separate WRITE outputs, you must explicitly specify f argument formatting characters, such as the line return (!) character.

   WRITE "numbers ",1,2,3,!
   WRITE "letters ","ABC"

 

displays as:

numbers 123
letters ABC

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). You can specify a postconditional expression for an argumentless WRITE or a WRITE with arguments. For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

expression

The value you wish to output. Most commonly this is either a literal (a quoted string or a numeric) or a variable. However, expression can be any valid Caché ObjectScript expression, including literals, variables, arithmetic expressions, object methods, and object properties. For more information on expressions, see [Using Caché ObjectScript](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS).

An expression can be a variable of any type, including [local variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local), [process-private globals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls), [global variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global), and [special variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special) ([ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite) cannot display [special variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special)). You can use extended reference to specify a global variable in another namespace. If you specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error, followed by the global name and database path, such as the following: <PROTECT> ^myglobal,c:\\intersystems\\cache\\mgr\\.

f

A format control to position the output on the target device. You can specify any combination of format control characters without intervening commas, but you must use a comma to separate a format control from an expression. For example, when you issue the following WRITE to a terminal:

   WRITE #!!!?6,"Hello",!,"world!"

The format controls position to the top of a new screen (#), then issue three line returns (!!!), then indent six columns (?6). The WRITE then displays the string Hello, performs a format control line return (!), then displays the string world!. Note that the line return repositions to column 1; thus in this example, Hello is displayed indented, but world! is not.

Format control characters cannot be used with an argumentless WRITE.

For further details, see [Using Format Controls with WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite72) .

\*integer

The \*integer argument allows you to use a positive integer code to write a character to the current device. It consists of an asterisk followed by any valid Caché ObjectScript expression that evaluates to a positive integer that corresponds to a character. The \*integer argument may correspond to a printable character or a control character. An integer in the range of 0 through 255 evaluates to the corresponding 8-bit ASCII character. An integer in the range of 256 through 65534 evaluates to the corresponding 16-bit Unicode character (on Unicode systems).

As shown in the following example, \*integer can specify an integer code, or specify an expression that resolves to an integer code. The following examples all return the word “Caché”:

   WRITE !,"Cach",\*233
   WRITE !,\*67,\*97,\*99,\*104,\*233
   SET accent\=233
   WRITE !,"Cach",\*accent    ; variables are evaluated
   WRITE !,"Cach",\*232+1     ; arithmetic operations are evaluated
   WRITE !,"Cach",\*00233.999 ; fractional numbers are truncated to integers

 

To write the name of the composer Anton Dvorak with the proper Czech accent marks, use:

   IF $SYSTEM.Version.IsUnicode()  {
   WRITE "Anton Dvo",\*345,\*225,"k" }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

The integer resulting from the expression evaluation may correspond to a control character. Such characters are interpreted according to the target device. A \*integer argument can be used to insert control characters (such as the form feed: \*12) which govern the appearance of the display, or special characters such as \*7, which rings the bell on a terminal.

For example, if the current device is a terminal, the integers 0 through 30 are interpreted as ASCII control characters. The following commands send ASCII codes 7 and 12 to the terminal.

   WRITE \*7 ; Sounds the bell
   WRITE \*12 ; Form feed (blank line)

Here’s an example combining expression arguments with \*integer specifying the form feed character:

   WRITE "stepping",\*12,"down",\*12,"the",\*12,"stairs"

\*integer and $X, $Y

An integer expression does not change the $X and $Y special variables when writing to a terminal. Thus, WRITE "a" and WRITE $CHAR(97) both increment the column number value contained in $X, but WRITE \*97 does not increment $X.

You can issue a backspace (ASCII 8), a line feed (ASCII 10), or other control character without changing the $X and $Y values by using \*integer. The following Caché Terminal examples demonstrate this use of integer expressions.

Backspace:

  WRITE $X,"/",$CHAR(8),$X   ; displays: 01
  WRITE $X,"/",\*8,$X         ; displays: 02

Linefeed:

  WRITE $Y,$CHAR(10),$Y
     /\* displays: 1
                   2  \*/
  WRITE $Y,\*10,$Y
     /\* displays: 4
                   4  \*/

For further details, see the [$X](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx) and [$Y](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy) special variables, and “[Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio)” in Caché I/O Device Guide.

\*-integer

An asterisk followed by a negative integer is a device control code. WRITE supports the following general device control codes:

Code

Device Operation

\*-1

Clears the input buffer upon the next READ.

\*-2

Disconnects a TCP device or a named pipe. See [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) and [Local Interprocess Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_interproccomm) in the Caché I/O Device Guide.

\*-3

Flushes the output buffer to the device. This forces a write to the file on disk.

\*-9

Truncates the contents of a sequential file at the current file pointer position. In order to truncate a file, the file must be open (using the OPEN command with at least “RW” access) and must be established as the current device (using the USE command). See [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in the Caché I/O Device Guide.

\*-10

Clears the input buffer immediately.

\*-99

Sends compressed stream data. See [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) in the Caché I/O Device Guide.

WRITE also supports a number of device control codes for magnetic tape devices. These device control codes are described in the [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) chapter of the Caché I/O Device Guide.

Input Buffer Controls

The \*-1 and \*-10 controls are used for input from a terminal device. These controls clear the input buffer of any characters that have not yet been accepted by a READ command. The \*-1 control clears the input buffer upon the next READ. The \*-10 control clears the input buffer immediately. If there is a pending CTRL-C interrupt when WRITE \*-1 or WRITE \*-10 is invoked, WRITE dismisses this interrupt before clearing the input buffer.

An input buffer holds characters as they arrive from the keyboard, even those the user types before the routine executes a READ command. In this way, the user can type-ahead the answers to questions even before the prompts appear on the screen. When the READ command takes characters from the buffer, Caché echoes them to the terminal so that questions and answers appear together. When a routine detects errors it may use the \*-1 or \*-10 control to delete these type-ahead answers. For further details, see [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in the Caché I/O Device Guide.

For use of \*-1 in [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) refer to the Caché I/O Device Guide.

Output Buffer Controls

The \*-3 control is used to flush data from an output buffer, forcing a write operation on the physical device. Thus it first flushes data from the device buffer to the operating system I/O buffer, then forces the operating system to flush its I/O buffer to the physical device. This control is commonly used when forcing an immediate write to a sequential file on disk. \*-3 is supported on Windows and UNIX platforms. On other operating system platforms it is a no-op.

For use of \*-3 in [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) refer to the Caché I/O Device Guide.

Examples

In the following example, the WRITE command sends the current value in variable var1 to the current output device.

  SET var1\="hello world"
  WRITE var1

 

In the following example, both WRITE commands display the Unicode character for pi. The first uses the $CHAR function, the second a \*integer argument:

   IF $SYSTEM.Version.IsUnicode()  {
   WRITE !,$CHAR(960)
   WRITE !,\*960
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

(Note that the above example requires a Unicode installation of Caché.)

The following example writes first name and last name values along with an identifying text for each. The WRITE command combines multiple arguments on the same line. It is equivalent to the two WRITE commands in the example that follows it. The ! character is a format control that produces a line break. (Note that the ! line break character is still needed when the text is output by two different WRITE commands.)

   SET fname\="Bertie"
   SET lname\="Wooster"
   WRITE "First name: ",fname,!,"Last name: ",lname

 

is equivalent to:

   SET fname\="Bertie"
   SET lname\="Wooster"
   WRITE "First name: ",fname,!
   WRITE "Last name: ",lname

 

In the following example, assume that the current device is the user’s terminal. The READ command prompts the user for first name and last name and stores the input values in variables fname and lname, respectively. The WRITE command displays the values in fname and lname for the user’s confirmation. The string containing a space character (" ") is included to separate the output names.

Test
  READ !,"First name: ",fname
  READ !,"Last name: ",lname
  WRITE !,fname," ",lname
  READ !,"Is this correct? (Y or N) ",check#1
  IF "Nn"\[check {
    GOTO Test 
  }

The following example writes the current values in the client(1,n) nodes.

SetElementValues
   SET client(1,1)\="Betty Smith"
   SET client(1,2)\="123 Primrose Path"
   SET client(1,3)\="Johnson City"
   SET client(1,4)\="TN"
DisplayElementValues
   SET n\=1
   WHILE $DATA(client(1,n)) { 
      WRITE client(1,n),! 
      SET n\=n+1 
    }
    RETURN

 

The following command writes the current value of an object property:

   WRITE person.LastName

where person is the object reference, and LastName is the object property name. Note that dot syntax is used in object expressions; a dot is placed between the object reference and the object property name or object method name.

The following command writes the value returned by the object method TotalLines():

   WRITE invoice.TotalLines()

A write command for objects can take an expression with cascading dot syntax, as shown in the following example:

   WRITE patient.Doctor.Hospital.Name

In this example, the patient.Doctor object property references the Hospital object, which contains the Name property. Thus, this command writes the name of the hospital affiliated with the doctor of the specified patient. The same cascading dot syntax can be used with object methods.

A write command for objects can be used with system-level methods, such as the following data type property method:

   WRITE patient.AdmitDateIsValid(date)

In this example, the AdmitDateIsValid() property method returns its result for the current patient object. AdmitDateIsValid() is a boolean method for data type validation of the AdmitDate property. Thus, this command writes a 1 if the specified date is a valid date, and writes 0 if the specified date is not a valid date.

Note that any object expression can be further specified by declaring the class or superclass to which the object reference refers. Thus, the above examples could also be written:

   WRITE ##class(Patient)patient.Doctor.Hospital.Name

   WRITE ##class(Patient)patient.AdmitDateIsValid(date)

Notes

WRITE with $X and $Y

A WRITE displays the characters resulting from the expression evaluation one at a time in left-to-right order. Caché records the current output position in the $X and $Y special variables, with $X defining the current column position and $Y defining the current row position. As each character is displayed, $X is incremented by one.

In the following example, the WRITE command gives the column position after writing the 11–character string Hello world.

   WRITE "Hello world"," "\_$X," is the column number"

Note that writing a blank space between the displayed string and the $X value (," ",$X) would cause that blank space to increment $X before it is evaluated; but concatenating a blank space to $X (," "\_$X) displays the blank space, but does not increment the value of $X before it is evaluated.

Even using a concatenated blank, the display from $X or $Y does, of course, increment $X, as shown in the following example:

   WRITE $Y," "\_$X
   WRITE $X," "\_$Y

In the first WRITE, the value of $X is incremented by the number of digits in the $Y value (which is probably not what you wanted). In the second WRITE, the value of $X is 0.

With $X you can display the current column position during a WRITE command. To control the column position during a WRITE command, you can use the ? format control character. The ? format character is only meaningful when $X is at column 0. In the following WRITE commands, the ? performing indenting:

   WRITE ?5,"Hello world",!
   WRITE "Hello",!?5,"world"

Using Format Controls with WRITE

The f argument allows you to include any of the following format control characters. When used with output to the terminal, these controls determine where the output data appears on the screen. You can specify any combination of format control characters.

! Format Control Character

Advances one line and positions to column 0 ($Y is incremented by 1 and $X is set to 0). The actual control code sequence is device-dependent; it generally either ASCII 13 (RETURN), or ASCII 13 and ASCII 10 (LINE FEED).

Caché does not perform an implicit new line sequence for WRITE with arguments. When writing to a terminal it is a good general practice to begin (or end) every WRITE command with a ! format control character.

You can specify multiple ! format controls. For example, to advance five lines, WRITE !!!!!. You can combine ! format controls with other format controls. However, note that the following combinations, though permitted, are not in most cases meaningful: !# or !,# (advance one line, then advance to the top of a new screen, resetting $Y to 0) and ?5,! (indent by 5, then advance one line, undoing the increment). The combination ?5! is not legal.

If the current device is a TCP device, ! does not output a RETURN and LINE FEED. Instead, it flushes any characters that remain in the buffer and sends them across the network to the target system.

\# Format Control Character

Produces the same effect as sending the CR (ASCII 13) and FF (ASCII 12) characters to a pure ASCII device. (The exact behavior depends on the operating system type, device, and record format.) On a terminal, the # format control character clears the current screen and starts at the top of the new screen in column 0. ($Y and $X are reset to 0.)

You can combine # format controls with other format controls. However, note that the following combinations, though permitted, are not in most cases meaningful: !# or !,# (advance one line, then advance to the top of a new screen, resetting $Y to 0) and ?5,# (indent by 5, then advance to the top of a new screen, undoing the increment). The combination ?5# is not legal.

?n Format Control Character

This format control consists of a question mark (?) followed by an integer, or an expression that evaluates to an integer. It positions output at the nth column location (counting from column 0) and resets $X. If this integer is less than or equal to the current column location (n<$X), this format control has no effect. You can reference the $X special variable (current column) when setting a new column position. For example, ?$X+3.

/mnemonic Format Control Character

This format control consists of a slash (/) followed by a mnemonic keyword, and (optionally) a list parameters to be passed to the mnemonic.

/mnemonic(param1,param2,...)

Caché interprets mnemonic as an entry point name defined in the active mnemonic space. This format control is used to perform such device functions as rewinding a magnetic tape or positioning the cursor on a screen. If there is no active mnemonic space, an error results. Amnemonic may (or may not) require a parameter list.

You can establish the active mnemonic space in either of the following ways:

*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[IO Settings\]. View and edit the File, MagTape, Other, or Terminal mnemonic space setting.
    
*   Include the /mnemonic space parameter in the OPEN or USE command for the device.
    

The following are some examples of mnemonic device functions:

Mnemonic

Description

/IC(n)

Inserts spaces for n characters at the current cursor location, moving the rest of the line to the right

/DC(n)

Deletes n characters to the right of the cursor and collapses the line

/EC(n)

Erases n characters to the right of the cursor, leaving blanks in their stead

For further details on mnemonics, see the [Caché I/O Device Guide](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms).

Specifying a Sequence of Format Controls

Caché allows you to specify a sequence of format controls and to intersperse format controls and expressions. When specifying a sequence of format controls it is not necessary to include the comma separator between them (though commas are permitted.) A comma separator is required to separate format controls from expressions.

In the following example, the WRITE command advances the output by two lines and positions the first output character at the column location established by the input for the READ command.

   READ !,"Enter the number: ",num
   SET col\=$X
   SET ans\=num\*num\*num
   WRITE !!,"Its cube is: ",?col,ans

Thus, the output column varies depending on the number of characters input for the READ.

Escape Sequences with WRITE

The WRITE command, like the READ command, provides support for escape sequences. Escape sequences are typically used in format and control operations. Their interpretation is specific to the current device type.

To output an escape sequence, use the form:

   WRITE \*27,"char"

where \*27 is the ASCII code for the escape character, and char is a literal string consisting of one or more control characters. The enclosing double quotes are required.

For example, if the current device is a VT-100 compatible terminal, the following command erases all characters from the current cursor position to the end of the line.

   WRITE \*27,"\[2J"

To provide device independence for a program that can run on multiple platforms, use the SET command at the start of the program to assign the necessary escape sequences to variables. In your program code, you can then reference the variables instead of the actual escape sequences. To adapt the program for a different platform, simply make the necessary changes to the escape sequences defined with the SET command.

WRITE Compared with Other Write Commands

For a comparison of WRITE with the [ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite), [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump), and [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) commands, refer to the [Write Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_cmds_iowrite) features tables in the “Commands” chapter of Using Caché ObjectScript.

See Also

*   [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse) command
    
*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite) command
    
*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    
*   [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) command
    
*   [$X](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx) special variable
    
*   [$Y](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy) special variable
    
*   Writing escape sequences for [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) and [Interprocess Communications](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_interproccomm) in the Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) in Caché I/O Device Guide
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    
*   [The Spool Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_spool) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cwrite.xml**