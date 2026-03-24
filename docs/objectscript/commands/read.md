# READ

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cread

---

 

Caché ObjectScript Reference

READ

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread "READ") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Accepts input and stores it in a variable.

Synopsis

READ:pc readargument,... 
R:pc readargument,...

where readargument is

fchar
prompt
variable:timeout
\*variable:timeout 
variable#length:timeout

Arguments

pc

Optional — A postconditional expression.

fchar

Optional — One or more format control characters. Permitted characters are !, #, ?, and /.

prompt

Optional — A string literal that provides a prompt or message for user input. Enclose in quotation marks.

variable

The variable to receive the input data. Can be a local variable, a process-private global, or a global. May be unsubscripted or subscripted.

length

Optional — The number of characters to accept, specified as an integer, or an expression or variable that evaluates to an integer. The preceding # symbol is mandatory.

timeout

Optional — The number of seconds to wait for the request to succeed, specified as an integer. Fractional seconds are truncated to the integer portion. The preceding colon (:) is mandatory. If omitted, Caché waits indefinitely.

You can specify more that one fchar or prompt argument by separating the arguments with commas.

Description

The READ command accepts input from the current device. The current device is established using the OPEN and USE commands. The $IO special variable contains the device ID of the current device. By default, the current device is the user terminal.

The READ command suspends program execution until it either receives input from the current device or times out. For this reason, the READ command should not be used in programs executed as background (non-interactive) jobs if the current device is the user terminal.

The variable argument receives the input characters. READ first defines variable, if it is undefined, or clears it if it has a previous value. Therefore, if no data is input for variable (for example, if the READ times out before any characters are entered) variable is defined and contains the null string. This is also true if the only character entered is a terminator character (for example, pressing the <Enter> key from the user terminal). For the effects of an interrupt (for example, <Ctrl-C>) see below.

Note that for fixed-length and variable-length reads, variable does not store the terminator character used to terminate the read operation. Single-character reads handle variable differently; for single-character read use of variable, see below.

If you specify the optional timeout value, a READ can time out before all characters are input. If a READ times out, those characters input before the timeout are stored in variable. Entering a terminator character is not necessary in this case; the characters entered before the timeout are transferred to variable, and the READ terminates, setting $TEST equal to 0.

There are three types of READ operations: variable-length read, fixed-length read, and single-character read. All of these can be specified with or without a timeout argument. A single READ command can include multiple READ operations in any combination of these three types. Each read operation is executed independently in left-to-right sequence. A READ command can also contain any number of comma-separated prompt and fchar arguments.

The three types of READ operations are as follows:

*   A variable-length read has the following format:
    
    READ variable
    
    A variable-length read accepts any number of input characters and stores them in the specified variable. Input is concluded by a terminator character. For a terminal, this terminator is usually supplied by pressing the <Enter> key. The input characters, but not the terminator character, are stored in variable.
    
*   A fixed-length read has the following format:
    
    READ variable#length
    
    A fixed-length read accepts a maximum of length input characters and stores them in the specified variable. Input concludes automatically when the specified number of characters is input, or when a terminator character is encountered. For example, entering two characters in a four-character fix-length read, and then pressing the <Enter> key. The input characters, but not the terminator character (if any), are stored in variable.
    
*   A single-character read has the following format:
    
    READ \*variable
    
    A single-character read accepts a single input character and stores its ASCII numeric value equivalent in the specified variable. It stores the character itself in the $ZB and $KEY special variables. Input concludes automatically when a single character is input. A terminator character is considered a single-character input, and is stored as such. If the optional timeout argument is specified, and a timeout occurs, the timeout sets variable to –1.
    

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

fchar

Any of the following format control codes. When used with user input from the keyboard, these controls determine where a specified prompt or the user input area will appear on the screen.

! starts a new line. You may specify multiple exclamation points

\# starts a new page. On a terminal, it clears the current screen and starts at the top of the new screen.

?n positions at the nth column location, where n is a positive integer.

/keyword(parameters) A device control mnemonic. Performs a device-specific operation, such as positioning the cursor on a video terminal or rewinding a magnetic tape. The slash character (/) is followed by a keyword, which is optionally followed by one or more parameters enclosed in parentheses. Multiple parameters are separated by commas. The keyword is an entry point label into the current device’s mnemonic space routine.

You can establish the default mnemonic space for a device type in either of the following ways:

*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[IO Settings\]. View and edit the File, MagTape, Other, or Terminal mnemonic space setting.
    
*   Include the /mnemonic space parameter in the OPEN or USE command for the device.
    

You can specify multiple format controls. For example: #!!!?20 means to start at the top of a new page (or screen), go down three lines, and then position to column 20. You can intersperse format control characters with other comma-separated READ arguments. For example:

  READ #!!,"Please enter",!,"your name: ",x,"THANK YOU"

Displays something like the following:

\>

Please enter
your name: FRED
THANK YOU
>

prompt

A string literal that provides a prompt or message for user input with the terminal keyboard. Generally, a prompt argument is followed by a variable, so that the user input area follows the displayed literal. You can specify a multi-line prompt or message by using a comma-separated series of prompt and fchar arguments.

variable

The local variable, process-private global, or global that is to receive the input data. It can be either unsubscripted or subscripted. If a specified variable does not already exist, Caché defines it at the beginning of the READ operation. If a specified variable is defined and has a value, Caché clears this value at the beginning of the READ operation.

When you input characters, they are stored in variable as they are input. If the optional timeout argument is specified, and the read operation is interrupted by a timeout, the characters typed up to that point are stored in variable. (However, note the behavior of variable upon a encountered a <Ctrl-C> interrupt, as described below.)

Nonprinting characters (such as <Tab>) are stored in variable. A terminator character can be used to conclude any type of read operation. For example, from a terminal, you press the <Enter> key to conclude a read operation. This terminator character is not stored in variable for a variable-length or fixed-length read. The terminator character is stored in variable for a single-character read.

length

A positive integer specifying the maximum number of characters to accept for a fixed-length read. The READ completes either when the specified number of characters is input, or when it encounters a terminator character. This argument is optional, but if specified the preceding # symbol is required.

Specifying zero or a negative number results in a <SYNTAX> error. However, leading zeros and the fractional portion of a number are ignored, and the integer portion used. You can specify length as a variable or an expression that resolve to an integer.

Note that READ a#1 and READ \*a can both be used to input a single character. However the value stored in variable is different: a#1 stores the input character in variable a; \*a stores the ASCII numeric value for the input character in variable a; both store the input character in the $ZB special variable. These two types of single-character input also differ in how they handle terminator characters and how they handle a timeout.

timeout

The number of seconds to wait for the request to succeed. This argument is optional, but if specified, the preceding colon is required. You must specify timeout as an integer or an expression that evaluates to an integer. The timeout argument sets the $TEST special variable as follows:

*   READ with timeout argument completes successfully (does not time out): $TEST set to 1 (TRUE).
    
*   READ with timeout argument times out: $TEST set to 0 (FALSE).
    
*   READ with no timeout argument: $TEST remains set to its previous value.
    

Note that $TEST can also be set by the user, or by a LOCK, OPEN, or JOB timeout.

If the timeout period expires before the READ completes and some characters have been input (for a variable-length or fixed-length reads) the input characters are stored in variable. If no characters have been input (for a variable-length or fixed-length reads), Caché defines variable (if necessary) and sets it to the null string. If no character has been input for a single-character READ, Caché defines variable (if necessary) and sets it to –1.

Examples

The following example uses the variable-length form of READ to acquire any number of characters from the user. The format control ! starts the prompt on a new line.

   READ !,"Enter your last name: ",lname

The following example uses the single-character form of READ to acquire one character from the user and store it as its ASCII numeric value.

   READ !,"Enter option number (1,2,3,4): ",\*opt
   WRITE !,"ASCII value input=",opt
   WRITE !,"Character input=",$KEY

The following example uses the fixed-length form of READ to acquire exactly three characters from the user.

   READ !,"Enter your 3-digit area code: ",area#3

The following example prompts for three parts of a name: a fixed-length given name (gname) of up to 12 characters, a fixed-length (one-character) middle initial (iname), and a family name (fname) of any length. The gname and iname variables are coded to time out after 10 seconds:

   READ "Given name:",gname#12:10,!,
        "Middle initial:",iname#1:10,!,
        "Family name:",fname
   WRITE $TEST

A timeout of a read operation causes the READ command to proceed to the next read operation. The first two read operations set $TEST whether or not they time out. The third read operation does not set $TEST, so the value of $TEST in this example reflects the result (success or timeout) of the second read operation.

The following example uses indirection to dynamically change the prompt associated with the READ command:

PromptChoice
   READ "Type 1 for numbers or 2 for names:",p,#!!!!
   IF p'=1,p'=2 {WRITE !,"Invalid input" RETURN }
   ELSE {DO DataInput(p) }
DataInput(dtype)
   SET MESS(1)\="""ENTER A NUMBER:"""
   SET MESS(2)\="""ENTER A NAME:"""
   SET x\=1
   READ !,@MESS(dtype),val(x)
   IF val(x)\="" {WRITE !,"Goodbye" RETURN
   }
   ELSE {
        IF dtype\=1,1\=$ISVALIDNUM(val(x)) { WRITE !,"You input number: ",val(x),! }
        ELSEIF dtype\=2 { WRITE !,"You input string: ",val(x),! }
        ELSE { WRITE !,"That is not a number",! }
   SET x\=x+1
   DO DataInput(dtype)
   } 

The following example sets the length of a fixed-length read based on the number of digits of the first number input:

FirstNum
   READ "ENTER LARGEST INTEGER (and press Return): ",val(1)
   SET ibuf\=$LENGTH(val(1))
   WRITE !,"Your largest number is: ",val(1),!
   DO OtherNums(ibuf)
OtherNums(digits)
   SET x\=2
   READ !,"ENTER NEXT INTEGER: ",val(x)#digits
   IF val(x)\="" { WRITE !,"Goodbye" RETURN }
   ELSEIF val(x)\>val(1) { WRITE !,"Number is too big",!
                          DO OtherNums(digits) }
   ELSE { WRITE !,"You input: ",val(x),!
   SET x\=x+1
   DO OtherNums(digits) }

Notes

READ Uses the Current Device

READ inputs character-oriented data from the current I/O device. You must open a device with the OPEN command, then establish it as the current device with the USE command. Caché maintains the current device ID in the $IO special variable.

While the most common use for READ is to acquire user input from the keyboard, you can also use it to input characters from any byte-oriented device, such as a magnetic tape, a sequential disk file, or a communications buffer.

Read Line Recall

Read line recall mode permits a READ command on a terminal device to receive as its input a previously input line. This recalled input line can then be edited. The user must interactively conclude the input of a recalled line in the same way that user-specified input is concluded. Caché supports read line recall for both variable-length terminal reads (READ variable) and fixed-length terminal reads (READ variable#length). Caché does not support read line recall for single-character terminal reads (READ \*variable). To activate read line recall for the current process, use the [LineRecall()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#LineRecall) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. To set the system-wide read line recall default, use the [LineRecall](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#LineRecall) property of the Config.Miscellaneous class. You can also set the OPEN and USE protocols for terminals, as described in the [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) chapter of the Caché I/O Device Guide.

READ Terminators

Caché terminates a read operation when the input string reaches the specified length (for single-character READ and fixed-length READ). For a variable-length READ, Caché terminates reading if the input string reaches the maximum string length for the current process.

Caché also terminates reading when it encounters certain terminator characters. The terminators are determined by the device type. For example, with terminals, the default terminators are RETURN (also known as the <Enter> key) (ASCII 13), LINE FEED (ASCII 10), and ESCAPE (ASCII 27).

You can modify the terminator default when you issue an OPEN or USE command for a device. OPEN and USE allow you to specify a terminator parameter value. See the [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) chapter of the Caché I/O Device Guide for OPEN and USE protocols for terminals. See the [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) chapter of the Caché I/O Device Guide for details about terminators based on device type.

Caché does not store the input terminator with the input value for variable-length and fixed-length reads; it records it in the $KEY and $ZB special variables. Caché does store the input terminator (if specified) as the input value for a single-character read.

Timeout and the $ZA, $ZB, and $TEST Special Variables

Caché records the completion status of a READ in the $TEST, $ZA, and $ZB special variables, as follows:

Type of READ

Variable data

$TEST value

$ZA value

$ZB value

Variable, ended with line return

input characters (or null string if none)

1

0

terminator character

Variable, some input, then timeout

input characters

0

2

null string

Variable, no input, timeout

null string

0

2

null string

Fixed, all chars entered

input characters

1

0

the last character entered

Fixed, line return

input characters (or null string if none)

1

0

terminator character

Fixed, timed out

input characters (or null string if none)

0

2

null string

Single character, data input

ASCII value of input character

1

0

the input character

Single character, terminator character input

ASCII value of terminator character

1

0 <Enter>, 256 <Esc>

terminator character

Single character, timed out

–1

0

2

null string

$ZB and $KEY

The $ZB and $KEY special variables return the exact same value for every type of read, except one. When you perform a fixed-length read and input the specified number of characters, the READ completes without a terminator. In this case, $ZB contains the last character input (the terminating character), and $KEY contains the null string (there being no terminator character).

Interrupts

If there is a pending CTRL-C interrupt when READ is invoked, READ dismisses this interrupt before reading from the terminal.

If a read in progress is interrupted by a CTRL-C interrupt, variable reverts to its previous state. For example, if you input several characters for a read operation, and then issue a CTRL-C, variable reverts to its state before the read operation. That is, if it was undefined, it remains undefined; if it had a previous value, it contains the previous value. This behavior is completely different than a read operation that times out. A read timeout retains the new state of variable, including any characters input before the timeout occurred. If a READ command contains multiple read operations, the interrupt affects only the read operation in progress. To commit or revert multiple read operations as a unit, use transaction processing.

For information on enabling and disabling CTRL-C interrupts, refer to the [BREAK flag](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_flag) command and the [$ZJOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob) special variable.

Reading from Non-Keyboard Devices

As described earlier, READ can be used to acquire input from any character-oriented device. This includes devices such as magnetic tapes and sequential disk files, as well terminal keyboards. However, you must first establish the device to read from as the current device with the OPEN and USE commands.

With non-keyboard devices, you can use any of the three available forms (variable-length, single-character, and fixed-length). The choice of which form to use in any given case depends on the type of terminator available. With fixed-length READ, Caché treats terminators encountered within the input string the same as any other character.

For example, if you are reading from a device that presents data in a line-oriented format with CARRIAGE RETURN/LINE FEED as the line terminator, you can use the variable-length form. In this case, Caché reads each line into variable in its entirety, terminating input only when it reaches the Return (ASCII code 13) at the end of the line. (Remember, from the user input examples shown previously, that <Return> is the input terminator.)

On the other hand, if you are reading from a magnetic tape that presents records as a series of fixed-length fields, you would use the fixed-length (variable#length) form. For example, assume that you have a mag tape that uses a record format consisting of four fields of up to 8, 12, 4, and 6 characters, respectively. You might use code similar to the following to read in the data:

   READ field1#8,field2#12,field3#4,field4#6

In this case, the #n value sets the input terminator for each field.

Which terminator is used for a given device can be set by the device parameters that you specify for that device on the OPEN or USE command.

When reading block-oriented data from magnetic tape, the $ZB special variable contains the number of bytes remaining in the I/O buffer. Its function is entirely different than its use when reading character-oriented data. $ZB does not contain the read terminator character or the last input character during block-oriented I/O. Refer to [$ZB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb) for further details.

Reading Nonprinting Characters

Nonprinting characters are characters outside the standard range of ASCII printable characters. In other words, they are characters whose ASCII codes are less than ASCII 32 or greater than ASCII 127. They are characters that have no single key equivalents on a standard keyboard.

The characters whose codes are less than ASCII 32 are usually used for control operations. They can be entered only in conjunction with the Ctrl key. For example, ETX (ASCII 3) is entered as <Ctrl-C> and is used to assert a BREAK when entered from the keyboard.

The characters whose codes are greater than ASCII 127 are usually used for graphics operations. As a rule, they cannot be entered from the keyboard, but they can be read from other types of devices. For example, ASCII 179 produces the vertical line character.

You can use the READ command to input nonprinting characters as well as the standard ASCII printable characters. However, you must include code to handle the escape sequence used for each such character. An escape sequence is a sequence of characters that starts with the Esc character (ASCII 27). For example, you can code a READ so that the user is allowed to press a function key as a valid input response. Pressing the function key produces an escape sequence that can be different for different types of terminals.

Caché ObjectScript supports input of escape sequences by storing them in the $ZB and $KEY special variables, rather than in the specified variable. For example, for a function key press, Caché stores the Esc code (ASCII 27) in $ZB and $KEY. To handle an escape sequence, you must include code that tests the current value in $ZB or $KEY after each READ because subsequent reads update these special variables and overwrite any previous value. ($ZB and $KEY are very similar, but not identical; see $KEY for details.) To display a nonprinting character, such as an escape sequence, use the ZZDUMP command or the $ASCII function.

Sequential File End-of-File

The behavior of READ when encountering an end-of-file for a sequential file depends on the system-wide default. Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Compatibility Settings\]. View and edit the current setting of SetZEOF. This option controls the behavior when Caché encounters an unexpected end-of-file when reading a sequential file. When set to “true”, Caché sets the $ZEOF special variable to indicate that you have reached the end of the file. When set to “false”, Caché issues an <ENDOFFILE> error. The default is “false”.

To change this end-of-file behavior for the current process, use the [SetZEOF()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#SetZEOF) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. To set the system-wide default for end-of-file behavior, use the [SetZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#SetZEOF) property of the Config.Miscellaneous class.

Default Record Length

If the number of characters to read is not specified, Caché assumes a default length of 32,767 characters, regardless of whether [long strings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) are enabled or not.

See Also

*   [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command
    
*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    
*   [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse) command
    
*   [$KEY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vkey) special variable
    
*   [$TEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest) special variable
    
*   [$ZA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vza) special variable
    
*   [$ZB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb) special variable
    
*   [$ZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof) special variable
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) in Caché I/O Device Guide
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cread.xml**