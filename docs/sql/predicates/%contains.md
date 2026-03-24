# %CONTAINS

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_contains

---

 

Caché SQL Reference

%CONTAINS

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_between "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [%CONTAINS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains "%CONTAINS") \]

Go to: Description Examples Other Equivalence Comparisons See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value to one or more phrases using word-aware matching.

Synopsis

scalar-expression %CONTAINS(word\[,word...\])

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with one or more word strings. Must be of data type %Text.

word

An alphabetic string or comma-separated list of alphabetic strings to match with values in scalar-expression. A word should be a complete word, or a phrase consisting of several complete words. The word or phrase should be delimited with single quotes.

Description

The %CONTAINS predicate allows you to select those data values that match the string or strings specified in word. This comparison operation is word-aware; it is not a simple string match operation. If you specify more than one word, scalar-expression must contain all of the specified word strings. Word strings may be presented in any order. If word does not match any of the scalar expression data values, %CONTAINS returns the null string.

%CONTAINS is a collection predicate. It can only be used in the WHERE clause of a SELECT statement. %CONTAINS cannot be used as a predicate that selects fields for a JOIN operation.

%CONTAINS can be used on a %Text string or a character stream field.

To use %CONTAINS on a string, change the %String property to %Text, and set LANGUAGECLASS and MAXLEN [property parameters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_proplit#GOBJ_datatypes_parameters). For example:

Property MySentences As %Text(LANGUAGECLASS = "%Text.English",MAXLEN = 1000);

Specifying a MAXLEN value (in bytes) is required for [%Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Text) properties.

To use %CONTAINS to search a [character stream field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs), the stream field must be defined as type %Stream.GlobalCharacterSearchable. For example:

Property MyTextStream As %Stream.GlobalCharacterSearchable(LANGUAGECLASS = "%Text.English");

The %CONTAINS predicate is one of the few predicates that can be used on a [stream field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs) in a WHERE clause.

The available languages are English, French, German, Italian, Japanese, Portuguese, and Spanish. See the [%Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Text) package class documentation (in %SYS) in the InterSystems Class Reference for further details.

Caché generates an SQLCODE -309 error if scalar-expression is neither data type %Text nor %Stream.GlobalCharacterSearchable.

Caché generates an SQLCODE -472 error if scalar-expression is not a collection-valued field (property).

Word-Aware Matching

A word should always be a complete word or sequence of words. When the word argument(s) are single words, Caché uses language analysis rules, including punctuation analysis and stemming rules, to match only the specified word. For example, the word argument “set” would match the word “set” or “Set” (not case-sensitive), and also stem forms such as “sets” and “setting”. However, it would not match words such as “setscrew,” “settle,” or “Seth,” even though these words contain the specified string.

Stemming rules provide for matching between any two forms of the word stem. Stemming is performed on both the search term(s) and the searched text. For example, you can specify %CONTAINS('jumping') and match the word “jumps” in the text.

This word-aware matching is fundamentally different from the character-by-character string matching performed by the SQL Contains operator (\[).

Multiple-Word Phrases

The %CONTAINS keyword operator can search for multiple-word word strings, such as 'United States of America'. How these searches are conducted depends upon the setting of the NGRAMLEN property for the class. By default, NGRAMLEN=1, which means that %CONTAINS treats a word argument as a single word, regardless of how many words it actually contains.

If NGRAMLEN is equal or greater than the number of words in a word phrase, the phrase is treated as a word-aware test. That is, comparisons are not case-sensitive, and comparison is word-by-word. Thus 'School of Art' would match 'School of Art', or 'school of art', but not 'School of Arthur Murray'.

If NGRAMLEN is less than the number of words in a word phrase, Caché consults the dictionary of the LANGUAGECLASS to select the least-frequently-occurring term of length NGRAMLEN (or less). For example, when NGRAMLEN=2 you may still use predicates such as myDoc %CONTAINS('big black dog'). In this case, the least-frequently-occurring term of length NGRAMLEN might be 'big black'. Caché then retrieves the myDoc property (typically from the [master map](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_showplan#GSQLOPT_showplan_maps)), and the document is re-parsed to determine if the post-stemming representation of the query pattern appeared in the post-stemming representation of myDoc. This re-parsing is expensive. In the case of the Japanese language, which does not require case conversion, some of the post-processing done by %CONTAINS is unnecessary overhead.

Contains Analysis

%CONTAINS matching is governed by the class parameters of the [%Text.Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Text.Text) system class, found in the %SYS namespace. These parameters allow you to specify, among other things, whether comparison is to be case-sensitive or not case-sensitive, and the treatment of numbers, punctuation characters, and multi-word phrases.

Caché can use specific language analysis rules, including common word analysis (“noise word” lists) and stemming rules, to determine similarity. The available languages are English, French, German, Italian, Japanese, Portuguese, and Spanish.

For a much more detailed treatment of %CONTAINS and %Text, refer to the [%Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Text) package class documentation in the InterSystems Class Reference.

%CONTAINS and %CONTAINSTERM

The %CONTAINS and %CONTAINSTERM predicates perform the same word-aware comparison for their supplied word arguments. They differ in their requirements for multiple-word phrases:

*   A %CONTAINSTERM argument cannot contain noise words. A %CONTAINS argument can contain noise words.
    

The %CONTAINSTERM operator gives superior performance for certain types of comparisons, especially very large search sets. %CONTAINSTERM is preferable when performing comparisons in Japanese.

%CONTAINSTERM comparisons use the collation of the scalar-expression, and are generally not case-sensitive. For this reason, the CASEINSENSITIVE class property is not applicable to %CONTAINSTERM comparisons. Collation can optionally be set to %EXACT for case-sensitive operations.

Use of %CONTAINS in a search of text represented as a [stream field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs) requires that the stemmed, noiseword-filtered text be converted to a string. If such a stream is longer than the maximum length of a string, then applications should use %CONTAINSTERM instead of %CONTAINS, as there is no limitation on size of a stream when %CONTAINSTERM is in use. For information on the maximum length of a string, see the section “[Support for Long String Operations](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_ch_serverconfig#GORIENT_serverconfig_long_strings)” in the chapter “[Server Configuration Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_ch_serverconfig)” in the Caché Programming Orientation Guide.

For further details, refer to the [%CONTAINSTERM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm) predicate.

Collection Predicates

%CONTAINS is a collection predicate. It can be used in most contexts where a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual. It is subject to the following restrictions:

*   You cannot use %CONTAINS in a HAVING clause.
    
*   You cannot use %CONTAINS as a predicate that selects fields for a JOIN operation.
    
*   You cannot associate %CONTAINS with another predicate condition using the OR logical operator if the two predicates reference fields in different tables. For example:
    
    WHERE t1.text %CONTAINS('Continental United States') OR t2.Timezone BETWEEN 5 AND 8 
    
    Because this restriction depends on how the optimizer uses indices, SQL may only enforce this restriction when indices are added to a table. It is strongly suggested that this type of logic be avoided in all queries.
    

iKnow and iFind

The Caché [iKnow text analysis tool](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIKNOW) and [iFind text search tool](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIKNOW_ifind) also provide word-aware analysis. These facilities are entirely separate from [%Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Text) classes. They provide a substantially different and significantly more sophisticated form of textual analysis.

Examples

The following Embedded SQL example performs a %CONTAINS comparison with a literal phrase:

  &sql(SELECT name,stats
       INTO :badname, :badstat
       FROM Sample.Employee
       WHERE status %CONTAINS('an invalid value'))

The following Embedded SQL example uses a host variable to perform a %CONTAINS comparison:

  SET text\="invalid"
  &sql(SELECT name,stats
       INTO :badname, :badstat
       FROM Sample.Employee
       WHERE status %CONTAINS(:text))

Other Equivalence Comparisons

You can perform other types of equivalence comparisons by using string comparison operators. These include the following:

*   An equivalence comparison on the initial character(s) of a string using %STARTSWITH.
    
*   An equivalence comparison on the entire string, using the equal sign operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State \= 'VT'
    
     
    
    This example selects any record that contains the Home\_State field value “VT”. By default, this string comparison is not case-sensitive.
    
*   An non-equivalence comparison on the entire string, using the does not equal operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State <> 'MA'
    ORDER BY Home\_State
    
     
    
    This example selects all records that where the Home\_State field value is not equal to “MA”. By default, this string comparison is not case-sensitive.
    
*   An equivalence comparison on the entire string to multiple values, using the IN keyword operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State IN ('VT','MA','NH','ME')
    ORDER BY Home\_State
    
     
    
    This example selects any record that contains any of the specified Home\_State field values. By default, this string comparison is not case-sensitive.
    
*   An equivalence comparison on the entire string to a value pattern, using the %PATTERN keyword operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State %PATTERN '1U1"C"'
    ORDER BY Home\_State
    
     
    
    This example selects any record that contains a Home\_State field value that matches the pattern of 1U (one uppercase letter) followed by 1"C" (one literal letter “C”). This pattern would be fulfilled by the Home\_State abbreviations “NC” or “SC”.
    
*   An equivalence comparison of a substring to a value, using the contains operator:
    
    SELECT Name FROM Sample.Person
    WHERE Name \[ 'y'
    
     
    
    This example selects all Name records that contain the lowercase letter “y”. By default, this string comparison is case-sensitive.
    
*   An equivalence comparison of a substring with one or more wildcards to a value, using the LIKE keyword operator:
    
    SELECT Name FROM Sample.Person
    WHERE Name LIKE '\_a%'
    
     
    
    This example selects all Name records that contain the letter “a” as the second letter. This string comparison uses the Name collation type to determine whether the comparison is case-sensitive or not case-sensitive.
    

For further details on these and other comparison conditional predicates, refer to the [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause.

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [%CONTAINSTERM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm) predicate
    
*   [%PATTERN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pattern)
    
*   [%SIMILARITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_similarity) function
    
*   [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper)
    
*   [%INTERNAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_internal)
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   “[Queries Invoking Free-text Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_freetext)” in the “Querying the Database” chapter of Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_between "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_contains.xml**