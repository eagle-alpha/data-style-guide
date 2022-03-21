# CSV (Comma Separated Value) File Type

### Delimiters
- __[MUST]__ Don't use alphabet or numeric characters as the delimiter. 
- __[MUST]__ Don't use quote characters as the delimiter.
- __[MUST]__ Don't use multi-byte characters such as unicode symbols as the delimiter
- __[SHOULD]__ Use Unicode Basic Latin punctuation or symbols for the delimiter that are not commonly used in your specific data. Commas are not always the best option if your data has a lot of free text. "|" is a relatively commonly used delimiter as it is not common in human-written text. More exotic Unicode Punctuation and Symbols have a dropoff in support by most data parsers. 

### Quoting and Block Text Column Values
- __[MUST]__ If a column value contains the delimiter, quotes must be used for every row of that column, not just for the columns where the delimiter occurs. It is recommended, but not required in this case, to quote all fields for safe measure. 
- __[SHOULD]__ Use the " as the default quotechar. Not to be confused with the more exotic quotation marks that sometimes come onboard from document editors such as Microsoft Word. 
- __[SHOULD]__ Remove newlines (especially Windows newlines which are multi-character __\r\n__) from inside quoted blocks of text contained inside a column value. Many data parsers use conflicting logic when interpreting newlines, with some just assuming ALL newline characters are actual newlines in the file itself, no matter what kind of quoting is used. 
- __[SHOULD]__ Only ever use Linux newline character __\n__ for your actual line terminator. Windows newlines __\r\n__ are multi-character and certain parsers such as Spark just don't parse it, leaving a stray \r hanging around in the final column values.

### Columns
- __[MUST]__ Use only single-line column names. There are almost no situations where a multi-line column header is necessary. Most data ingestion to data warehouses would require a transformation to a single line header, therefore it's best to transform at the source instead of making your customer figure this out. If you're in need of a better column description, consider filling out our data dictionary with descriptions of columns.  

### Null Values
- __[MUST]__ Use the empty value to indicate a NULL value. Do not use words such as "null","n/a","NULL", or a blank space " " to indicate this. For example:
```commandline
col1,col2,col3,col4
hi there,,that was a great lack of data there,more rambling
         ^----- this indicates a NULL value in col2 column
```

### Editing Tools
- __[SHOULD]__ Use Microsoft Excel __ONLY__ to VIEW a CSV file. Do not use to WRITE or EDIT a CSV file. Common horror story examples are date formats switching to local Date syntax, currency values getting / dropping commas for thousands, leading zeros of codes being dropped because it's assumed to be an integer. Very tempted to make this one a MUST.
- __[SHOULD]__ Use Standard Text editors such as Notepad++ on Windows and Sublime Text on MacOS. When using these tools, make sure to use the show all symbols in order to see the newlines, tabs, and other escaped characters when editing. 