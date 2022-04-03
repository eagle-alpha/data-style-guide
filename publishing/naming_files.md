[Back to Table of Contents](../README.md)
# Naming files
Some file publication systems (specifically distributed computation engines) don't provide control over the naming of specific files upon export, so if that's the case for your setup, we'd recommend reading up on folder structure conventions to consider [here](../general/folder_structures.md). For everyone else, here's a few tips:
### Portability
- __[SHOULD]__ assume that the consumer might not able to retain the entire folder structure you have created for your data. Therefore, the filename, when combined with the data contained in that file, should be able to reconstruct a significant majority, if not the entirety, of the information denoted by the folder structure.
- __[SHOULD]__ denote the differing column schema if multiple data files exist in the same folder. A common approach to this would be a filename delineation based on the conceptual name of the schema (in database terms, this would be the table name). 
### Formatting
- __[MUST]__ not use spaces in the filename. While some modern file storage systems can handle spaces in filenames, the downstream effects of putting a space in a filename in the context of converting that name to a database or a table will inevitably lead to removing thise
- __[SHOULD]__ use ISO format timestamps & dates only in a pure string (short) format if you are inserting a timestamp or date in the filename. You find find recommendations [here](../data_types/timestamps.md).
- __[SHOULD]__ add the compression type extension to the filename if the file is compressed. 