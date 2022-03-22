[Back to Table of Contents](../README.md)
# JSON

### Text Editors and JSON Standards
- __[SHOULD]__ Use a text editor such as Notepad++ on Windows or Sublime Text on MacOS with the JSON syntax highlighting enabled to view and edit JSON data files. 
- __[SHOULD]__ Read the wealth of JSON syntax standards documentation out there instead of relying on this document for the specific do's and don'ts of valid JSON. 

### Nesting and discoverability
JSON is an extremely generic data language type based around lists and dictionaries that can be endlessly nested if desired. You should not desire that though!
- __[SHOULD]__ Keep the prime data structure in the data file minimally nested. While many modern data warehouses can query highly nested data structures, successive lists of dictionaries of lists of dictionaries makes it very challenging from an approachability and discoverability of content standpoint. For example:
    ```json
    [{
      "field_A":
          {
            "value": "yes"
          },
      "field_B":
          {
            "first_name": "Eagle",
            "last_name": "Alpha"
          }
    }]
    ```
    Can be easily flattened for better functional compatibility to:
    ```json
    [{
      "field_A__value": "yes",
      "field_B__first_name": "Eagle",
      "field_B__last_name": "Alpha"
    }]
    ```
- __[SHOULD]__ If you must put a list of things inside the value of a field, limit it to a list of strings, integers, or floats instead of further nesting such as dictionaries. If you find yourself really needing nested lists of dictionaries, consider separating this element out to a join table with join keys. 

    This is okay:
    ```json
    [{
      "field_C_values": [1,3,9,4,5,3]
    }]
    ```
    While this is too much nesting for most people:
    ```json
    [{
      "field_C_values": 
        [
          {
            "first_name": "Eagle",
            "last_name": "Alpha"
          },
          {
            "first_name": "Pigeon",
            "last_name": "Beta"
          }
        ]
    }]
    ```
- __[MUST]__ If you don't want to heed our advice and really like highly nested lists of dictionaries, you must retain an ordering index inside the list of dictionaries. Many distributed data warehouses do not guarantee order retention, therefore it is advised to make a field available that allows for explicit sorting instead of relying on implicit sort order retention. For example, add it like this and it won't matter what order your list of dictionaries shows up in:
    ```json
    [{
      "field_C_values": 
        [
          {
            "index_val": 0,
            "first_name": "Eagle",
            "last_name": "Alpha"
          },
          {
            "index_val": 3,
            "first_name": "Pigeon",
            "last_name": "Beta"
          },
          {
            "index_val": 1,
            "first_name": "Bloom",
            "last_name": "Berger"
          },
          {
            "index_val": 2,
            "first_name": "Factoid",
            "last_name": "Set"
          }
  
        ]
    }]
    ```

### JSON Lines and Records-oriented Valid JSON Syntax

- __[SHOULD]__ Use JSON lines syntax for all record-based data publications as it is easier for distributed data architectures to read. 

    "JSON lines" is the most common style we see for "record"-based data publication, however it is ironically not valid JSON. The closest valid JSON syntax to it is the "records" orient syntax of the [Pandas to_dict](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_dict.html) method, which follows a syntax similar to the above nested list of dictionaries:
    ```json
    [
        {
            "index_val": 0,
            "first_name": "Eagle",
            "last_name": "Alpha"
        },
        {
            "index_val": 3,
            "first_name": "Pigeon",
            "last_name": "Beta"
        },
        {
            "index_val": 1,
            "first_name": "Bloom",
            "last_name": "Berger"
        },
        {
            "index_val": 2,
            "first_name": "Factoid",
            "last_name": "Set"
        }
    ]
    ```
    JSON lines syntax essentially removes the "list" component of the above syntax to allow for individual record retrieval without having to load in the entire file. Instead of commas separating the dictionaries in the list, the newline character is used:
    ```json lines
    {"index_val": 0,"first_name": "Eagle","last_name": "Alpha"}
    {"index_val": 3,"first_name": "Pigeon","last_name": "Beta"}
    {"index_val": 1,"first_name": "Bloom","last_name": "Berger"}
    {"index_val": 2,"first_name": "Factoid","last_name": "Set"}
    ```
  
### Dynamic vs. Fixed Fields
- __[SHOULD]__ Fix the fields for every record to be the union of all non-null valued fields from any record in the dataset. Mark as __null__ any fields that are non-valued for that specific record. The cost of a larger-sized dataset is worth it when balanced with the lack of discoverability of the data structure if the full set of fields cannot be easily ascertained by reading a small sample of the data. This union of all fields operation is non-deterministic in the case where different data files are truthfully supposed to represent different tables that should not be combined. ß