[Back to Table of Contents](../README.md)
# Column Names

### How to name a column
- __[SHOULD]__ Not use spaces in a column name, even if quoted. Data warehouses and query engines end up requiring quotes around any references to that column in SQL, so it just creates needless extra work for the customer to always have to quote a column reference. If a delineation is needed, use an underscore ( _ )
- __[SHOULD]__ Use only lowercase letters in column names. Many (but importantly, not all) data warehouses and data lake query engines are case-insensitive, so to avoid a reference nightmare where certain pieces of your ETL pipeline need the specification to be the Title_Case name of your column and other parts are agnostic to case, keep things lowercase!
- __[SHOULD]__ Not start a column name with a number. 
- __[SHOULD]__ Limit the character set in a column name to only alphabetical, numerical, and the underscore ( _ ). Do not use other symbols or punctuation in column names includes percent signs ( % ), dashes ( - ), and periods ( . )
- __[SHOULD]__ If representing a field with a hierarchical relationship (such as flattened nesting), represent the structure in the column name by combining the field names at each level with two underscores ( __ ). For example, field name "level_1", with field "level_2" nested beneath it, and "level_3" nested beneath it would become "level_1__level_2__level_3". Limit the other uses of the double underscore in column names in your dataset if needing to represent flattened nesting in this way. 