---
weight: 30
title: "Working example (with Data Pipeline API functionality)"
---

<span style="font-size:12pt; color:red">Note that this is a living document and the following is subject to change. For reference, my R code lives [here](https://github.com/ScottishCovidResponse/SCRCdataAPI/tree/implement_yaml). Please post any questions on Zulip.</span>

# Working example (with Data Pipeline API functionality)

The following example downloads some data from outside the pipeline, does some processing in R (for example), and records the original file and the resultant data product into the pipeline.

In this simple example, the user should run the following from the terminal:

```bash
fdp pull config.yaml
fdp run config.yaml
fdp push config.yaml
```

These functions require a *config.yaml* file to be supplied by the user. This file should specify various metadata associated with the code run, including where external objects comes from and the aliases that will be used in the submission script, data objects to be read and written, and the submission scipt location.

## User written *config.yaml*

```yaml
run_metadata:
  description: Register a file in the pipeline
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: soniamitchell
  default_data_store: /Users/SoniaM/datastore/
  local_repo: /Users/Soniam/Desktop/git/SCRC/SCRCdata
  script: |- 
    R -f inst/SCRC/scotgov_management/submission_script.R {CONFIG_DIR}
register:
- external_object: management-data
  source_name: Scottish Government Open Data Repository
  source_abbreviation: Scottish Government Open Data Repository
  source_website: https://statistics.gov.scot/
  root_name: Scottish Government Open Data Repository
  root: https://statistics.gov.scot/sparql.csv?query=
  path: |
    PREFIX qb: <http://purl.org/linked-data/cube#>
    PREFIX data: <http://statistics.gov.scot/data/>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX mp: <http://statistics.gov.scot/def/measure-properties/>
    PREFIX dim: <http://purl.org/linked-data/sdmx/2009/dimension#>
    PREFIX sdim: <http://statistics.gov.scot/def/dimension/>
    PREFIX stat: <http://statistics.data.gov.uk/def/statistical-entity#>
    SELECT ?featurecode ?featurename ?date ?measure ?variable ?count
    WHERE {
      ?indicator qb:dataSet data:coronavirus-covid-19-management-information;
                  dim:refArea ?featurecode;
                  dim:refPeriod ?period;
                  sdim:variable ?varname;
                  qb:measureType ?type.
      {?indicator mp:count ?count.} UNION {?indicator mp:ratio ?count.}

      ?featurecode <http://publishmydata.com/def/ontology/foi/displayName> ?featurename.
      ?period rdfs:label ?date.
      ?varname rdfs:label ?variable.
      ?type rdfs:label ?measure.
    }
  title: Data associated with COVID-19
  description: The data provide past data around COVID-19 for the daily updates provided by the Scottish Government.
  unique_name: COVID-19 management information
  product_name: records/SARS-CoV-2/scotland/cases-and-management
  file_type: csv
  release_date: {DATETIME}
  version: 0.{DATETIME}.0
  primary: True
  accessibility: open

write:
- data_product: records/SARS-CoV-2/scotland/cases-and-management/ambulance
  description: Ambulance data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/calls
  description: Calls data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/carehomes
  description: Care homes data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/hospital
  description: Hospital data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/mortality
  description: Mortality data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/nhsworkforce
  description: NHS workforce data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/schools
  description: Schools data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/testing
  description: Testing data
  version: 0.{DATETIME}.0
```

## Working *config.yaml*

`fdp run` should create a working *config.yaml* file, which is then read by the Data Pipeline API.

```yaml
run_metadata:
  description: Register a file in the pipeline
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: soniamitchell
  default_data_store: /Users/SoniaM/datastore/
  local_repo: /Users/Soniam/Desktop/git/SCRC/SCRCdata
  script: R -f inst/SCRC/scotgov_management/submission_script.R /Users/SoniaM/datastore/coderun/20210511-231444/
read:
- external_object: management-data
  doi_or_unique_name: COVID-19 management information
  title: Data associated with COVID-19
  version: 0.20210414.0
write:
- data_product: records/SARS-CoV-2/scotland/cases-and-management/ambulance
  description: Ambulance data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/calls
  description: Calls data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/carehomes
  description: Care homes data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/hospital
  description: Hospital data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/mortality
  description: Mortality data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/nhsworkforce
  description: NHS workforce data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/schools
  description: Schools data
  version: 0.{DATETIME}.0
- data_product: records/SARS-CoV-2/scotland/cases-and-management/testing
  description: Testing data
  version: 0.{DATETIME}.0
```

## *submission_script.R*

A submission script should be supplied by the user, which in this case registers an external object, reads it in, and then writes it back to the pipeline as a data product component. In the above example, this script is located in *<local_repo>/inst/SCRC/scotgov_management/submission_script.R*.

```R
library(SCRCdataAPI)

# Open the connection to the local registry with a given config file
# You can put in a file if you really want to, but otherwise read from 
# the environment directly or from a command line argument
handle <- initialise(Sys.getenv("FDP_CONFIG_DIR"))

# Return location of file stored in the pipeline
input_path <- link_read(handle, "raw-mortality-data")

# Process raw data and write data product
data <- read.csv(input_path)
array <- some_processing(data)
index <- write_array(array, 
                     handle, 
                     data_product = "human/mortality", 
                     component = "mortality_data",
                     dimension_names = list(location = rownames(array),
                                            date = colnames(array)))
issue_with_component(index,
                     handle,
                     issue = "this data is bad",
                     severity = 7)

finalise(handle)
```

### `initialise()`

- read the working *config.yaml* file
- return a `handle` containing:
  - the working *config.yaml* file contents
  - the object id for this file
  - the object id for the submission script file

### `link_read()`

- this function returns the path of an external object in the local data store
- if the alias is already recorded in the handle, return the path
- if the alias is not recorded in the handle, find the location of the file referenced by its `alias`
  - in the above example the alias is `management-data`
  - note that the alias is not recorded in the data registry, rather, it's a means to reference external objects in the *config.yaml*)
- store metadata associated with the external object

### `read_array()`

- responsible for reading the correct data product, which at this point has been downloaded from the remote data store by `fdp pull`
- should by default read the latest version of the data, which at this point has been downloaded from the remote data store by `fdp pull`

### `link_write()`

- when writing external objects, we use `link_read()` and `link_write()` to read and write objects, rather than the standard API `read_xxx()` and `write_xxx()` calls.

### `write_array()`

- responsible for writing an array as a component to an hdf5 file
- should allocate the correct data product name to the data (*e.g.* for *human/outbreak/simulation_run-{RUN_ID}*, `{RUN_ID}` is replaced with an appropriate index)
- should by default increment the data product version by PATCH if none is specified
- if the **component** is already recorded in the handle, return the index of this handle reference invisibly
- otherwise:
  - if this is the first component to be written, record the save location in the handle, conversely, if this is not the first component to be written, reference the save location from the handle
  - write the component to the hdf5 file
  - determine the correct version number to be associated with the new data product
  - update the `handle` with the component that was written and its location in the data store

### `issue_with_component()`

- **this is very much a work in progress**
- find the input or output reference (in the handle) that the issue is associated with
- note that issues can also be associated with scripts, etc. (I've not gone near this yet)
- record issue metadata in the handle
- NOTE that in the above example, `issue_with_component()` takes an `index` that references an object recorded in the handle, alternatively it may take a `dataproduct`, `component`, and `version` as identifiers
- NOTE that `issue_with_component()` is but one of a series of functions including `inssue_with_dataproduct()`, `issue_with_externalobject()`, and `issue_with_script()`; it might make more sense for you to write a generic `raise_issue()` function depending on language constraints

### `finalise()`

- rename the data product as *<hash>.h5*
- record data product metadata (*e.g.* location, components, various descriptions, issues) in the data registry
- record the code run in the data registry

## *submission_script.py*

Alternatively, the submission script may be written in Python.

```python
from data_pipeline_api.standard_api import StandardAPI

with StandardAPI.from_config("config.yaml") as api:
  matrix = read(api.link_read("raw-mortality-data"))
  api.write_array("human/mortality", "mortality_data", matrix)
  api.issue_with_component("human/mortality", "mortality_data", "this data is bad", "7")
  api.finalise()
```

## *submission_script.jl*

Alternatively, the submission script may be written in Julia.

```julia
using DataPipeline

# Open the connection to the local registry with a given config file
handle = initialise("config.yaml") 

# Return location of file stored in the pipeline
input_path = link_read(handle)

# Process raw data and write data product
data = read_csv(input_path)
array = some_processing(data)
index = write_estimate(array, 
                       handle, 
                       data_product = "human/mortality", 
                       component = "mortality_data")

issue_with_component(index,
                     handle,
                     issue = "this data is bad",
                     severity = 7)

finalise(handle)
```

## C++

## Java