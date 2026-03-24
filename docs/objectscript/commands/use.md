# USE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cuse

---

 

Caché ObjectScript Reference

USE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse "USE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Establishes a device as the current device.

Synopsis

USE:pc useargument,...
U:pc useargument,...

where useargument can be

device:(parameters):"mnespace"

Arguments

pc

Optional — A postconditional expression.

device

The device to be selected as the current device, specified by a device ID or a device alias. A device ID can be an integer (a device number), a device name, or the pathname of a sequential file. If a string, it must be enclosed with quotation marks.

parameters

Optional — The list of parameters used to set device characteristics. The parameter list is enclosed in parentheses, and the parameters in the list are separated by colons. Parameters can either be positional (specified in a fixed order in the parameter list) or keyword (specified in any order). A mix of positional and keyword parameters is permitted. The individual parameters and their positions and keywords are highly device-dependent.

mnespace

Optional — The name of the mnemonic space that contains the control mnemonics to use with this device, specified as a quoted string.

Description

USE device establishes the specified device as the current device. The process must have already established ownership of the device with the OPEN command.

The current device remains current until you issue another USE command to select another owned device as the current device or until the process terminates.

The USE command can establish as the current device such devices as terminal devices, magnetic tape devices, spool devices, TCP bindings, interprocess pipes, named pipes, and inter-job communications. The USE command can also be used to open a sequential file. The device argument specifies the file pathname as a quoted string.

The parameters available with the USE command are highly device-dependent. In many cases the available parameters are the same as those available with the OPEN command; however, some device parameters can only be set using the OPEN command, and other can only be set using the USE command.

The USE command can specify more than one useargument, separated by commas. However, you can only have one current device at a time. If you specify more than one useargument, the device specified in the last useargument becomes the current device. This form of USE may be used to set parameters for several devices, and then establish the last-named device as the current device.

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

device

The device to be selected as the current device. Specify the same device ID (or other device identifier) as you specified in the corresponding OPEN command. For more information on specifying devices, refer to the [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command.

parameters

The list of parameters used to set operating characteristics of the device to be used as the current device. The enclosing parentheses are required if there is more than one parameter. (It is good programming practice to always use parentheses when you specify a parameter.) Note the required colon before the left parenthesis. Within the parentheses, colons are used to separate multiple parameters.

The parameters for a device can be specified using either positional parameters or keyword parameters. You can also mix positional parameters and keyword parameters within the same parameter list.

In most cases, specifying contradictory, duplicate, or invalid parameter values does not result in an error. Wherever possible, Caché ignores inappropriate parameter values and takes appropriate defaults.

The available parameters are, in many cases, the same as those supported for the OPEN command. For sequential files, TCP devices, and interprocess communication pipes some parameters can only be set with the OPEN command; for sequential files some parameters can only be set with the USE command. USE parameters are specific to the type of device that is being selected and to the particular implementation. The USE command keyword parameters are listed by device type in [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in the Caché I/O Device Guide.

If you do not specify a list of USE parameters, Caché uses the device’s default OPEN parameters. The default parameters for a device are configurable. Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[Devices\] to display the current list of devices. For the desired device, click “edit” to display its Open Parameters: option. Specify this value in the same way you specify the OPEN command parameters, including the enclosing parentheses. For example, ("AVL":0:2048).

Positional Parameters

Positional parameters must be specified in a fixed sequence in the parameter list. You can omit a positional parameter (and receive the default value), but you must retain the colon to indicate the position of the omitted positional parameter. Trailing colons are not required; excess colons are ignored. The individual parameters and their positions are highly device-dependent. There are two types of positional parameters: values and letter code strings.

A value can be an integer (for example, record size), a string (for example, host name), or a variable or expression that evaluates to a value.

A letter code string uses individual letters to specify device characteristics for the open operation. For most devices, this letter code string is one of the positional parameters. You can specify any number of letters in the string, and specify the letters in any order. Letter codes are not case sensitive. A letter code string is enclosed in quotation marks; no spaces or other punctuation is allowed within a letter code string (exception: K and Y may be followed by a name delimited by backslashes: thus: K\\name\\). For example, when opening a sequential file, you might specify a letter code string of “ANDFW” (append to existing file, create new file, delete file, fix-length records, write access.) The position of the letter code string parameter, and the meanings of individual letters is highly device-dependent.

Keyword Parameters

Keyword parameters can be specified in any sequence in the parameter list. A parameter list can consist entirely of keyword parameters, or it can contain a mix of positional and keyword parameters. (Commonly, the positional parameters are specified first (in their correct positions) followed by the keyword parameters.) You must separate all parameters (positional or keyword) with a colon (:). A parameter list of keyword parameters has the following general syntax:

USE device:(/KEYWORD1=value1:/KEYWORD2=value2:.../KEYWORDn=valuen):"mnespace"

The individual parameters and their positions are highly device-dependent. As a general rule, you can specify the same parameters and values using either a positional parameter or a keyword parameter. You can specify a letter code string as a keyword parameter by using the /PARAMS keyword.

mnespace

The name of the mnemonic space that contains the device control mnemonics used by this device. By default, Caché provides two mnemonic spaces: ^%XMAG for magnetic tape devices, and ^%X364 (ANSI X3.64 compatible) for all other devices and sequential files. Default mnemonic spaces are assigned by device type.

Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Device Settings\] > \[IO Settings\]. View and edit the File, MagTape, Other, or Terminal mnemonic space setting.

A mnemonic space is a routine that contains entry points for the device control mnemonics used by READ and WRITE commands. The READ and WRITE commands invoke these device control mnemonics using the /mnemonic(params) syntax. These device control mnemonics perform operations such as moving the cursor to a specified screen location or rewinding a magnetic tape.

Use the mnespace argument to override the default mnemonic space assignment. Specify a Caché ObjectScript routine that contains the control mnemonics entry points used with this device. The enclosing double quotes are required. Specify this option only if you plan to use device control mnemonics with the READ or WRITE command. If the mnemonic space does not exist, Caché issues a <NOROUTINE> error. For further details on mnemonic spaces, see [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in the Caché I/O Device Guide.

Examples

In this example, the USE command sets the sequential file "STUDENTS" as the current device and sets the file pointer so that subsequent reads begin at offset 256 from the start of the file.

   USE "STUDENTS":256

Note:

Sequential file seeking with the USE command, such as that shown in the above example, is not supported on OpenVMS systems.

Notes

Device Ownership

Device ownership is established with the OPEN command. The only exception is the principal device, which is assigned to the process and is usually the terminal at which you sign on. If the device specified in the USE command is not owned by the process, Caché issues a <NOTOPEN> error message.

The Current Device

The current device is the device used for I/O operations by the READ and WRITE commands. The READ command acquires input from the current device and the WRITE command sends output to the current device.

Caché maintains the ID of the current device in the $IO special variable. If the USE request is successful, Caché sets $IO to the ID of the specified device. The [GetType()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Device#GetType) method of the [%Library.Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Device) class returns the device type of the current device.

The Principal Device

The special device number 0 (zero) refers to the principal device. Each process has one principal device. Caché maintains the ID of the principal device in the $PRINCIPAL special variable. The principal device is automatically opened when you start up Caché. Initially, the principal device ($PRINCIPAL) and the current device ($IO) are the same.

After you issue a USE command, your current device ($IO) is normally the one named in the last USE command you executed.

While many processes can have the same principal device, only one at a time can own it. After a process successfully issues an OPEN command for a device, no other process can issue OPEN for that device until the first process releases it, either by explicitly issuing a CLOSE command, by halting, or because that user ends the session.

Although you can issue OPEN and USE for a device other than your principal device from the programmer prompt, each time Caché returns to the > prompt, it implicitly issues USE 0. To continue using a device other than 0, you must issue a USE command in each line you enter at the > prompt.

Your principal device automatically becomes your current device when you do any of the following:

*   Log on.
    
*   Issue a USE 0 command.
    
*   Cause an error when an error trap is not set.
    
*   Close the current device.
    
*   Return to programmer mode.
    
*   Exit Caché by issuing a HALT command.
    

USE 0 implies an OPEN command to the principal device. If another process owns the device, this process hangs on the implicit OPEN as it does when it encounters any OPEN.

Although USE 0 implies OPEN 0 for the principal device, issuing a USE command for any other device that the process does not own (due to a previous OPEN command) produces a <NOTOPEN> error.

Note:

While most Caché platforms allow you to close your principal input device, Caché for UNIX® does not. Therefore, when a job that is the child of another job tries to perform I/O on your login terminal, it hangs until you log off Caché. At that time, the output may or may not appear.

Using the Null Device (UNIX® and OpenVMS)

When you issue an OPEN and USE command to the null device (/dev/null on UNIX® or NL: on OpenVMS), Caché treats the null device as a dummy device. Subsequent READ commands immediately return a null string (""). Subsequent WRITE commands immediately return success. No actual data is read or written. On systems based on UNIX®, the device /dev/null bypasses the UNIX® open, write, and read system calls entirely.

Processes started by other processes with the JOB command have a principal device of /dev/null or NL: by default.

If you open /dev/null other than within Caché for example, by redirecting Caché output to /dev/null from the UNIX® shell the UNIX® system calls operate as they do for any other device.

See Also

*   [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command
    
*   [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose) command
    
*   [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio) special variable
    
*   [$PRINCIPAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    
*   [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) in Caché I/O Device Guide
    
*   [Magnetic Tape I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_magtapeio) in Caché I/O Device Guide
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    
*   [The Spool Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_spool) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cuse.xml**