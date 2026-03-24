# %CONTAINSTERM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_containsterm

---

 

Caché SQL Reference

%CONTAINSTERM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [%CONTAINSTERM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm "%CONTAINSTERM") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value to one or more words using word-aware matching.

Synopsis

scalar-expression %CONTAINSTERM(word\[,word...\])

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with one or more word strings.

word

An alphabetic string or comma-separated list of alphabetic strings to match with values in scalar-expression. A word must be a complete word, or a multiple-word phrase (with the words separated by spaces). The word or phrase should be delimited with single quotes. See below for restrictions on multiple-word phrases.

Description

The %CONTAINSTERM predicate allows you to select those data values that match the word or words specified in word. This comparison operation is word-aware; it is not a simple string match operation. If you specify more than one word, scalar-expression must contain all of the specified word strings. Word strings may be presented in any order. If word does not match any of the scalar expression data values, %CONTAINSTERM returns the null string.

%CONTAINSTERM is a collection predicate. It can only be used in the WHERE clause of a SELECT statement.

%CONTAINSTERM can be used on a %Text string or a character stream field.

To use %CONTAINSTERM on a string, change the %String property to %Text, and set LANGUAGECLASS and MAXLEN [property parameters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_proplit#GOBJ_datatypes_parameters). For example:

Property MySentences As %Text(LANGUAGECLASS = "%Text.English",MAXLEN = 1000);

Specifying a MAXLEN value (in bytes) is required for [%Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Text) properties.

To use %CONTAINSTERM to search a [character stream field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs), the stream field must be defined as type %Stream.GlobalCharacterSearchable. For example:

Property MyTextStream As %Stream.GlobalCharacterSearchable(LANGUAGECLASS = "%Text.English");

The %CONTAINSTERM predicate is one of the few predicates that can be used on a [stream field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs) in a WHERE clause.

The available languages are English, French, German, Italian, Japanese, Portuguese, and Spanish. See the [%Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Text) package class documentation (in %SYS) in the InterSystems Class Reference for further details.

Caché generates an SQLCODE -309 error if scalar-expression is neither data type %Text nor %Stream.GlobalCharacterSearchable.

Caché generates an SQLCODE -472 error if scalar-expression is not a collection-valued field (property).

%CONTAINS and %CONTAINSTERM

The %CONTAINS and %CONTAINSTERM predicates perform the same word-aware comparison for their supplied word arguments. They differ in their requirements for multiple-word phrases:

*   A %CONTAINSTERM argument cannot contain noise words. A %CONTAINS argument can contain noise words.
    
*   Every %CONTAINSTERM argument phrase must be NGRAMLEN words or less in length. A %CONTAINS argument phrase can be any number of words in length.
    

The %CONTAINSTERM operator gives superior performance for certain types of comparisons, especially very large search sets. %CONTAINSTERM is preferable when performing comparisons in Japanese.

%CONTAINSTERM comparisons use the collation of the scalar-expression, and are generally not case-sensitive. For this reason, the CASEINSENSITIVE class property is not applicable to %CONTAINSTERM comparisons. Collation can optionally be set to %EXACT for case-sensitive operations.

Word-Aware Matching

Caché uses language analysis rules, including punctuation analysis and stemming rules, to match only the specified word. For example, the word argument “set” would match the word “set” or “Set” (not case-sensitive), and also stem forms such as “sets” and “setting”. However, it would not match words such as “setscrew,” “settle,” or “Seth,” even though these words contain the specified string.

Stemming rules provide for matching between any two forms of the word stem. Stemming is performed on both the search term(s) and the searched text. For example, you can specify %CONTAINSTERM('jumping') and match the word “jumps” in the text.

This word-aware matching is fundamentally different from the character-by-character string matching performed by the SQL Contains operator (\[).

For further details on contains comparison, refer to the [%CONTAINS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains) operator.

Collection Predicates

%CONTAINSTERM is a collection predicate. It can be used in most contexts where a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual. It is subject to the following restrictions:

*   You cannot use %CONTAINSTERM in a HAVING clause.
    
*   You cannot use %CONTAINSTERM as a predicate that selects fields for a JOIN operation.
    
*   You cannot associate %CONTAINSTERM with another predicate condition using the OR logical operator if the two predicates reference fields in different tables. For example:
    
    WHERE t1.text %CONTAINSTERM('Continental United States') OR t2.Timezone BETWEEN 5 AND 8 
    
    Because this restriction depends on how the optimizer uses indices, SQL may only enforce this restriction when indices are added to a table. It is strongly suggested that this type of logic be avoided in all queries.
    

iKnow and iFind

The Caché [iKnow text analysis tool](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIKNOW) and [iFind text search tool](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIKNOW_ifind) also provide word-aware analysis. These facilities are entirely separate from [%Text](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Text) classes. They provide a substantially different and significantly more sophisticated form of textual analysis.

Examples

The following Embedded SQL example performs a %CONTAINSTERM comparison with a literal phrase:

  &sql(SELECT name,stats
       INTO :badname, :badstat
       FROM Sample.Employee
       WHERE status %CONTAINSTERM('invalid value'))

The following Embedded SQL example uses a host variable to perform a %CONTAINSTERM comparison:

  SET text\="invalid"
  &sql(SELECT name,stats
       INTO :badname, :badstat
       FROM Sample.Employee
       WHERE status %CONTAINSTERM(:text))

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [%CONTAINS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains) predicate
    
*   [%SIMILARITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_similarity) function
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   “[Queries Invoking Free-text Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_freetext)” in the “Querying the Database” chapter of Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_containsterm.xml**