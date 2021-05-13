---
weight: 20
title: "DP API Example 1"
---

# Data Pipeline API Example 1

## *config.yaml*

`fdp pull` and `fdp run` require a *config.yaml* file to be supplied by the user.

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
    R -f inst/SCRC/scotgov_management/submission_script.R {CONFIG_PATH}
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
  release_date: {RELEASE_DATE}
  version: {VERSION_DATE}
  primary: True
  accessibility: open

write:
- data_product: records/SARS-CoV-2/scotland/cases-and-management/ambulance
  description: Ambulance data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/calls
  description: Calls data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/carehomes
  description: Care homes data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/hospital
  description: Hospital data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/mortality
  description: Mortality data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/nhsworkforce
  description: NHS workforce data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/schools
  description: Schools data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/testing
  description: Testing data
  version: {VERSION_DATE}

```

## Working *config.yaml*

`fdp run` should create a working *config.yaml* file, which is read by the Data Pipeline API.

```yaml
run_metadata:
  description: Register a file in the pipeline
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: soniamitchell
  default_data_store: /Users/SoniaM/datastore/
  local_repo: /Users/Soniam/Desktop/git/SCRC/SCRCdata
  script: R -f inst/SCRC/scotgov_management/submission_script.R /Users/SoniaM/datastore/coderun/20210511-231444/config.yaml
read:
- external_object: management-data
  doi_or_unique_name: COVID-19 management information
  title: Data associated with COVID-19
  version: 0.20210414.0
write:
- data_product: records/SARS-CoV-2/scotland/cases-and-management/ambulance
  description: Ambulance data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/calls
  description: Calls data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/carehomes
  description: Care homes data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/hospital
  description: Hospital data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/mortality
  description: Mortality data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/nhsworkforce
  description: NHS workforce data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/schools
  description: Schools data
  version: {VERSION_DATE}
- data_product: records/SARS-CoV-2/scotland/cases-and-management/testing
  description: Testing data
  version: {VERSION_DATE}
```

## *submission_script.R*

A submission script should be supplied by the user. In the above example, the location of this script is referenced in `run_metadata: script:`.

```R
library(SCRCdataAPI)

# Open the connection to the local registry with a given config file
# You can put in a file if you really want to, but otherwise read from 
# the environment directly or from a command line argument
handle <- initialise(Sys.getenv("FDP_CONFIG_DIR"))

# Return location of file stored in the pipeline
input_path <- read_link(handle, "raw-mortality-data")

# Process raw data and write data product
data <- read.csv(input_path)
array <- some_processing(data)
index <- write_array(array, 
                     handle, 
                     data_product = "data_product_name", 
                     component = "component",
                     dimension_names = list(location = rownames(array),
                                            date = colnames(array)))
issue_with_component(index,
                     handle,
                     issue,
                     severity)

finalise(h)
```

## `initialise()`

- read the working *config.yaml* file
- return a `handle` containing:
  - the working *config.yaml* file contents
  - the object id for this file
  - the object id for the submission script file

## `read_link()`

- this function returns the path of an external object in the local data store
- if the alias is already recorded in the handle, return the path
- if the alias is not recorded in the handle, find the location of the file referenced by its `alias`
  - in the above example the alias is `management-data`
  - note that the alias is not recorded in the data registry, rather, it's a means to reference external objects in the *config.yaml*)
- store metadata associated with the external object

## `write_array()`

- write an array as a component to an hdf5 file
- if the component is already recorded in the handle, return the index of this handle reference invisibly
- otherwise
  - if this is the first component to be written set a save location, otherwise reference the save location from the handle
  - write the component to the hdf5 file
  - determine the correct version number to be associated with the new data product
  - update the `handle` with the component that was written and its location in the data store

## `issue_with_component()`

**this is very much a work in progress**
- find the input or output reference (in the handle) that the issue is associated with
- note that issues can also be associated with scripts, etc. (I've not gone near this yet)
- record issue metadata in the handle
- NOTE that in the above example, `issue_with_component()` takes an `index` that references an object recorded in the handle, alternatively it may take a `dataproduct`, `component`, and `version` as identifiers
- NOTE that `issue_with_component()` is but one of a series of functions including `inssue_with_dataproduct()`, `issue_with_externalobject()`, and `issue_with_script()`; it might make more sense for you to write a generic `raise_issue()` function depending on language constraints

## `finalise()`

- rename the data product as *<hash>.h5*
- record data product metadata (*e.g.* location, components, various descriptions, issues) in the data registry
- record the code run in the data registry
