[Back to Table of Contents](../README.md)
# Folder Structures

### Heterogenous Files and Symlinks
Most data warehouses and data lake query engines arrange data "tables" based on the sharing of common directory paths. In the case where files of different types or schemas appear in the same directory, it is often necessary to create a "symlink" to set up these "tables" - essentially a generic lookup list defining data file associations explicitly. Using symlinks significantly slows the computational process for many distributed engines, and should be avoided where possible to allow for quicker computation on your data. 
- __[SHOULD]__ maintain only data files of the same file type in specific directory. Only a select few data warehouses and data lake query engines offer full prefix, suffix, and wildcard filtering on data files in a directory. 
- __[SHOULD]__ maintain only data files relating to the same conceptual "table" within a directory. In practice, this often means maintaining only files of the same schema within a directory.
- __[SHOULD]__ if a conceptual "table" contains data files partitioned under many subdirectories, do not place files of different file type or schema in any subdirectory adjacent to a subdirectory used for partitioning that conceptual table. 

    In practice, this means do this:
    ```
    thisTable/
        |--folderAForThisTable/
            |--file1_schema_A.csv
            |--file2_schema_A.csv
        |--folderBForThisTable/
            |--file3_schema_A.csv
            |--file4_schema_A.csv
    anotherTable/
        |--folderCforAnotherTable/
            |--fileABC_schema_B.txt
    ```
    Instead of this:
    ```
    thisTable/
        |--folderAForThisTable/
            |--file1_schema_A.csv
            |--file2_schema_A.csv
        |--folderBForThisTable/
            |--file3_schema_A.csv
            |--file4_schema_A.csv
        |--folderCforAnotherTable/
            |--fileABC_schema_B.txt
    ```    
- __[SHOULD]__ maintain any documentation in a hierarchically "top-level" directory, separated from the directories used to maintain data files.


### Directories as Partitions
- __[SHOULD]__ Avoid creating a directory structure based on partitioning a high-cardinality column (a column with many unique values). A directory structure should have no more than 100 subdirectories. 
- __[SHOULD]__ Avoid using the primary date as the directory partition unless explicitly requested by an end-user. While it is often given as the example of how to set up partitions on big data, it is rare, aside from dashboard-generated queries, that we see a date-filtered query that harnesses the efficiencies of partitioning by this column. The prime "entity" of the data can be a better alternative, but if this column has a high cardinality, it might not be worth the decrease in human-readability of the directory structure. 
- __[SHOULD]__ Avoid using highly nested directory structures as a way to describe a conceptual "table" in your dataset. One to two levels deep is often all that is needed to separate data intuitively, as it mimics the database>table or database>schema>table structure of nearly every major database catalog. Describe data "tables" in metadata standards such as data dictionaries instead of using the directory structure for this purpose. 