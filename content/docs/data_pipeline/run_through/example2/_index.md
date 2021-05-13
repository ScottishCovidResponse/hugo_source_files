---
weight: 30
title: "Read data product, process, and write data product (with aliases)"
---

# Read data product, process, and write data product (with aliases)

## *config.yaml*

`fdp pull` and `fdp run` require a *config.yaml* file to be supplied by the user.

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
    data_product: human/outbreak/simulation_run-{run_id}
```

## Working *config.yaml*

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
    data_product: human/outbreak/simulation_run-{run_id}
```

## *submission_script.R*

A submission script should be supplied by the user. In the above example, the location of this script is referenced in `run_metadata: script:`.

```R
library(SCRCdataAPI)

# Open the connection to the local registry with a given config file
# You can put in a file if you really want to, but otherwise read from 
# the environment directly or from a command line argument
handle <- initialise(Sys.getenv("FDP_CONFIG_DIR"))

# Read data product
data <- read_array("human/population")

# Process raw data and write data product
array <- some_processing(data)
write_array(array, 
            handle, 
            data_product = "human/outbreak-timeseries", 
            component = "component_name",
            dimension_names = list(location = rownames(array),
                                   date = colnames(array)))

results <- more_processing(array)
write_array(results, 
            handle, 
            data_product = "human/outbreak/simulation_run", 
            component = "component_name",
            dimension_names = list(location = rownames(results),
                                   date = colnames(results)))

finalise(handle)
```

### Data product naming

- `read_array()` is responsible for reading the correct data product (*i.e.* *scotland/human/population*), which at this point has been downloaded from the remote data store by `fdp pull`
- `write_array()` is responsible for allocating the correct data product name (*i.e.* *scotland/human/outbreak-timeseries* and *human/outbreak/simulation_run-{run_id}*, where `{run_id}` is replaced with an appropriate index) to the data

### Data product versioning

- Note that `version` has been omitted from the *config.yaml* file)
- `read_array()` should by default read the latest version of the data, which at this point has been downloaded from the remote data store by `fdp pull`
- `write_array()` should by default increment the data product by PATCH if none is specified
