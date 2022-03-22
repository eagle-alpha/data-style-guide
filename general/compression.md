[Back to Table of Contents](../README.md)
# Compression

### Compression for distributed query engines
- __[SHOULD]__ Compress ALL files prior to distribution using single-file methods such as GZIP or SNAPPY. 
- __[MUST]__ Not use multi-file or folder level compression such as a ZIP archive. Most data warehouses and data lake query engines do not support direct reading of ZIP archives because the file format in the folder cannot be guaranteed and the unpredictability of the decompression action.  