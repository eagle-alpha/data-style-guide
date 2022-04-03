[Back to Table of Contents](../README.md)
# Full Refresh Publications
"Full Refresh" publications are often used for datasets that have significant historical restatements or are produced using models that are updated at a high frequency in relation to the data publication frequency. 

### Don't Confuse the Consumer: Publish Full Refresh Data with Care 
- __[SHOULD]__ not delete or overwrite previous publications that were made point-in-time when publishing full refresh data. 
- __[SHOULD]__ set a schedule for how often full refresh publications are created and define its relationship to "delta" or higher frequency incremental publications. 
### Model Metadata
- __[SHOULD]__ publish model information as metadata with any publication structure where the underlying training data is updated on a frequency of an order of magnitude of the data publication frequency. 
- __[SHOULD]__ publish model information metadata in a separated "join" table instead of the primary data file publication.
- __[SHOULD]__ publish model training data information, specifically the collection time of the training data used for specific output data publications. 
### Relationship to "Delta" Publications
- __[SHOULD]__ provide any aggregation methods used on the "delta" publication data in order to reproduce the full refresh publication data from the "delta" publications. 
- __[SHOULD]__ not use different file formats or column schemas between the delta publications and the full refresh publications. 