[Back to Table of Contents](../README.md)
# Delta Files
"Delta" publications are a method of publishing ongoing data products where only new datapoints are published, usually corresponding to new events in time, but also potentially updates to events that were previously published (but not a restatement or overwrite action).

### Publishing
- __[SHOULD]__ always publish in the data a unique identifier of an event or record, especially if subsequently published deltas reference a previously published event. This helps to construct "the full story" about an event from the various deltas. 
- __[SHOULD]__ Not publish duplicated information to what is previously published. Many data warehouses and data lake query engines would not automatically exclude duplications. 
- __[SHOULD]__ Not use delta publications for restatements of previously published information. See [Full Refresh](full_refresh.md) publications for this. 
- __[SHOULD]__ Name the delta data file with at least the publication timestamp for the delta. It is also worth considering naming to delta files to always correspond to data published in specific starting files. 

