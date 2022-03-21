# Parquet
There's honestly not a lot to say about Parquet other than to please use it as much as possible. More information about the most effective partitioning strategies for the time being can be found elsewhere and will be linked to here in the future. Please read the argument for [why you should use Parquet](choosing_the_right_file_type.md).

### Data Types
- __[SHOULD]__: Use the Parquet data types instead of saving off a Parquet file as a bunch of strings only. Most data warehouses and query engines support the reading of these data types. 
