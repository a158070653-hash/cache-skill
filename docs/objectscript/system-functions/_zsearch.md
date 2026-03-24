# $ZSEARCH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzsearch

---

 

Caché ObjectScript Reference

$ZSEARCH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzseek "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZSEARCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsearch "$ZSEARCH") \]

Go to: Description Parameters Examples Notes

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the full file specification, pathname and filename, of a specified file.

Synopsis

$ZSEARCH(target)
$ZSE(target)

Parameter

target

A filename, a pathname, or a null string. May contain one or more \* or ? wildcard characters.

Description

$ZSEARCH returns the full file specification (pathname and filename) of a specified target file or directory. The filename may contain wild cards so that $ZSEARCH can return a series of fully qualified pathnames that satisfy the wild carding.

Note:

Some operating systems use the slash (/) character as the directory path delimiter. Other operating systems use the backslash (\\) character. In this Description, the word “slash” means either slash or backslash, as appropriate.

If the target parameter does not specify a pathname, $ZSEARCH searches the current working directory. $ZSEARCH applies the rules in its matching process in the following order:

1.  $ZSEARCH scans the target to see if it is surrounded with percent characters (%). If $ZSEARCH finds such text, it treats the string as an environment variable. $ZSEARCH performs name translation on the string.
    
2.  $ZSEARCH scans the string that results from the previous step to find the final slash. If $ZSEARCH finds a final slash, it uses the string up to, but not including, the final slash as the path or directory to be searched. If $ZSEARCH does not find a final slash, it searches the current working directory, which is determined by the current namespace.
    
3.  If $ZSEARCH found a final slash in the previous step, it uses the portion of the target string following the final slash as the filename search pattern. If $ZSEARCH did not find a final slash in the previous step, it uses the whole string that results from Step 1 as the filename search pattern.
    

The filename search pattern can be any legal filename string or a filename wildcard expression. The first filename that matches the search pattern is returned as the $ZSEARCH function value. Which is the first matching file is platform-dependent (as described in the Notes section).

If the next invocation of $ZSEARCH specifies the null string as the target, $ZSEARCH continues with the previous target and returns the next filename that matches the search pattern. When there are no more files that match the search pattern, $ZSEARCH returns a null string.

The [NormalizeDirectory()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.File#NormalizeDirectory) method of the [%Library.File](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.File) class can also be used to return the full pathname of a specified file or directory, as shown in the following example:

  ZNSPACE "SAMPLES"
  WRITE ##class(%Library.File).NormalizeDirectory("CACHE.DAT"),!
  ZNSPACE "USER"
  WRITE ##class(%Library.File).NormalizeDirectory("CACHE.DAT")

 

However, NormalizeDirectory() cannot use wildcards.

Wildcards

$ZSEARCH allows the use of the following wildcard expressions within the quoted target string.

Wildcard

Match

\*

Matches any string of zero or more characters.

?

Matches any single character. On OpenVMS systems, this wildcard is the % character.

These wildcards follow the host platform’s usage rules. On Windows, $ZSEARCH performs a case-independent search, then returns the actual case of the located file or directory. For example, “j\*” can match “JOURNAL”, “journal”, or “Journal”; the actual directory name is “Journal”, which is what is returned.

On Windows and UNIX® systems you can also use the following standard pathname symbols: a single dot (.) to specify the current directory, or a double dot (..) to specify its parent directory. These symbols can be used in combination with wildcard characters.

Parameters

target

The following are the available types of values for the target parameter:

Target Type

Description

pathname

An expression that evaluates to a string specifying the path to the file or group of files you want to list.

filename

A filename. The default location is the current dataset.

null string ("")

Returns the next matching file name from the previous $ZSEARCH.

Examples

The following Windows examples find all files ending with “.DAT” as a file extension in the SAMPLES namespace.

   ZNSPACE "SAMPLES"
   SET file\=$ZSEARCH("\*.DAT")
   WHILE file'="" {
       WRITE !,file
       SET file\=$ZSEARCH("")
   }
   WRITE !,"That is all the matching files"
   QUIT

 

returns:

C:\\InterSystems\\Cache\\Mgr\\Samples\\CACHE.DAT

The following Windows example finds all files beginning with the letter “c” in the SAMPLES namespace.

   ZNSPACE "SAMPLES"
   SET file\=$ZSEARCH("c\*")
   WHILE file'="" {
       WRITE !,file
       SET file\=$ZSEARCH("")
   }
   WRITE !,"That is all the matching files"
   QUIT

 

returns:

C:\\InterSystems\\Cache\\Mgr\\Samples\\CACHE.DAT
C:\\InterSystems\\Cache\\Mgr\\Samples\\cache.lck

Notes

Directory Locking

In order to give accurate results, the process keeps the directory open until $ZSEARCH has returned all files in the directory (that is, until $ZSEARCH returns a null string, or a new $ZSEARCH is started). This may prevent other operations, such as deleting the directory. When you start a $ZSEARCH you should always repeat the $ZSEARCH("") until it returns a null string. An alternative, if you do not want to retrieve all files, is to issue $ZSEARCH with a filename that you know does not exist, such as $ZSEARCH(-1).

Windows Support

For Windows, the target parameter is a standard file specification, which may contain wildcard characters (\* and ?). On Windows systems, the \* wildcard can be used to match a dot, but the ? wildcard cannot. Within a name element, ? wildcards cannot match zero characters; however, at the end of a name element trailing ? wildcards are ignored when they match zero characters.

If you do not specify a directory, the current working directory is used. $ZSEARCH returns the first matching entry in the directory in alphabetical order. It returns the full file specification or fully qualified pathname. The drive letter is always returned as an uppercase letter, regardless of how it was specified.

By default, Windows checks only the first three characters of a filename extension suffix. Therefore, $ZSEARCH("\*.doc") would return not only all files with the .doc suffix, but also all files with the .docx suffix. If you wish to limit your search to only .docx files, you must specify the four character suffix: $ZSEARCH("\*.docx")

UNIX® Support

For UNIX®, the target parameter is a standard UNIX® file specification, which may contain wildcard characters (\* and ?). If you do not specify a directory, the current working directory is used.

For UNIX®, $ZSEARCH returns the first active entry in the directory. Since UNIX® does not keep the directory entries in alphabetical order, the returned values are not in alphabetical order. Unlike Windows platforms, the $ZSEARCH function does not return the full file specification or fully qualified pathnames, unless the current working directory is used.

OpenVMS Support

For OpenVMS, the target parameter is a standard OpenVMS file specification, which may contain wildcard characters (\* and %). If you do not specify a directory, the current working directory is used. For OpenVMS, there is no substitution of environment variables or scanning for a slash character in the target.

For OpenVMS, $ZSEARCH returns the first active entry in the directory. $ZSEARCH returns the full file specification, including device and directory.

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzseek "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzsearch.xml**