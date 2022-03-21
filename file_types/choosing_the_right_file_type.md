# Choosing the right file type

## TL:DR - We recommend Parquet in almost all cases
__[SHOULD]__ Use Parquet file format for data distribution of > one file of shared schema or total dataset size of > 30MB

## Why? Consider the End User
The best way to approach the question of which file type to use for distributing data is to consider the end use of the files published. While it might seem intuitive to think that CSV is the standard because everyone is familiar with it (often the case), that does not mean that it is the most easily integrated into their internal data architecture where the majority of the testing and assessment is carried out.  

### CSV and JSON
CSV is often requested because of the desire by the end-user to view the contents of the file on a local computer; however, if the dataset size is greater than about 30mb, it becomes cumbersome to use in an Excel or text editor context locally. Adding on various file compression types obligates the end user to have more programs installed on their local computers for extracting data from compressed form. If the publication style of the dataset is such that multiple files constitute a dataset, it's highly unlikely that an end user is going to use a visual method such as Excel or a text editor for working with the entire dataset. 

We most often see CSV formatted datasets being ingested into internal cloud data warehouses or set up in a data lake query engine. There are a few pitfalls that this approach can induce, mainly around the need to auto-sense the delimiting style and set up the parser parameters. Because CSVs are an ASCII (text) format, issues such as quotes around fields, newline characters and multiline strings can significantly degrade the ability of data warehouse systems to automatically set up a dataset for querying. A surprising number of these more exotic CSV formats are simply not supported by major cloud data warehouses, obligating the end user to transform the delivered data using some other more ad-hoc large-scale data system just to work in the organization's mainstay query engine.   

JSON shares a similar set of pitfalls to CSV when considering publishing of data. While JSON is the industry standard for ad-hoc in-memory API communication, when publishing data to files, it often becomes cumbersome to work with. The standard "JSON lines" syntax (see "records" format in the [JSON documentation](json.md)) suffers from a tradeoff between "over-publishing" the field names every line ("fixed") or only publishing the present data on each line ("dynamic"). While the "fixed" field names every row is more deterministic, it also results in significant data duplication, increasing the file size needlessly. The "dynamic" alternative presents challenges to data querying and discovery, as it obligates either a full scan of all rows of the data to determine the union of all query-able fields, or a statistical sampling of rows - which can lead to fields being missed.

Parsing ASCII-based, delimited file formats becomes problematic because it means that the data associated with a specific field must be separated from other field values line-by-line at runtime. For many cloud data warehouses, the parsing is done at time of ingestion, which can be significantly slowed by the need to extract information line-by-line and row-by-row leading to increased costs. For many data lake query engines, the parsing is performed at time of query, obligating the engine to read the entire contents of a row of data just to extract any single field value. As many of these query engines have a cost model based on amount of data scanned, this means very real and unneccessary costs incurred just by using these file formats.  

### Parquet
That leaves us with "the rest" of the file formats out there. Many of them have niche use cases (We have never heard of anyone trying to distribute data in AVRO format). Far and away, the most common and best supported file format of the rest is Parquet. There are many attributes that set Parquet apart that make it very easy to work with in the context of "big data", but a few of the most important for distributing data are:
- __deterministic parsing__: Parquet is a "columnar" file format, which means the column names are explicitly defined and directly tied to the data in that column, meaning there is no uncertainty as to the column name, the data associated with that column, and its data type. 
- __accesible metadata__: Parquet is a binary file format that follows a structure allowing for direct reading of a file's metadata including column names, column data types, and partition information without requiring the reading of the actual data stored in the file.
- __efficient compressibility__: binary format and columnar arrangement mean compression and decompression are very efficient at runtime. 

While you won't be able to open up your dataset in Excel from a Parquet format, making it very easy for a customer to ingest, set up, and work with a dataset of any size out-of-the-box in any data warehouse or data lake query engine in our view outweights that benefit. 


