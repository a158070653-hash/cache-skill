# JOB

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cjob

---

 

Caché ObjectScript Reference

JOB

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob "JOB") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Runs a process in background.

Synopsis

JOB:pc jobargument,...
J:pc jobargument,...

where jobargument is one of the following:

Local Jobs:

routine(routine-params):(process-params):timeout
routine(routine-params)\[joblocation\]:(process-params):timeout
routine(routine-params)|joblocation|:(process-params):timeout

##class(className).methodName(args):(process-params):timeout
..methodName(args):(process-params):timeout
$CLASSMETHOD(className,methodName,args):(process-params):timeout

Remote Jobs:

routine\[joblocation\]
routine|joblocation|

Arguments

pc

Optional — A postconditional expression.

routine

The routine to be executed by the process created by JOB.

routine-params

Optional — A comma-separated list of parameters to pass to the routine. These parameters can be values, expressions, or existing local variable names. If specified, the enclosing parentheses are required. Routine parameters can only be passed to local jobs.

className.methodName(args)

..methodName(args)

The class method to be executed by the process created by JOB. The className cannot be $SYSTEM; it can be %SYSTEM. If you specify .. in place of className, JOB uses the current class context (the [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis) class). A comma-separated list of args arguments is optional; the enclosing parentheses are required. Omitted arguments are not permitted. For further details on using [$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod), refer to the section [Dynamically Accessing Objects](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos#GOBJ_specialcos_dynaccess) in the “Object-specific ObjectScript Features” chapter of Using Caché Objects.

process-params

Optional — A colon-separated list of positional parameters used to set various elements in the job’s environment. The process-params list is enclosed in parentheses and the parenthesized list preceded by a colon. All process-params are optional; the parentheses are required. To indicate a positional parameter is missing, its colon must be present, though trailing colons may be omitted. The process-params argument can only be specified for local jobs.

timeout

Optional — The number of seconds to wait for the jobbed process to start. Fractional seconds are truncated to the integer portion. The preceding colon is required. The timeout argument can only be specified for local jobs. If omitted, Caché waits indefinitely.

joblocation

Optional — An explicit or implied namespace used to specify the system and directory on which to run a local or remote job. An implied namespace is a directory path or OpenVMS file specification preceded by two caret characters: "^^dir". Enclose joblocation in either square brackets or vertical bars.

You cannot specify a joblocation when jobbing a class method. If joblocation specifies a remote system, you cannot specify routine-params, process-params, or timeout.

If joblocation specifies a local job, you cannot specify the first process parameter (nspace) because this would conflict with the joblocation parameter. Therefore, only the second, third, and fourth process parameters can be specified, and the missing nspace parameter must be indicated by a colon.

Description

JOB creates a separate process known as a job, jobbed process, or background job. The created process runs in the background, independently of the current process, usually without user interaction. A jobbed process inherits its configuration environment from the invoking process, except what is explicitly specified in the JOB command. For example, a jobbed process inherits the locale settings of the parent process, not the system default locale.

By contrast, a routine invoked with the DO command runs in the foreground as part of the current process.

JOB can create a local process on your local system, or it can invoke the creation of a remote process on another system. For more on remote jobs, see [Starting Remote Jobs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob#RCOS_cjob88).

When a job begins, Caché can call a user-written JOB^%ZSTART routine. When a job ends, Caché can call a user-written JOB^%ZSTOP routine. These entry points can be used for maintaining a log of job activity and troubleshooting problems encountered. For further details, see the section on “[Using the Caché ^%ZSTART and ^%ZSTOP Routines](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_startstop)” in Caché Specialized System Tools and Utilities.

Caution:

OpenVMS Only: The Caché JOB command, or the [$ZF(-2)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2) function, should be used to start a Caché process from OpenVMS, rather than using the OpenVMS DCL commands SPAWN/NOWAIT or PIPE. In order for Caché to function properly, it needs to know about the normal and failure exit of all Caché processes. Caché can detect virtually all process exits. However, Caché cannot detect certain exit conditions that can occur when Caché is started with a DCL SPAWN/NOWAIT or PIPE command. If you start Caché with either of these mechanisms, there is a small timing window during which the failure of the parent process can result in the undetected failure of the child process. This can result in an environment-wide hang of Caché.

Arguments

pc

An optional postconditional expression. Caché executes JOB if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

routine

The process to be started. It can take any of the following forms:

Process Specification

Description

label

Specifies a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) within the current routine.

^routine

Specifies a routine that resides on disk. Caché loads the routine from disk and starts execution at the first line of executable code within the routine.

label^routine

Specifies a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) within the named routine, which resides on disk. Caché loads the routine from disk and starts execution at the indicated label.

label+offset

label+offset^routine

Specifies an offset of a specified number of lines from the [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels). Use of offsets can cause problems with program maintenance, and is discouraged. You cannot specify an offset when calling a CACHESYS % routine. If you attempt to do so, Caché issues a <NOLINE> error.

If you are within a procedure block, calling the JOB command starts a child process that is outside the scope of the procedure block. It therefore cannot resolve a label reference within the procedure block. Therefore, for the JOB to reference a label within a procedure, the procedure cannot use a procedure block.

If you specify a nonexistent label, Caché issues a <NOLINE> error. If you specify a nonexistent routine, Caché issues a <NOROUTINE> error. For further details on these errors, refer to the [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable.

routine-params

A comma-separated list of values, expressions, or existing local variable names. The enclosing parentheses are required. This list is known as the actual parameter list. The routine must have a formal parameter list with the same or a greater number of parameters. If you specify extra actual parameters, Caché issues a <PARAMETER> error. Note that if the invoked routine contains a formal parameter list, you must specify the enclosing parentheses, even if you do not pass any parameters. You cannot omit items from the list of routine-params.

You can pass routine parameters only by value, which means that you cannot pass arrays. This is different from the DO command where you can pass parameters by value and by reference. A special consequence of this restriction is you cannot pass arrays with the JOB command, since they are passed only by reference.

You cannot pass an object reference (oref) to a jobbed process. This is because a reference to an object existing in the invoking process context cannot be referenced in the new jobbed process context. Attempting to pass an oref results in passing an empty string ("") to the jobbed process.

When the routine starts, Caché evaluates any expressions and maps the value of each parameter in the actual list, by position, to the corresponding variable in the formal list. If there are more variables in the formal list than there are parameters in the actual list, Caché leaves the extra variables undefined.

Routine parameters can only be passed to local processes. You cannot specify routine parameters when creating a remote job. See [Starting Remote Jobs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob#RCOS_cjob88).

process-params

A colon-separated list of positional parameters used to set various elements in the job’s environment. The preceding colon and enclosing parentheses are required. All of the positional parameters are optional. You can specify up to seven positional parameters for process-params. These seven parameters are:

(nspace:switch:principal-input:principal-output:priority:os-directory:process-name)

Since the parameters are positional, you must specify them in the order shown. If you omit a parameter that precedes a specified parameter, you must include a colon as a placeholder for it.

Process parameters cannot be specified for a remote job.

The following table describes the process parameters:

Process Parameter

Description

nspace

The default namespace of the process. The specified routine is drawn from this namespace. If you omit nspace, your current default namespace is the default namespace of the jobbing process. An invalid namespace may prevent the job from starting. A local job cannot specify both the joblocation argument and namespace as a process parameter; you must omit the nspace process parameter, retaining the placeholder colon.

switch

An integer consisting of the sum of one or more of the following values:

An integer bit mask that can represent zero or more of the following flags:

1 — Pass the symbol table to the spawned job.

2 — Do not use a JOB Server.

4 — Pass an open TCP/IP socket to the spawned job using the principal I/O device ([$PRINCIPAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal)). (Deprecated, use 16 instead. See below.)

8 — Establish the process-specific window for two-digit years of the spawned job to be the system-wide default sliding window definition. Otherwise, the spawned job inherits the sliding window definition of the process issuing the JOB command.

16 — Pass an open TCP/IP socket to the spawned job using current I/O device ([$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio)).

128 through 16384 (in multiples of 32) — An additional integer value that specifies a partition size (in kilobytes) for the JOBbed child process. See ["Specifying Child Process Partition Size”](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob#RCOS_cjob169) for more information.

131072 — On OpenVMS, this value allows JOB to create a process that checks the UAF authorization file and goes through a normal OpenVMS login. This results in a jobbed process that has the same logical name table as the invoking process. These logical names include DECW$DISPLAY, SYS$LOGIN, SYS$LOGIN\_DEVICE, and SYS$SCRATCH. If a JOB Server is enabled, this value causes it to not be used.

The switch value can be the sum of any combination of these integers. For example, a switch value of 13 (1+4+8) passes the symbol table (1), passes the open TCP/IP socket (4), and establishes a process-specific window for two-digit years that is the system-wide default (8).

principal-input

Principal input device for the process.

UNIX®: See principal-output for default.

OpenVMS: The default is your current input device. If your current input device is your principal device and you log off, the jobbed process will be unable to issue JOB commands to start new processes.

OpenVMS: If you do not specify a principal input and output device, the JOB command may not know which device to open and, as a result, does not start the process. To avoid this situation, you should always specify a principal input and output device. If you do not require any I/O, specify the null device: NL:

principal-output

Principal output device for the process. The default is the device you specify for principal-input.

UNIX®: If you do not specify either device, the process uses the default principal device for processes started with the JOB command, which is /dev/null.

OpenVMS: If you do not specify either device, the process uses your current principal input and output devices.

OpenVMS: If the jobbed process is using your current device and you log off, the jobbed process will be unable to issue JOB commands to start new processes. If you do not require any I/O, specify the null device: NL:

priority

UNIX® and OpenVMS — An integer that specifies the priority for the child process (subject to operating system constraints). If not specified, the child process takes the parent process' base priority plus the system-defined job priority modifier. You can use the [$VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview) function to determine the current priority of a job. Windows has a Normal priority of 7. OpenVMS priority ranges between 0 and 15, with 4 as Normal priority. UNIX® priority ranges between -20 and 20, with 0 as Normal priority. In UNIX®, a process cannot give itself an increased priority unless running as root.

os-directory

An operating system working directory for file I/O. The default is to use the working directory inherited from the parent process. This parameter may be ignored on some systems.

process-name

OpenVMS only — A specified process name for the child process. May be up to 15 characters in length. The default is to use a Caché-generated child process name. If specified, it is the user’s responsibility to ensure the uniqueness of this process name.

Switch 4 and Switch 16

The use of switch\=4 is discouraged, because this establishes the passed TCP device as the principal device of the child job. In this case, the child job could halt when it detected the TCP remote connection dropped and would not perform error trapping. Instead, users should use switch\=16, and then in the child job use the [%SYSTEM.INetInfo.TCPName()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.INetInfo#TCPName) method to get the passed TCP device name. In this case, the child job could continue to run when it detected the TCP remote connection dropped, because the principal device of the child job is not the passed TCP device.

timeout

The number of seconds to wait for the jobbed process to start before timing out and aborting the job. The preceding colon is required. You must specify timeout as an integer value or expression. If a jobbed process times out, Caché aborts the process and sets the $TEST special variable to 0 (FALSE). Execution then proceeds to the next command in the calling routine; no error message is issued. If a jobbed process succeeds, Caché sets $TEST to 1 (TRUE). Note that $TEST can also be set by the user, or by a LOCK, OPEN, or READ timeout.

Timeout can only be specified for a local process.

Examples

The following example starts the monitor routine in the background. If the process does not start in 20 seconds, Caché sets $TEST to FALSE (0).

   JOB ^monitor::20
   WRITE $TEST

The following example starts execution of the monitor routine at the line label named Disp.

   JOB Disp^monitor

The following example starts the Add routine, passing it the value in variable num1, the value 8, and the value resulting from the expression a+2. The Add routine must contain a formal parameter list that includes at least three parameters.

   JOB ^Add(num1,8,a+2)

The following example starts the Add routine, which has a formal parameter list, but passes no parameters. In this case, the Add routine must include code to assign default values to its formal parameters, since they receive no values from the calling routine.

   JOB ^Add()

The following example creates a process running your current routine at label AA. The process parameters pass your current symbol table to the routine. It can use a JOB Server.

   JOB AA:("":1) 

The following OpenVMS example creates a process running the ^REPORT routine in implied namespace, the directory \[USER.TEST\]. An implied namespace is a directory path or OpenVMS file specification preceded by two caret characters: "^^dir". Because ^REPORT does not require input from the principal device, the null device is specified as principal input. The file report.L is the principal output device.

   JOB ^REPORT:("^^\[USER.TEST\]"::"NL:":"report.L") 

This following example passes the routine parameters VAL1 and the string "DR." to the routine ^PROG, starting at entry point ABC, in the current namespace. The routine expects two arguments. Caché does not pass the current symbol table to this job, it will use a JOB Server if possible, and use tta5: as principal input and output device.

   JOB ABC^PROG(VAL1,"DR."):(:0:"tta5:")

The following examples show the jobbing of a class method, with a timeout of ten seconds. They use tta5: as principal input and output device.

The following example uses ##class syntax to invoke a class method:

   JOB ##class(MyClass).Run():(:0:"tta5:"):10

The following example uses the $CLASSMETHOD function to invoke a class method:

   JOB $CLASSMETHOD(MyClass,Run):(:0:"tta5:"):10

[$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod) parameters must be passed by value, not by reference, when using JOB $CLASSMETHOD.

The following example uses relative dot syntax (..) to refer to a method of the current object:

   JOB ..CleanUp():10

For further details, refer to “[Object-specific ObjectScript Features](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos)” in Using Caché Objects.

Notes

Caché Assigns Job Numbers and Memory Partitions

After you start a jobbed process, Caché allocates a separate memory partition for it and assigns it a unique job number (also referred to as a Process ID or pid). The job number is stored in the $JOB special variable. The status of the job (including whether or not it was started by a JOB command) is stored in the $ZJOB special variable.

Since jobbed processes have separate memory partitions, they do not share a common local variable environment with the process that created them or with each other. When you start a jobbed process, you can use parameter passing (routine-params) to pass values from the current process to the jobbed process.

If the JOB command fails, it is usually because:

*   There are no free partitions.
    
*   There is not enough memory to create a partition with the characteristics specified by process-params.
    

Jobbed Process Permissions are Platform-dependent

Processes created by the JOB command run as the Caché service account user. This means that you must insure that the Caché service account has explicit permissions to access all necessary resources.

A spawned job process may run under a different userid than that of the process that issued the JOB command. The userid of the spawned job process depends on the platform:

*   On Windows platforms, the job process uses the userid established for the Caché instance.
    
*   On UNIX® platforms, the job process uses the userid of the process that issued the JOB command.
    
*   On OpenVMS platforms, the job process uses the userid of the process that issued the JOB command.
    

Thus, when you spawn a job, you must make sure that the userid for the job process has the necessary permissions to use any files read or written during the job execution.

Communicating Between Jobs

Parameter passing by value can occur in only one direction and only at job start up. For processes to communicate with each other, they must use mutually agreed upon global variables. Such variables are commonly known as scratch globals because their sole purpose is to allow processes to exchange information among themselves.

*   Processes can use the [%SYSTEM.Event](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Event) class methods to communicate between jobs.
    
*   You can pass all local variables in the current process to the invoked process by specifying a special process parameter.
    
*   Processes can communicate between jobs through the IPC (Interprocess Communication) devices (device numbers 224 through 255) or, on UNIX® operating systems, through UNIX® pipes.
    

Establishing Device Ownership

Caché assumes that the invoked routine includes code (that is, OPEN and USE commands) to handle device ownership for the new process. The default device is the null device.

Caché does not assign a default device to any process other than the process started at sign in.

Setting Job Priority

The %PRIO utility allows you to control the priority at which a UNIX® or OpenVMS jobbed process runs. The available options are NORMAL (uses load balancing to adjust CPU usage), LOW, and HIGH. A jobbed process with a priority of HIGH competes on an equal basis with interactive processes for CPU resources.

Caché also allows you to establish default priorities for jobbed processes.

You can use the [SetBatch()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#SetBatch) method to establish a process as executing in batch mode. A batch mode process has a lower priority than a non-batch process.

Using the JOB Command in a Raw Partition (UNIX®)

You can use the JOB command in a raw partition in either of two ways:

*   Issue the JOB command while in the raw partition.
    
*   Issue the JOB command while in another namespace, and specify the raw partition as the nspace process parameter of the JOB command. Here nspace is an implied namespace. An implied namespace is a directory path or OpenVMS file specification preceded by two caret characters: "^^dir". Implied namespace syntax is described in [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.
    

Commands and jobbed processes running in a raw partition must always specify the full pathname when making references to filenames, and must not use any pathname that starts with "." or ".." , as these are special UNIX® files and are not present in a raw partition. Violating either of these rules causes a <DIRECTORY> error.

To obtain the full pathname of the current namespace, you can invoke the [NormalizeDirectory()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.File#NormalizeDirectory) method, as shown in the following example:

   WRITE ##class(%Library.File).NormalizeDirectory("")

 

Alternatively, you can use the UNIX® batch command & instead of the Caché ObjectScript JOB command. For example, to start a routine called Test as a batch job in a raw disk partition, you invoke Caché as follows:

$cache "^Test" &

Receiving Remote JOB Requests with DSM-DDP

With DSM-DDP networking, Caché can accept JOB requests from a remote DSM system. The job request contains the name of a routine to start in a particular namespace on a particular system, in the following format:

JOB label+offset^routine:"Namespace"

The network job request does not support parameter passing.

This feature allows you to port existing server applications from DSM to Caché. The client portion of the application can operate without change on the DSM host.

Starting Remote Jobs

You can send a remote job from one Caché system to another using the following syntax:

JOB routine\[joblocation\] JOB routine|joblocation|

The two forms are equivalent; you can use either square brackets or vertical bars to enclose the joblocation parameter. A remote job cannot pass routine parameters, process parameters, or a timeout.

joblocation

A specification of the location of the job. The enclosing square brackets or vertical bars are required.

The action Caché takes depends on the job location syntax you are using.

joblocation Syntax

Result

\["namespace"\]

Caché checks whether this explicit namespace has its default dataset on the local system or on a remote system. If the default dataset is on the local system, Caché starts the job using the parameters you specify. If the default dataset is on a remote system, Caché starts the remote job in the directory of the namespace’s default dataset.

\["dir","sys"\]

Caché converts this location to the implied namespace form \["^sys^dir"\].

\["^sys^dir"\]

The job runs in the specified directory on the specified remote system. Caché does not allow any routine parameters, process parameters, or timeout specification.

\["^^dir"\]

The job runs in the specified directory (implied namespace) as a local job on the current system using the parameters you specify. An implied namespace is a directory path or OpenVMS file specification preceded by two caret characters: "^^dir".

\["dir",""\]

Caché issues a <COMMAND> error.

Global Mapping with Remote Jobs (Windows)

Caché does not provide global mapping for remote jobs, whether or not global mapping has been defined on the requesting system. To avoid the lack of global mapping, use extended references with your global specifications that point to the location of any globals not in that namespace. If the namespace you specify in an extended reference is not defined on the system you specify, you receive a <NAMESPACE> error. Namespaces and the syntax for extended global references are described in [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

Remote Job Success Confirmation

You do not receive a direct message on the requesting system that indicates whether the job was successfully initiated on the target system. However, Caché does write a message indicating job initiation success or failure on the console log of the target system.

Configuration File on Remote Systems

You must configure the ability to receive remote job requests on any system that will receive them.

On the receiving system, go to the Management Portal, select \[Home\] > \[Configuration\] > \[Advanced Memory Settings\]. Locate netjob to view and edit. When “true”, incoming remote job requests via ECP will be honored on this server. The default is “true”.

The license on the remote system must support enough users to run remotely initiated jobs. You can determine the number of available Caché licenses using class methods of the [%SYSTEM.License](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.License) class, as described in the InterSystems Class Reference.

Using the $ZCHILD and $ZPARENT Special Variables

[$ZPARENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzparent) contains the pid (Process ID) of the process which jobbed the current process, or 0 if the current process was not created through the JOB command.

[$ZCHILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild) contains the pid of the last process created by the JOB command, whether or not the attempt was successful.

Using JOB Servers

JOB Servers are Caché processes that wait to process job requests. Jobbed processes that attach to JOB Servers avoid the added overhead of having to create a new process. Whenever a user issues a JOB command with the switch parameter set to use JOB Server if available, Caché checks to see if any JOB Servers are available to handle it. If not, it will create a process. If there is a free JOB Server, the job attaches to that JOB Server.

When a job halts while running in a JOB Server, the JOB Server hibernates until it receives another job request. A jobbed process not running in a JOB Server exits and the process is deleted.

There are some unavoidable differences between the JOB Server environment and the jobbed process environment, which may be a security concern with jobbed processes executing in JOB Servers. A jobbed process takes on the security attributes of the process that issued the JOB command at both the Caché and the OpenVMS level.

Input and Output Devices

Only one process can own a device at a time. Under OpenVMS it is impossible to close your principal input or output device. This means that a job executing in a JOB Server is unable to perform input or output to your principal I/O devices even though you may close device 0.

Therefore, if you expect a JOB Server to perform input, you must specify:

*   An alternative input device for it
    
*   The null device for an output device (if you do not want to see the output)
    

Failure to follow these guidelines may cause the job executing in the JOB Server to hang if it needs to do any input or output from/to your principal I/O devices. You may find that frequently job output does get through to your terminal (for example, if you have the SHARE privilege), but typically it will not.

Troubleshooting Jobs That Will Not Execute

If your job does not start, check your I/O specification. Your job will not start if Caché cannot open the devices you requested. Note that the null device (NL: on OpenVMS and /dev/null on UNIX®) is always available.

If your job starts but then halts immediately, make sure you have sufficient swap space. Your job receives an error if you do not have enough swap space.

If your job does not start, make sure that you have used the correct namespace in the JOB command. You can test whether a namespace is defined by using the [Exists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Namespace#Exists) method:

   WRITE ##class(%SYS.Namespace).Exists("USER"),!  ; an existing namespace
   WRITE ##class(%SYS.Namespace).Exists("LOSER")   ; a nonexistent namespace

 

If the JOB command is still not working, try the following:

*   Execute the routine with the DO command.
    
*   OpenVMS: Jobbed processes all execute the system login file, SYS$MANAGER:SYLOGIN.COM. Check to make sure it contains no statements that will cause a jobbed process to hang.
    
*   OpenVMS: Jobbed processes that do not run in JOB Servers also execute your LOGIN.COM file. Check this file as well.
    
*   Make sure you are not exceeding the number of processes for which you are licensed in OpenVMS or Caché. You can determine the number of available Caché licenses using class methods of the [%SYSTEM.License](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.License) class, as described in the InterSystems Class Reference.
    
*   If there is a timeout parameter in your JOB command, check whether the speed of your system is using up the timeout period.
    

When many jobs are executing concurrently, the JOB command may hang while waiting for a routine buffer or a license slot. You can interrupt a JOB command by using Ctrl-C.

JOB Command Completion

A jobbed process continues to completion even if the process that created it logs off before that completion.

JOB Command with TCP Devices

You can use the JOB command to implement a TCP concurrent server. A TCP concurrent server allows multiple clients to be served simultaneously. In this mode a client does not have to wait for the server to finish serving other clients. Instead, each time a client requests the server, it spawns a separate subjob for that client which remains open as long as the client needs it. As soon as this subjob has been spawned (indicated by the return of the JOB command), another client may request service and the server will create a subjob for that client as well.

Client/Server Connections in the Non-Concurrent and Concurrent Modes

![](images/rcos_cjob_connections.png)

A concurrent server uses the JOB command with the switch process parameter bit mask bit 4 or bit 16 turned on, and passes to the spawned process the input and output process parameters.

*   If you specify switch bit 4, you must specify the TCP device in both principal–input and principal–output process parameters. You must use the same device for both principal–input and principal–output, as follows:
    
      JOB child:(:4:tcp:tcp)
    
    The spawned process then sets this single I/O device, as follows:
    
      SET tcp\=$IO
    
*   If you specify switch bit 16, you can specify different devices for the TCP device, the principal–input, and the principal–output process parameters, as follows:
    
      USE tcp
      JOB child:(:16:input:output)
    
    USE tcp preceding the JOB command specifies the current device (rather than the principal device), as the TCP device. The spawned process can then set these devices, as follows:
    
      SET tcp\=##class(%SYSTEM.INetInfo).TCPName()
      SET input\=$PRINCIPAL
      SET output\=$IO
      WRITE tcp," ",input," ",output
    
     
    

It is important to note that the JOB command will pass the TCP socket to the jobbed process if the 4 or 16 bit is set. This capability may be combined with other features of the JOB command by adding the appropriate bit code for each additional feature. For example, when switch includes the bit with value 1, the symbol table is passed. To turn on concurrency and pass the symbol table, switch would have a value of 5 (4+1) or 17 (16+1).

Before you issue the JOB command, the TCP device must:

*   Be open
    
*   Be listening on a TCP port
    
*   Have accepted an incoming connection
    

After the JOB command, the device in the spawning process is still listening on the TCP port, but no longer has an active connection. The application should check the $ZA special variable after issuing the JOB command to make sure that the CONNECTED bit in the state of the TCP device was reset.

The spawned process starts at the designated entry point using the specified device(s) as TCP device, principal–input, and principal–output device. The TCP device has the same name in the child process as in the parent process. The TCP device has one attached socket. The USE command is used to establish the TCP device in “M” mode, which is equivalent to “PSTE”. The “P” (pad) option is needed to pad output with record terminator characters. When this mode is set, WRITE ! sends LF (line feed) and WRITE # sends FF (form feed), in addition to flushing the write buffer.

The TCP device in the spawned process is in a connected state: the same state the device would receive after it is opened from a client. The spawned process can use the TCP device with an explicit USE statement. It can also use the TCP device implicitly.

The following example shows a very simple concurrent server that spawns off a child job whenever it detects a connection from a client. JOB uses a switch value of 17, consisting of the concurrent server bit 16 and the symbol table bit 1:

server ;
     SET io\="|TCP|1"
     SET ^serverport\=7001
     OPEN io:(:^serverport:"MA"):200
     IF $TEST\=0 {
      WRITE !,"Cannot open server port"
      QUIT }
     ELSE { WRITE !,"Server port opened" }
loop  ;
     USE io READ x   ; Read for accept
     USE 0  WRITE !,"Accepted connection"
     USE io 
     JOB child:(:17::)  ; Concurrent server bit is on
     GOTO loop
child ;
     SET io\=##class(%SYSTEM.INetInfo).TCPName()
     SET input\=$PRINCIPAL
     SET output\=$IO
     USE io:(::"M")  ; Ensure that "M" mode is used
     WRITE $JOB,!  ; Send job id on TCP device to be read by client
     QUIT
client ;
     SET io\="|TCP|2"
     SET host\="127.0.0.1"
     OPEN io:(host:^serverport:"M"):200  ; Connect to server
     IF $TEST\=0 {
      WRITE !,"Cannot open connection"
      QUIT }
     ELSE { WRITE !,"Client connection opened" }
     USE io READ x#3:200  ; Reads from subjob
     IF x\="" {
        USE 0
        WRITE !,"No message from child"
        CLOSE io
        QUIT }
     ELSE { 
        USE 0
        WRITE !,"Child is on job ",x
        CLOSE io
        QUIT }

The child uses the inherited TCP connection to pass its job ID (in this case assumed to be 3 characters) back to the client, after which the child process exits. The client opens up a connection with the server and reads the child’s job ID on the open connection. In this example, the IPv4 value "127.0.0.1" for the variable "host" indicates a loopback connection to the local host machine (the corresponding IPv6 loopback value is "0:0:0:0:0:0:0:1" or "::1"). You can set up a client on a different machine from the server if "host" is set to the server’s IP address or name. Further details on IPv4 and IPv6 formats can be found in the section “[Use of IPv6 Addressing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_ch_serverconfig#GORIENT_serverconfig_IPv6)” in the chapter “[Server Configuration Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_ch_serverconfig)” in the Caché Programming Orientation Guide.

In principle, the child and client can conduct extended communication, and multiple clients can be talking concurrently with their respective children of the server.

Specifying Child Process Partition Size

The partition size ([$ZSTORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage) size) for a background job is determined as follows:

*   If job servers are active, for any job server process the job partition size will always be 262144 (in kilobytes).
    
*   If job servers are inactive, the job partition size defaults to the default partition size for non-background jobs ([bbsiz](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=AVMEM)). This is the system-wide default in effect when you execute JOB, regardless of the partition size of the parent process from which you issue JOB.
    
    If job servers are enabled, but all of them are active and you start a new job, that job process will not be a job server process, so its $ZSTORAGE value will default to bbsiz. This is also true when job servers are active, but due to loading the job is run in a non-job-server process.
    
*   If job servers are inactive, you can specify the partition size of the jobbed child process in the JOB statement. You specify the partition size (in kilobytes) of the jobbed process in the second process parameter of the JOB command. For example, JOB ^myroutine:(:8192). The value you specify must be a multiple of 32 and must range from 128 through 16384. It also cannot exceed the default partition size; it can only be used to specify a lower value than the default.
    
    You can optionally specify the partition-size process parameter value in combination with other process information that you would normally put in the second process parameter of JOB. Consider the following JOB command: JOB ^myroutine:(:544+1). This command specifies that the symbol table of the jobbing process should be passed to the jobbed process and that the jobbed process should have a partition size of 544K. Although you can specify this second parameter, which passes two values (544 and 1) as 545, 544 +1 is clearer and has exactly the same effect.
    

Note that a job itself can programmatically set its own partition size using [SET $ZSTORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage).

See Also

*   [^$JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_sjob) structured system variable
    
*   [$JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vjob) special variable
    
*   [$TEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest) special variable
    
*   [$ZJOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob) special variable
    
*   [$ZCHILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild) special variable
    
*   [$ZPARENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzparent) special variable
    
*   [$ZF(-1)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-1) function
    
*   [$ZF(-2)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-2) function
    
*   [TCP Client/Server Communication](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_tcp) in Caché I/O Device Guide
    
*   [The Spool Device](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_spool) in Caché I/O Device Guide
    
*   Using the Management Portal to monitor and control processes in [Managing Caché](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSA_manage) in Caché System Administration Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cjob.xml**