# $VIEW

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fview

---

 

Caché ObjectScript Reference

$VIEW

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftranslate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwascii "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview "$VIEW") \]

Go to: Description Parameters Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the contents of memory locations.

Synopsis

$VIEW(offset,mode,length)
$V(offset,mode,length)

Parameters

offset

An offset, in bytes, from a base address within the memory region specified by mode. Interpretation is mode-dependent (see below.)

mode

Optional — The memory region whose base address will be used to locate the data. Default is -1.

length

Optional — The length of the data to be returned, in bytes. May also contain a letter “O” reverse order suffix. Default is 1.

Description

$VIEW returns the contents of memory locations.

The view buffer is a special memory area used to store blocks of data read from the Caché database (CACHE.DAT) with the [VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview) command. After reading a block into the view buffer, you can use the $VIEW function with mode 0 to examine the contents of the view buffer. You must open the [view buffer](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview#RCOS_cview_buffer) as device 63 in order to access it; when finished, you should close device 63.

You can also use $VIEW to return [process summary information](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview#RCOS_fview_processinfo).

The $VIEW function is usually used when debugging and repairing Caché databases and system information.

Parameters

offset

The value of this argument depends upon the mode argument, as follows:

*   When mode is 0, -1, or -2, specify a positive integer as the offset from the base address, in bytes, counting from 0.
    
*   When mode is -3, or a positive integer, specify an address in the process address space. The value -1 can be used to retrieve a summary of the process state, as described in “[Process Summary Information](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview#RCOS_fview_processinfo)” below.
    
*   When mode is -5, specify a positive integer that specifies the number of global nodes in the current block. In this case, odd values return full global references and even values return pointers or data.
    

mode

The possible values for mode are shown in the following table. If mode is omitted, the default is -1. Note that some values are implementation specific. Implementation restrictions are noted in the table.

Mode

Memory Management Region

Base Address

0

The view buffer

Beginning of view buffer

\-1

The process’s partition (default)

Beginning of partition

\-2

The system table

Beginning of system table

\-3

The address space for the current process.

0

\-5

Global reference and data

Special. See “[Using Mode -5](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview#RCOS_fview35),” later in the Notes section.

\-6

Reserved for InterSystems use

 

\-7

Used only by the integrity checking utility

Special.

n

When n is a positive number, the address space for the running process n, where n is the pid (value of the $JOB special variable) for that process. Treats offset and length the same as mode -3.

0

length

A length in bytes, or a flag character. Interpretation depends on the mode and offset values:

*   When mode is 0, -1, or -2, -3, or a positive integer (a pid), and offset is a positive integer, the length parameter can be:
    
    *   A negative integer from -1 to the [maximum string length](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) (as a negative integer) to return that length of data as a string. $VIEW returns the specified number of characters starting at the address indicated by offset.
        
    *   A positive integer in the range 1 through 8 (inclusive) to return the decimal value of the data. $VIEW returns from one to four contiguous bytes, or eight contiguous bytes, starting at the address indicated by offset.
        
    *   A letter C or P as a quoted string to indicate a four-byte address on 32-bit systems, or an eight-byte address on 64-bit systems. When specifying C or P, you do not specify a length integer value.
        
    
    To return a byte value in reverse order (low-order byte at lowest address) append a letter O suffix to the length value and enclose the resulting string in double quotes.
    
    If the length parameter is omitted, the default is 1.
    
*   When mode is -3, or a positive integer (a pid), and offset is -1, the length parameter is a flag that specifies the format of the summary information. Specify a length of 1 to return this summary as a delimited string, or 2 to return this summary as a $LIST structure. If the length parameter is omitted, the default is 1.
    
*   When mode is -5, do not specify a length parameter.
    

Notes

$VIEW Usage Restricted

The $VIEW function is a restricted system capability. It is a protected command because the invoked code is located in the CACHESYS database. For further details, refer to the “CACHESYS Special Capabilities” in the [Assets and Resources](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCAS_rsrcs) chapter of the Caché Security Administration Guide.

Process Summary Information

When offset is -1, you can use mode -3 to return summary information from the current process address space as a ^ delimited string, as shown in this example:

  WRITE $VIEW(\-1,\-3,1)

 

You can also return the same information as a $LIST structure, as follows:

  SET infolist \= $VIEW(\-1,\-3,2)
  ZWRITE infolist

 

To return summary information from the address space of a specified process, provide the Process ID (pid) for that process as a positive integer for the mode argument, as shown in this example:

  SET pid\=$PIECE($IO,"|",4)
  WRITE $VIEW(\-1,pid,1)

 

The summary information return value is in the following format:

pid^mode^dev^mem^dir^rou^stat^prio^uic^loc^blk^^^defns^lic^jbstat^mempeak

The fields are defined as follows:

pid

The process ID. See the [Pid](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.ProcessQuery#Pid) property of the SYS.Process class.

mode

\* if in programmer mode, + or – if the job is part of a callin connection, omitted for daemons.

dev

Current open device(s), returned as a comma-separated list. The current device (the $IO device) is indicated by an asterisk (\*) suffix. See the [OpenDevices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.ProcessQuery#OpenDevices) property of the SYS.Process class.

mem

Memory in use in the process partition (in KBs), if the process is not a daemon. See the [MemoryUsed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.ProcessQuery#MemoryUsed) property of the SYS.Process class.

dir

Default directory.

rou

Routine name.

stat

A comma-separated pair of integer counts, bol,gcnt, where bol is the beginning of line token, specifying the number of lines executed, and gcnt is the global count, specifying the total number of FOR loops and XECUTE commands performed.

prio

User’s current base priority. See the [Priority](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.ProcessQuery#Priority) property of the SYS.Process class.

uic

Obsolete, defaults to 0.0. Formerly was high,low representing the high and low order bits of the User Identification Code (uic) for Caché security at version 5.1.

loc

Location, for daemon processes only.

blk

Number of 2K blocks that can be used for buddy block queues. This is the maximum size of user memory space (also known as partition space). See the [MemoryAllocated](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.ProcessQuery#MemoryAllocated) property of the SYS.Process class.

defns

Default namespace. See the [NameSpace](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.ProcessQuery#NameSpace) property of the SYS.Process class.

lic

License bits.

jbstat

Job status, specified as high,low representing the high and low order bits. Refer to [$ZJOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob) special variable for details.

mempeak

Peak memory usage for the process, in kilobytes. This value is approximate to the nearest 64K. See the [MemoryPeak](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.ProcessQuery#MemoryPeak) property of the SYS.Process class.

Using Mode -5

If the current block in the view buffer contains part of a global, specifying -5 for mode returns the global references and the values contained in the block. The length parameter is not valid for a mode of -5.

With a mode of -5, the offset value specifies the number of global nodes in the block, rather than a byte offset from the base address. Odd values return full global references and even values return pointers or data.

For example, to return the full global reference of the nth node in the view buffer, specify n\*2-1 for offset. To return the value of the nth node, specify n\*2. To return the global reference of the last node in the block, specify -1 for the offset value.

$VIEW returns the nodes in collating sequence (that is, numeric). This is the same sequence that the $ORDER function uses. By writing code that expects this sequence, you can quickly perform a sequential scan through a global in the view buffer. (Several of the Caché utilities use this technique.) $VIEW returns a null string ("") if the offset specifies a location beyond the last node in the view buffer. Be sure to include a test for this value in your code.

If the current block is a pointer block, the values returned are Caché block numbers, which are pointers. If the block is a data block, the values returned are the data values associated with the nodes.

If $VIEW issues a <DATABASE> or <FUNCTION> error, it means that the information in the block is neither a valid global reference nor valid data.

The following example shows generalized code for examining the contents of the view buffer. The code first opens the view buffer and prompts for the number of the block to be read in. The FOR loop then cycles through all the offsets in the current block. The $VIEW function uses a mode of -5 to return the value at each offset. The WRITE commands output the resulting offset-value pairs.

When the end of the block is reached, $VIEW returns a null string (""). The IF command tests for this value and writes out the “End of block” message. The QUIT command then terminates the loop and control returns to the prompt so the user can read in another block.

Start OPEN 63
      WRITE !,"Opening view buffer."
      READ !!,"Number of block to read in: ",block QUIT:block\=""
      VIEW block
         FOR i\=1:1 { 
               SET x\=$VIEW(i,\-5)
               IF x\="" {
                    WRITE !!,"End of block: ",block
                    CLOSE 63
                    QUIT }
               WRITE !,"Offset = ",i
               WRITE !,"Value = ",x
        }
        GOTO Start+2
        CLOSE 63
        QUIT

For a global block, typical output produced by this routine might appear as follows:

Opening view buffer.
Number of block to read in:3720
Offset = 1
Value = ^client(5)
Offset = 2
Value = John Jones
Offset = 3
Value = ^client(5,1)
Offset = 4
Value = 23 Bay Rd./Boston/MA 02049
 .
 .
 .
Offset = 126
Value = ^client(18,1,1)
Offset = 127
Value = Checking/45673/1248.00
End of block: 3720
Number of block to read in:

Reverse Order Byte Values (Big-Endian only)

On big-endian systems, you can return byte values in reverse order by using a letter “O” suffix as part of the length parameter. When you specify the letter O in length, $VIEW returns a byte value in reverse order. (The length value must be enclosed in double quotes.) This is shown in the following example:

   USE IO 
   FOR Z\=0:0 {
      WRITE \*\-6 
      SET NEXTBN\=$VIEW(LINKA,0,"3O") 
      QUIT:NEXTBN\=0 }

In the example above, the length parameter of $VIEW is “3O” (3 and the letter O). When run on a big-endian system, this specifies a length of the next three (3) bytes in reverse order (O). Thus, $VIEW starts at a position in memory (the view buffer—as indicated by a mode of 0) and returns the highest byte, the second highest byte, and the third highest byte.

On little-endian systems, the letter “O” is a no-op. A length value of “3O” is the same as a length value of “3”.

You can use the [IsBigEndian()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Version#IsBigEndian) class method to determine which bit ordering is used on your operating system platform: 1=big-endian bit order; 0=little-endian bit order.

  WRITE $SYSTEM.Version.IsBigEndian()

 

See Also

*   [VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview) command
    
*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftranslate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwascii "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fview.xml**