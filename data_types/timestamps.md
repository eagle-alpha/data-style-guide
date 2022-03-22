[Back to Table of Contents](../README.md)
# Timestamps

### Format to use for representing Timestamps and Dates
- __[MUST]__ Use ISO 8601 standard formats for timestamps and dates. ISO standards are globally recognized, and many data warehouses, data lake query engines, and file formats natively use ISO standards. One of the many convenient attributes of ISO timestamp standard is that it is lexicographically sortable, meaning if you name a directory with an ISO timestamp or date, you can sort the directories in time-order using standard file explorers. You can find a great summary of the formats [here](https://en.wikipedia.org/wiki/ISO_8601)
    
    ISO Standard Timestamp format (for UTC timezone):
    ```
  YYYY-MM-DDThh:mm:ss.sss 
  2021-03-02T17:33:24.379
  
  YYYY-MM-DDThh:mm:ss.sssZ 
  2021-03-02T17:33:24.379Z
  
  YYYY-MM-DDThh:mm:ss.sss UTC
  2021-03-02T17:33:24.379 UTC
  
  Short forms:
  YYYYMMDDThhmmss.sss 
  20210302T173324.379
  
  YYYYMMDDThhmmss 
  20210302T173324
  ```
  ISO Standard Calendar Date format:
    ```
  YYYY-MM-DD
  2021-03-02
  ```
- __[SHOULD]__ Keep all timestamps in UTC timezone, which is the operating assumption of most data consumers (even though ISO says to assume local time). It is not necessary to add the Z or UTC to the end of a timestamp and some major data warehouse and data lake query engines do not actually support timezones in their timestamps.
- __[SHOULD]__ Save information about the local timezone of the event in another column separated from the timestamp, which should be saved in UTC time. 
- __[SHOULD]__ Use the Date granularity instead of the Timestamp ONLY if the data truly represents the summation of an event at the day level. OTHERWISE use the full timestamp and assume the "zero'th" time dimension value (or minimum) for the levels of granularity beyond which the data is not captured. For example, if the data is the summation of an event to the hour level, use the below syntax:
  ```
  YYYY-MM-DDThh:00:00.000 
  2021-03-02T17:00:00.000
  ```
### Publication timestamps
- __[SHOULD]__ As most data publications occur by adding new data files to a data repository, the data filename should include the publication timestamp for that file (shared among all rows of the data in that file). The ISO shortform is often used for this due to lack of symbol support in most filenames. Most data lake storage systems will provide a modified time of the data file in its metadata, however, this is often lost or overwritten in the transfer of data to customers, so it is best to retain it in the filename additionally. 
- __[SHOULD]__ be careful to delineate between the publication timestamp and any other prime timestamps in the data. For example, make it clear that the data capturing the event that occurred on 2021-03-02 (the event time) was published on 2021-03-05 (the publication time) by separating out the publication timestamp from the event timestamp into distinct columns in the data.  