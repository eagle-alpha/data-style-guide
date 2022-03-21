# JSON

### Common Dictionary Structures
A list of common JSON dictionary styles can be found in the [Pandas to_dict](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_dict.html) documentation, but are reproduced here as well:
- ‘dict’ : dict like {column -> {index -> value}}

- ‘list’ : dict like {column -> [values]}

- ‘series’ : dict like {column -> Series(values)}

- ‘split’ : dict like {‘index’ -> [index], ‘columns’ -> [columns], ‘data’ -> [values]}

- ‘tight’ : dict like {‘index’ -> [index], ‘columns’ -> [columns], ‘data’ -> [values], ‘index_names’ -> [index.names], ‘column_names’ -> [column.names]}

- ‘records’ : list like [{column -> value}, … , {column -> value}]

- ‘index’ : dict like {index -> {column -> value}}