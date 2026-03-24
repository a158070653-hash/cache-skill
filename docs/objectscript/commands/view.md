# VIEW

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cview

---

 

Caché ObjectScript Reference

VIEW

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview "VIEW") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Reads and writes database blocks and modifies data in memory.

Synopsis

VIEW:pc viewargument
V:pc viewargument

where viewargument is one of the following:

block
offset:mode:length:newvalue

Arguments

pc

Optional — A postconditional expression.

block

A block location, specified as an integer.

offset

An offset, in bytes, from a base address within the memory region specified by mode.

mode

The memory region whose base address will be used to calculate the data to be modified.

length

The length of the data to be modified.

newvalue

The replacement value to be stored at the memory location.

Description

The VIEW command reads and writes database blocks and writes locations in memory. VIEW has two argument forms:

*   VIEW block transfers data between the Caché database and memory.
    
*   VIEW offset:mode:length:newvalue places newvalue in the memory location identified by offset, mode, and length.
    

You can examine data in memory with the [$VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview) function.

Note:

InterSystems recommends that you avoid use of the VIEW command. When used in any environment, it can corrupt memory structures.

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

block

A block location, specified as an integer. If block is a positive integer, VIEW reads that number block into the view buffer. If block is a negative integer, VIEW writes the block currently in the view buffer to that block address. The block and the offset:mode:length:newvalue arguments are mutually exclusive.

If the block is already in a memory buffer, the current contents of the buffer will be copied.

Block location 0 is not a valid location. Attempting to specify VIEW 0 results in a <BLOCKNUMBER> error.

offset

An offset, in bytes, from a base address within the memory region specified by mode.

mode

The memory region whose base address will be used to calculate the data to be modified. See [Modifying Data in Memory](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview#RCOS_cview66) for a description of the possible values.

length

The length of the data to be modified.

Specify the number of bytes as an integer from 1 to 4, or 8. You can also use the letters C or P to indicate the size of an address field (pointer) on the current platform.

If newvalue defines a string, specify the number of bytes as a negative integer, counting from 1. If the length of newvalue exceeds this number, Caché ignores the extraneous characters. If the length of newvalue is less than this number, Caché stores the supplied characters and leaves the rest of the memory location unchanged.

To store a byte value in reverse order (low-order byte at lowest address) append the letter O to the length number and enclose both in double quotes.

newvalue

The replacement value to be stored at the memory location.

Examples

The following example reads the sixth block from the Caché database into the view buffer:

   VIEW 6

The following example writes the view buffer back to the sixth block of the Caché database, presumably after the data has been modified:

   VIEW \-6

The following example copies the string "WXYZ" into four bytes starting at offset ADDR in the view buffer. The expression $VIEW(ADDR,0,-4) would then result in the value "WXYZ":

   VIEW ADDR:0:\-4:"WXYZ"

Notes

Use VIEW with Caution

Use the VIEW command with caution. It is usually used for debugging and repair of Caché databases and Caché system information. It is easy to corrupt memory or your Caché database by using VIEW incorrectly.

VIEW Usage Restricted

The VIEW command is a restricted system capability. It is a protected command because the invoked code is located in the CACHESYS database. For further details, refer to the “CACHESYS Special Capabilities” in the [Assets and Resources](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCAS_rsrcs) chapter of the Caché Security Administration Guide.

The View Buffer

When used to read and write database buffers, the VIEW command works with the view buffer (device 63). The view buffer is a special memory area that you must open before you can perform any VIEW operations.

When you open the view buffer (with the OPEN command), you indicate the Caché database (CACHE.DAT) to be associated with the view buffer. Using the VIEW command, you can then read individual blocks from the Caché database into the view buffer.

After reading a block into the view buffer, you can use the [$VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview) function to examine the data. Or, you can use the VIEW command to modify the data. If you modify the data, you can use the VIEW command again to write the modified block back to the Caché database.

Reading and Writing Data in a Caché Database

Before you can read and write data blocks in a Caché database with VIEW, you must first use the OPEN command to open the view buffer.

1.  Open the view buffer. The view buffer is designated as device number 63. Hence the command is:
    
       OPEN 63:location
    
    where location is the namespace that contains the CACHE.DAT file to be associated with the view buffer. The location is implementation specific. The OPEN 63 command creates the view buffer by allocating a region of system memory whose size is equal to the block size used by the Caché database.
    
2.  Use the VIEW block form to read in a block from the associated Caché database. Specify block as a positive integer. For example:
    
       VIEW 4
    
    This example reads the fourth block from the Caché database into the view buffer. Because the size of the view buffer equals the block size used in the Caché database, the view buffer can contain only one block at any given time. As you read in subsequent blocks, each new block overwrites the current block. To determine which blocks to read in from the Caché database, you should be familiar with the structure of the file.
    
3.  Examine the data in the block with the $VIEW function or modify it with the VIEW command.
    
4.  If you changed any of the data in the view buffer, write it back to the Caché database. To write data, use the VIEW block form but specify a negative integer for block. The block number usually matches the number of the current block in the view buffer, but it does not have to. The specified block number identifies which block in the file will be replaced (overwritten) by the block in the view buffer. For example, VIEW -5 replaces the fifth block in the Caché database with the current block in the view buffer.
    
5.  Close the view buffer using CLOSE 63.
    

Transferring a Block between Caché Databases

When you open the view buffer, Caché does not automatically clear the existing block. This allows you to transfer a block of data from one Caché database to another using the following sequence:

1.  Use OPEN 63 and specify the namespace that contains the first Caché database.
    
2.  Use VIEW to read the desired block from the file into the view buffer.
    
3.  If necessary, use VIEW to modify the data in the view buffer.
    
4.  Use OPEN 63 again and specify the namespace that contains the second Caché database.
    
5.  Use VIEW to write the block from the view buffer to the second Caché database.
    
6.  Use CLOSE 63 to close the view buffer.
    

Modifying Data in Memory

In addition to reading and writing data from a Caché database, the VIEW command allows you to modify data in memory either in the view buffer or in other system memory areas.

To modify data, use the following form:

VIEW offset:mode:length:newvalue

All four arguments are required.

You modify data by storing a new value into a memory location, which is specified as a byte offset from the base address indicated by mode. You specify the amount of memory affected in the length argument.

The possible values for mode are shown in the following table:

Mode

Memory Management Region

Base Address

n\>0

Address space of process n, where n is the value of $JOB for that process, a process ID (pid).

0

0

The view buffer

Beginning of view buffer

\-1

The process’s partition

Beginning of partition

\-2

The system table

Beginning of system table

\-3

The process’s address space

0

\-6

Reserved for InterSystems use

 

\-7

Used only by the integrity checking utility

Special. See the [Caché High Availability Guide](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GHA).

See Also

*   [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command
    
*   [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose) command
    
*   [$VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cview.xml**