---
weight: 30
title: "DP API Example 2"
---

# Data Pipeline API Example 2

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

## Working *config.yaml*

`fdp run` should create a working *config.yaml* file, which is read by the Data Pipeline API.

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

