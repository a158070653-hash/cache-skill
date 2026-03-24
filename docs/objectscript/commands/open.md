# OPEN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_copen

---

 

Caché ObjectScript Reference

OPEN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen "OPEN") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Acquires ownership of a device or file for input/output operations.

Synopsis

OPEN:pc device:(parameters):timeout:"mnespace",... 
O:pc device:(parameters):timeout:"mnespace",...

Arguments

pc

Optional — A postconditional expression.

device

The device to be opened, specified by a device ID or a device alias. A device ID can be an integer (a device number), a device name, or the pathname of a sequential file. If a string, it must be enclosed with quotation marks. The maximum length of device is 256 characters for Windows and UNIX®, 245 characters for OpenVMS.

parameters

Optional — The list of parameters used to set device characteristics. The parameter list is enclosed in parentheses, and the parameters in the list are separated by colons. Parameters can either be positional (specified in a fixed order in the parameter list) or keyword (specified in any order). A mix of positional and keyword parameters is permitted. The individual parameters and their positions and keywords are highly device-dependent.

timeout

Optional — The number of seconds to wait for the request to succeed, specified as an integer. Fractional seconds are truncated to the integer portion. If omitted, Caché waits indefinitely.

mnespace

Optional — The name of the mnemonic space that contains the control mnemonics to use with this device, specified as a quoted string.

Description

Use the OPEN command to acquire ownership of a specified device (or devices) for input/output operations. An OPEN retains ownership of the device until ownership is released with the CLOSE command.

An OPEN command can be used to open multiple devices by using a comma to separated the specifications for each device. Within the specification of a device, its arguments are separated by using colons (:). If an argument is omitted, the positional colon must be specified; however, trailing colons are not required.

The OPEN command can be used to open devices such as terminal devices, magnetic tape devices, spool devices, TCP bindings, interprocess pipes, named pipes, and interjob communications.

The OPEN command can also be used to open a sequential file. The device argument specifies the file pathname as a quoted string. The parameters argument specifies the parameters governing the sequential file. These parameters can include the option of creating a new file if the specified file does not exist. Specifying the timeout argument, though optional, is strongly encouraged when opening a sequential file.

Sequential file open option defaults are set for the current process using the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class [OpenMode()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#OpenMode) and [FileMode()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#FileMode) methods, and system-wide by using the Config.Miscellaneous class [OpenMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#OpenMode) and [FileMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#FileMode) properties. For much more details on opening sequential files, see [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in the Caché I/O Device Guide.

The OPEN command is not used to access a Caché database file.

On Windows, Caché ObjectScript allocates each process an open file quota between database files and files opened with OPEN. When OPEN causes too many files to be allocated to OPEN commands, you receive a <TOOMANYFILES> error. Caché does not limit the number of open files; the maximum number of open files for each process is a platform-specific setting. Consult the operating system documentation for your system.

Arguments

pc

An optional postconditional expression. Caché executes the OPEN command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the OPEN command if the postconditional expression is false (evaluates to zero). Only one postconditional is permitted, even if the OPEN command opens multiple devices or files. For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

device

The device to be opened. You can specify the device using any of the following:

*   Physical device number, specified as a positive integer. For example, 2 is always the spooler device. This is the only way to specify a magnetic tape device (numbers 47 through 62). This number is internal to Caché and is unrelated to device numbers assigned by the platform operating system.
    
*   Device ID, specified as a quoted string. For example, "|TRM|:|4294318809". This value for the current device is found in the $IO special variable.
    
*   Device alias, specified as a positive integer. A device alias refers to a physical device number.
    
*   File pathname, specified as a quoted string. This is used for opening sequential files. A pathname can be canonical (c:\\myfiles\\testfile) or relative to the current directory (\\myfiles\\testfile).
    

The maximum length of device is 256 characters for Windows and UNIX®, 245 characters for OpenVMS. See "[Specifying the Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen#RCOS_copen50)" for more information.

parameters

The list of parameters used to set operating characteristics of the device to be opened. The enclosing parentheses are required if there is more than one parameter. (It’s good programming practice to always use parentheses when you specify a parameter.) Note the required colon before the left parenthesis. Within the parentheses, colons are used to separate multiple parameters.

The parameters for a device can be specified using either positional parameters or keyword parameters. You can also mix positional parameters and keyword parameters within the same parameter list.

In most cases, specifying contradictory, duplicate, or invalid parameter values does not result in an error. Wherever possible, Caché ignores inappropriate parameter values and takes appropriate defaults.

If you do not specify a list of parameters, Caché uses the device’s default parameters. The default parameters for a device are configurable. Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[Devices\] to display the current list of devices. For the desired device, click “edit” to display its Open Parameters: option. Specify this value in the same way you specify the OPEN command parameters, including the enclosing parentheses. For example, ("AVL":0:2048).

The available parameters are specific to the type of device that is being opened. For more information on device parameters, see [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide.

Positional Parameters

Positional parameters must be specified in a fixed sequence in the parameter list. You can omit a positional parameter (and receive the default value), but you must retain the colon to indicate the position of the omitted positional parameter. Trailing colons are not required; excess colons are ignored. The individual parameters and their positions are highly device-dependent. There are two types of positional parameters: values and letter code strings.

A value can be an integer (for example, record size), a string (for example, host name), or a variable or expression that evaluates to a value.

A letter code string uses individual letters to specify device characteristics for the open operation. For most devices, this letter code string is one of the positional parameters. You can specify any number of letters in the string, and specify the letters in any order. Letter codes are not case sensitive. A letter code string is enclosed in quotation marks; no spaces or other punctuation is allowed within a letter code string (exception: K and Y may be followed by a name delimited by backslashes: thus: K\\name\\). For example, when opening a sequential file, you might specify a letter code string of “ANDFW” (append to existing file, create new file, delete file, fix-length records, write access.) The position of the letter code string parameter, and the meanings of individual letters is highly device-dependent.

Keyword Parameters

Keyword parameters can be specified in any sequence in the parameter list. A parameter list can consist entirely of keyword parameters, or it can contain a mix of positional and keyword parameters. (Commonly, the positional parameters are specified first (in their correct positions) followed by the keyword parameters.) You must separate all parameters (positional or keyword) with a colon (:). A parameter list of keyword parameters has the following general syntax:

OPEN device:(/KEYWORD1=value1:/KEYWORD2=value2:.../KEYWORDn=valuen):timeout

The individual parameters and their positions are highly device-dependent. As a general rule, you can specify the same parameters and values using either a positional parameter or a keyword parameter. You can specify a letter code string as a keyword parameter by using the /PARAMS keyword.

timeout

The number of seconds to wait for the OPEN request to succeed. The preceding colon is required. timeout must be specified as an integer value or expression. If timeout is set to zero (0), OPEN makes a single attempt to open the file. If the attempt fails, the OPEN immediately fails. If the attempt succeeds it successfully opens the file. If timeout is not set, Caché will continue trying to open the device until the OPEN is successful or the process is terminated manually. If you use the timeout option and the device is successfully opened, Caché sets the $TEST special variable to 1 (TRUE). If the device cannot be opened within the timeout period, Caché sets $TEST to 0 (FALSE). Note that $TEST can also be set by the user, or by a JOB, LOCK, or READ timeout.

mnespace

The name of the mnemonic space that contains the device control mnemonics used by this device. By default, Caché provides two mnemonic spaces: ^%XMAG for magnetic tape devices, and ^%X364 (ANSI X3.64 compatible) for all other devices and sequential files. Default mnemonic spaces are assigned by device type.

Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[IO Settings\]. View and edit the File, MagTape, Other, or Terminal mnemonic space setting.

A mnemonic space is a routine that contains entry points for the device control mnemonics used by READ and WRITE commands. The READ and WRITE commands invoke these device control mnemonics using the /mnemonic(params) syntax. These device control mnemonics perform operations such a moving the cursor to a specified screen location or rewinding a magnetic tape.

Use the mnespace argument to override the default mnemonic space assignment. Specify a Caché ObjectScript routine that contains the control mnemonics entry points used with this device. The enclosing double quotes are required. Specify this option only if you plan to use device control mnemonics with the READ or WRITE command. If the mnemonic space does not exist, a <NOROUTINE> error is returned. For further details on mnemonic spaces, see [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in the Caché I/O Device Guide.

Examples

In the following example, the OPEN command attempts to acquire ownership of device 2 (the spooler). The first positional parameter (3) specifies the file number within the ^SPOOL global and the second positional parameter (12) specifies the line number within the file. If you later use the USE command to make this the current device (that is, USE 2), Caché ObjectScript sends subsequent output to the spooler.

   OPEN 2:(3:12)

In the following example, the OPEN command attempts to acquire ownership of the sequential file CUSTOMER within the timeout period of 10 seconds.

   OPEN "\\myfiles\\customer"::10

Note that because no parameters are specified, the parentheses are omitted, but the colon must be present.

The following example opens a sequential file named Seqtest; the letter code positional parameter is ”NRW”. The “N” letter code specifies that if the file does not exist, create a new sequential file with this name. The “R” and “W” letter codes specify that the file is being opened for reading and writing. The timeout is 5 seconds.

  ZNSPACE "%SYS"
  SET dir\=##class(%SYSTEM.Process).CurrentDirectory()  ; determine Caché directory 
  SET seqfilename\=dir\_"Samples\\Seqtest"
  OPEN seqfilename:("NRW"):5
    WRITE !,"Opened a sequential file named Seqtest"
    USE seqfilename
    WRITE "a line of data for the sequential file"
  CLOSE seqfilename:"D"
  WRITE !,"Closed and deleted Seqtest"
  QUIT

 

This example requires that UnknownUser have assigned the %DB\_CACHESYS role.

Notes

Device Ownership and the Current Device

OPEN establishes ownership of the specified device. The process retains ownership of the device until the process either terminates or releases the device with a subsequent CLOSE command. While a device is owned by a process, no other process can acquire or use the device.

A process can own multiple devices at the same time. However, only one device can be the current device. You establish an owned device as the current device with the USE command. The ID of the current device is found in the [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio) special variable.

A process always owns at least one device (designated as device 0), which is its principal device. This device is assigned to the process when it is started and is typically the terminal used to sign onto Caché. The ID of the principal device is found in the [$PRINCIPAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal) special variable.

When a process terminates, Caché issues an implicit CLOSE for each of the devices owned by the process and returns them to the pool of available devices.

Changing Parameters for an Owned Device

To change the parameters for a device that is already owned by the process, you can:

*   Close and then reopen the device with new parameter values.
    
*   If the device is a terminal, TCP, or magnetic tape device, you can issue an OPEN with new parameter values on an already open device.
    

If you specify the device on another OPEN command, any device parameters set by the initial OPEN command remain in effect unless explicitly changed. Depending on the type of device, subsequent I/O may be different than if you had closed and then reopened the device.

For some devices, you can omit the parameters option and later set the desired characteristics with the parameters option on the USE command.

Specifying the Device

When you open a device, you can identify the device by supplying a device number or an alias assigned to the device.

Using Physical Device Numbers

Caché allows you to identify certain devices by supplying their system-assigned physical device numbers. All implementations of Caché recognize the following physical device numbers:

*   0 = The process’s principal device (usually the device at which you sign on).
    
*   2 = The spooler (to store output for later printing).
    
*   63 = The [view buffer](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview#RCOS_cview_buffer).
    

OPEN 63 accepts a namespace, as shown in the following example:

   OPEN 63:"SAMPLES"

If you specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error.

Device 3 is a reserved device in OpenVMS. On all other platforms it is an invalid device; attempting to open it returns a <NOTOPEN> error without waiting for timeout expiration.

See [About I/O Devices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_intro) in Caché I/O Device Guide for more information about device numbers.

Using a Device Alias

An alias is an alternate numeric device ID. It must be a valid device number, it must be unique and cannot conflict with an assigned device number.

You can establish a numeric alias for a device. Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[Devices\] to display the current list of devices and their aliases. For the desired device, click “edit” to edit its Alias: option.

After you have assigned an alias to a device, you can use the OPEN command or the [%IS utility](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms#GIOD_iodc_userspec) to open the device using this alias.

Exceeding the Open File Quota

Caché allocates each process' open file quota between database files and files opened with OPEN. When OPEN causes too many files to be allocated to OPEN commands, you receive a <TOOMANYFILES> error.

Default Record Length

If the record size for sequential files is not specified in the OPEN command, Caché assumes a default record length of 32,767 characters regardless of whether [long strings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) are enabled or not.

See Also

*   [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose) command
    
*   [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse) command
    
*   [$TEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest) special variable
    
*   [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) in Caché I/O Device Guide
    
*   [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) in Caché I/O Device Guide
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    
*   [The Spool Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_spool) in Caché I/O Device Guide
    
*   [^%IS global and %IS utility](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_copen.xml**