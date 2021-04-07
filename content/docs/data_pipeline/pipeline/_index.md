---
weight: 2
title: "Data pipeline"
---

# SCRC data pipeline

The data pipeline is moving towards this new structure:

![SCRC Data API design figure](scrc-api-new.svg)

The API is accessed as a *session*, which corresponds in the registry to a *code run*. All reads and writes are logged directly to a local registry as the session progresses, as is the script which generates that i/o. Files are identified by their metadata, though the metadata is different for reads (where the files must exist) and writes (where they must not). The metadata is also different for *files in core pipeline data formats*, where a significant amounts of metadata are recorded, and *other arbitrary files*, where only a limited amount of data can be collected. Either type of file can be used for input or output, giving a total of four different interactions, two for input and two for output. These differences are described in more detail below.

The underlying data to which the API refers is determined by the interaction between a yaml configuration file (referred to here as a `config.yaml` file, and described below) provided at initialisation and the state of the remote registry at the time of processing of the `config.yaml` by the download synchronisation script (which is considered to contain the definitive version of all data at that time). The specific remote registry used is itself defined in the `config.yaml` file.

This interaction between the configuration file and the remote registry defines the “*local filesystem data repository*” that the local pipeline interacts with. The data directory can be automatically created by a *download synchronisation script* [(currently found here)](https://github.com/ScottishCovidResponse/data_pipeline_api/tree/master/data_pipeline_api/registry) which reads the `config.yaml` file, queries the appropriate remote registry, downloads appropriate data, and populates the local registry with the relevant metadata for those data.

When a model or script is run (as a *session* / “*code run*”), any output files are written to the data directory, and those outputs are logged in the local registry, which has itself been created (or updated) by the *download synchronisation script*. The local registry can be queried to determine whether the data generated is as intended, and if so it can then by synchronised back to the remote registry. This can be carried out automatically using an *upload synchronisation script* [(currently here)](https://github.com/ScottishCovidResponse/data_pipeline_api/tree/master/data_pipeline_api/registry). When the *session* is initialised a “*run id*” is created to uniquely identify that *code run*. It is constructed by forming the SHA1 hash of the configuration file content, plus the date time string.

## Dictionary of terms

*external object*: an object that is not in an internal pipeline format (toml or hdf5) - thus, external... 

*data product*: what we access with the standard API read_xxx(), write_xxx() calls. External objects we tell the pipeline we are going to read using record_read() and record_write(), but the reading and writing happens in your code
## config.yaml file format

The config file lets users specify metadata to be used during file lookup for read or write, and configure overall API behaviour.

### Example: simple inputs and outputs

```yaml
fail_on_hash_mismatch: True
run_metadata:
  description: A simple analysis
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: johnsmith
  default_data_store: ~/datastore/
  always_copy_to_store: False

read:
# Read version 1.0 of human/commutes
- data_product: human/commutes
  use:
    version: 1.0
# Read human/health from the cache
- data_product: human/health
  use:
    cache: ~/local/file.h5
# Read crummy_table with specific doi and title
- external_object: crummy_table
  use:
    doi: 10.1111/ddi.12887
    title: Supplementary Table 2
# Read secret_data with specific doi and title from the cache
- external_object: secret_data
  use:
    doi: 10.1111/ddi.12887
    title: Supplementary Table 3
    cache: ~/local/secret.csv
# Read weird_lost_file (which perhaps has no metadata) with specific hash
- object: weird_lost_file
  use:
    hash: b5a514810b4cb6dc795848464572771f

write:
# Write beautiful_figure and increment version number
- external_object: beautiful_figure
  use:
    unique_name: My amazing figure
    version: minor
```

- `fail_on_hash_mismatch:` will, if set to True (the default), cause the API to fail is an attempt is made to read a file whose computed hash differs from the one stored in the local registry

- `run_metadata` provides metadata for the run:
  - `description:` is a human readable description of the purpose of the config.yaml
  - `local_data_registry_url:` specifies the local data registry root, which defaults to https<!-- -->://localhost:8000/api/
  - `remote_data_registry_url:` specifies the remote data registry endpoint, which defaults to https<!-- -->://data.scrc.uk/api/
  - `default_input_namespace:` and `default_output_namespace:` specify the default namespace for reading and writing
  - `default_data_store:` specifies the file system root used for data writes, which defaults to ~/datastore (it may be relative, in which case it is relative to the directory containing the config file)
  - `always_copy_to_store` specifies whether files that already exist in the local filesystem (files specified in `read: use: cache:`) but not in the `default_data_store` should be copied to the data store (set to `True`) or not (set to `False`, default)
  - Additional fields exist (`remote_repo:`, `local_repo:`, `script_path:`, and `script:`), which will be described later. Any other fields will be ignored.
  
The `data_product:` (within `read:` and `write:`), `external_object:` (`read:` and `write:`) and `object:` (`read:` only) sections specify metadata subsets that are matched in the read and write processes. The metadata values may use glob syntax, in which case matching is done against the glob. The corresponding `use:` sections contain metadata that is used to update the call metadata before the file access is attempted. For reads, a `cache:` may be specified directly, in which case it will be used without any further lookup.

If a write is carried out to a data product where no such `data_product:` entry exists, then a new data product is created with that name in the local namespace, or the patch version of an existing data product is suitably incremented. The level of incrementation or version number can be explicitly defined by `version:`. If a write is carried out to an object that is not a data product and no such `external_object:` entry exists, then a new object is created with no associated external object or data product, and an issue is raised with the object to note the absence of an appropriate reference, referencing the name given in the write API call.

<span style="font-size:14pt; color:red">Note that: at the moment, we haven't ensured that any write will already have its objects synced to the local registry, so we may not know what the current version is. Either we need to say that you have to add writes to the config.yaml file, or you need to allow a special version called "patch", "minor" and "major" in the local registry (or "+0.0.1", "+0.1", "+1", so that you can then allow "+2", "+2.0.1", etc.).</span>

### Example: flexibile inputs and outputs

The following example describes an analysis which typically reads `human/population` and writes `human/outbreak-timeseries`. Instead, a test model is run using Scottish data, whereby `scotland/human/population` is read from the `eera` namespace, rather than `human/population`. Likewise, the output is written as `scotland/human/outbreak-timeseries` rather than `human/outbreak-timeseries`.

```yaml
fail_on_hash_mismatch: True
run_metadata:
  description: A test model
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: johnsmith
  default_data_store: ~/datastore/
  always_copy_to_store: False

read:
- data_product: human/population
  use:
    namespace: eera
    data_product: scotland/human/population

write:
- data_product: human/outbreak-timeseries
  use:
    data_product: scotland/human/outbreak-timeseries
- data_product: human/outbreak/simulation_run
  use:
    data_product: human/outbreak/simulation_run-{run_id}
```

Any part of a `use:` statement may contain the string `{run_id}`, which will be replaced with the run id.
- `run_id` specifies the run id to be used, otherwise a hash of the config contents and the date will be used.

## Example use of the pipeline

### Register a new external object

To get the pipeline up and running, we need to add some data. To do this we should download some data from outside the pipeline, do some processing in Python (for example), and record the original file and the resultant data product into the pipeline.

To achieve this, run the following from the command line:

```bash
tdp run config.yaml
```

That is, assuming a `config.yaml` file already exists. The yaml file, should specify where the external object comes from and the aliases that will be used in the submission script:

```yaml
run_metadata:
  description: Register a file in the pipeline
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: johnsmith
  default_data_store: ~/datastore/
  local_repo: ~/Users/johnsmith/git/myproject/
  script: # Points to the Python script, below (relative to local_repo)
    python -f path/submission_script.py {CONFIG_PATH}

register:
- external_object: raw-mortality-data
  source: # Maybe existing sources only require `name:` to be populated?
    name: Scottish Government Open Data Repository
    abbreviation: Scottish Government Open Data Repository
    website: https://statistics.gov.scot/
  downloaded_from:
    name: Scottish Government Open Data Repository
    root: https://statistics.gov.scot/sparql.csv?query=
    path: |
      PREFIX qb: <http://purl.org/linked-data/cube#>
      PREFIX data: <http://statistics.gov.scot/data/>
      PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
      PREFIX dim: <http://purl.org/linked-data/sdmx/2009/dimension#>
      PREFIX sdim: <http://statistics.gov.scot/def/dimension/>
      PREFIX stat: <http://statistics.data.gov.uk/def/statistical-entity#>
      PREFIX mp: <http://statistics.gov.scot/def/measure-properties/>
      SELECT ?featurecode ?featurename ?areatypename ?date ?cause ?location ?gender ?age ?type ?count
      WHERE {
       ?indicator qb:dataSet data:deaths-involving-coronavirus-covid-19;
         mp:count ?count;
         qb:measureType ?measType;
         sdim:age ?value;
         sdim:causeOfDeath ?causeDeath;
         sdim:locationOfDeath ?locDeath;
         sdim:sex ?sex;
         dim:refArea ?featurecode;
         dim:refPeriod ?period.

         ?measType rdfs:label ?type.
         ?value rdfs:label ?age.
         ?causeDeath rdfs:label ?cause.
         ?locDeath rdfs:label ?location.
         ?sex rdfs:label ?gender.
         ?featurecode stat:code ?areatype;
           rdfs:label ?featurename.
         ?areatype rdfs:label ?areatypename.
         ?period rdfs:label ?date.
      }
    accessibility: open        # Default, so not necessary
  metadata:
    unique_name: scottish deaths-involving-coronavirus-covid-19
    product_name: records/SARS-CoV-2/scotland/human-mortality
    title: scottish deaths-involving-coronavirus-covid-19
    primary: True
    release_date: 2021-04-01   # Assuming this is today
    description: scottish deaths-involving-coronavirus-covid-19 dataset
    version: 0.20210401.0      # Likewise
    accessibility: open
    
write:
- data_product: human/mortality
  component: mortality_data    # This is a rubbish name
```

Then the python script:

```python
from data_pipeline_api.standard_api import StandardAPI

with StandardAPI.from_config("config.yaml") as api:
    api.register("raw-mortality-data")
    matrix = read(api.record_open("raw-mortality-data"))
    api.write_array("human/mortality", "mortality_data", matrix)
```

This registers an external object, reads it in, and then writes it back to the pipeline as a data product component.

### Read and write an external object

A script to read and write an external object (*i.e.* something not in a core data pipeline format) in R. First, the yaml file, that gives the `doi_or_unique_name` and `title` of the external objects being read and written, and the aliases that will be used in the submission script:

```yaml
run_metadata: 
  description: A simple example using external objects
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC 
  default_output_namespace: johnsmith
  default_data_store: ~/datastore/
  local_repo: ~/Users/johnsmith/git/myproject/
  script: | # Points to the R script, below (relative to local_repo)
    R -f path/submission_script.R {CONFIG_PATH}

read: 
- external_object: "time-series"
  use:
    unique_name: "An exciting time series"
    title: "Table 1"

register: # Need to think about this a bit more
- external_object: "revised-time-series"
  unique_name: "An new, revised, time series"
  title: "Table 1"
  primary: True
```

Then the R script reads in the original data, modifies it and saves it to disk as a csv file, registering it in the local registry as an external object:

```R
library(DataPipeline)
library(readr)
library(dplyr)

# Open the connection to the local registry with a given config file
handle <- initialise("config.yaml") # How to reference the active script?

# Read in a csv file stored in the pipeline
time_series <- read_csv(pipeline_record_open(handle, "time-series"))

new_time_series <- time_series %>% select(time, pipes) %>% filter(time < 10)

write_csv(new_time_series,
          pipeline_record_write(handle, "revised-time-series"))

finalise(handle)
```

Since we're now working with external objects, we use `pipeline_record_open()` and `pipeline_record_write()` to read and write objects, rather than the standard API `read_xxx()` and `write_xxx()` calls.
### Read then write a data product component

Now that the pipeline is populated, one of the simplest possible use cases is just to read in a value, calculate a new value from it, and write out the new value. Again, we need to write a `config.yaml` file:

```yaml
run_metadata: 
  description: A simple example using data products
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC 
  default_output_namespace: johnsmith
  default_data_store: ~/datastore/
  local_repo: ~/Users/johnsmith/git/myproject/
  script: | # Points to the Julia script, below (relative to local_repo)
    Julia -f path/submission_script.jl {CONFIG_PATH}

read: 
- data_product: human/infection/SARS-CoV-2

write:
- data_product: human/infection/SARS-CoV-2/doubled
  component: doubled-infectious-period
```

Here, a submission script is stored in `local_repo:` (the root of a local repository). Alternatively, a `remote_repo:` can be provided, corresponding to the root of a remote repository. Note that only one of these is necessary. If `local_repo` is omitted, a clone of `remote_repo` will be created by `initialise()`. If `local_repo` is included, `initialise()` will check whether or not the local repository is clean. The submission script itself (usually, a single line of code that points to the processing / analysis script) should either be written in `script` or stored in a text file in `script_path`, which can be absolute or relative to the root of the repo.

The download sync using this yaml file will look in the `default_input_namespace` of the remote registry (`remote_data_registry_url:`) for the latest version of the `human/infection/SARS-CoV-2` data product and download it, syncing all of the associated metadata into the local registry.

The Julia script (found in `local_repo`/path/submission_script.jl) will do the work:

```julia
using DataPipeline

# Open the connection to the local registry with a given config file
handle = initialise("config.yaml") 

# Read in the estimate of a value from a toml file
inf_period = read_estimate(handle,
                           name = "human/infection/SARS-CoV-2",
                           component = "infectious-duration")

# Do some exciting processing
double_period = inf_period * 2

# Write out the newly created value to a toml file
write_estimate(handle, double_period,
               name = "human/infection/SARS-CoV-2/doubled",
               component = "doubled-infectious-period")

finalise(handle)
```

This script will find the latest version of the `human/infection/SARS-CoV-2` data product in the local registry, read the file and find the `infectious-duration` component. It will then double it and save the new value to disk as the `doubled-infectious-period` component of the `human/infection/SARS-CoV-2/doubled` data product, and record its existence in the local registry. When saving the metadata to the registry, that will include its provenance -- that it depends on the old value and was generated by the script being executed.
