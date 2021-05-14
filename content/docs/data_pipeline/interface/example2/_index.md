---
weight: 40
title: "Additional examples"
---

<span style="font-size:12pt; color:red">Note that this is a living document and the following is subject to change.</span>

# Additional examples

## Read data product, process, and write data product (with aliases)

### User written *config.yaml*

```yaml
run_metadata:
  description: A test model
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: soniamitchell
  default_data_store: /Users/SoniaM/datastore/
  local_repo: /Users/Soniam/Desktop/git/SCRC/SCRCdata
  script: |- 
    R -f inst/SCRC/scotgov_management/submission_script.R {CONFIG_DIR}

read:
- data_product: human/population
  use:
    namespace: johnsmith
    data_product: scotland/human/population

write:
- data_product: human/outbreak-timeseries
  use:
    data_product: scotland/human/outbreak-timeseries
- data_product: human/outbreak/simulation_run
  use:
    data_product: human/outbreak/simulation_run-{RUN_ID}
```

### Working *config.yaml*

`fdp run` should create a working *config.yaml* file, which is read by the Data Pipeline API. In this example, the working *config.yaml* file is pretty much identical to the original *config.yaml* file, only `{CONFIG_DIR}` is replaced by the directory in which the working *config.yaml* file resides.

```yaml
run_metadata:
  description: A test model
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC
  default_output_namespace: soniamitchell
  default_data_store: /Users/SoniaM/datastore/
  local_repo: /Users/Soniam/Desktop/git/SCRC/SCRCdata
  script: |- 
    R -f inst/SCRC/scotgov_management/submission_script.R /Users/SoniaM/datastore/coderun/20210511-231444/

read:
- data_product: human/population
  use:
    namespace: johnsmith
    data_product: scotland/human/population

write:
- data_product: human/outbreak-timeseries
  use:
    data_product: scotland/human/outbreak-timeseries
- data_product: human/outbreak/simulation_run
  use:
    data_product: human/outbreak/simulation_run-{RUN_ID}
```

## Read and write an external object

A script to read and write an external object (*i.e.* something not in a core data pipeline format) in R. First, the yaml file, that gives the `doi_or_unique_name` and `title` of the external objects being read and written, and the aliases that will be used in the submission script:

### User written *config.yaml*

```yaml
run_metadata: 
  description: A simple example using external objects
  local_data_registry_url: https://localhost:8000/api/
  remote_data_registry_url: https://data.scrc.uk/api/
  default_input_namespace: SCRC 
  default_output_namespace: johnsmith
  default_data_store: /datastore/
  local_repo: /Users/johnsmith/git/myproject/
  script: # Points to the R script, below (relative to local_repo)
    R -f path/submission_script.R {CONFIG_DIR}

read: 
- external_object: time-series
  unique_name: An exciting time series
  title: Table 1

write: 
- external_object: revised-time-series
  unique_name: An new, revised, time series
  title: Table 1
  file_type: csv
  primary: True
```

### Working *config.yaml*

```yaml
...
```

## Read then write a data product component

Now that the pipeline is populated, one of the simplest possible use cases is just to read in a value, calculate a new value from it, and write out the new value. Again, we need to write a *config.yaml* file:

### User written *config.yaml*

```yaml
run_metadata: 
  description: A simple example reading and writing data products
  default_input_namespace: SCRC
  local_repo: /Users/johnsmith/git/myproject
  script: | # addresses are relative to local_repo
    julia -f path/submission_script.jl {CONFIG_DIR}

read:
- data_product: human/infection/SARS-CoV-2

write:
- data_product: human/infection/SARS-CoV-2/doubled
  component: doubled-infectious-period
```

### Working *config.yaml*

```yaml
...
```
