---
weight: 2
bookFlatSection: true
title: "SCRC data pipeline"
---

# SCRC data pipeline

The data pipeline is moving towards this new structure:

![SCRC Data API design figure](scrc-api-new.svg)

The API is accessed as a *session*, which corresponds in the registry to a *code run*. All reads and writes are logged directly to a local registry as the session progresses, as is the script which generates that i/o. Files are identified by their metadata, though the metadata is different for reads (where the files must exist) and writes (where they must not). The metadata is also different for *files in core pipeline data formats*, where a significant amounts of metadata are recorded, and *other arbitrary files*, where only a limited amount of data can be collected. Either type of file can be used for input or output, giving a total of four different interactions, two for input and two for output. These differences are described in more detail below.

The underlying data to which the API refers is determined by the interaction between a yaml configuration file (referred to here as a `config.yaml` file, and described below) provided at initialisation and the state of the remote registry at the time of processing of the `config.yaml` by the download synchronisation script (which is considered to contain the definitive version of all data at that time). The specific remote registry used is itself defined in the `config.yaml` file.

This interaction between the configuration file and the remote registry defines the “*local filesystem data repository*” that the local pipeline interacts with. The data directory can be automatically created by a *download synchronisation script* [(currently found here)](https://github.com/ScottishCovidResponse/data_pipeline_api/tree/master/data_pipeline_api/registry) which reads the `config.yaml` file, queries the appropriate remote registry, downloads appropriate data, and populates the local registry with the relevant metadata for those data.

When a model or script is run (as a *session* / “*code run*”), any output files are written to the data directory, and those outputs are logged in the local registry, which has itself been created (or updated) by the *download synchronisation script*. The local registry can be queried to determine whether the data generated is as intended, and if so it can then by synchronised back to the remote registry. This can be carried out automatically using an *upload synchronisation script* [(currently here)](https://github.com/ScottishCovidResponse/data_pipeline_api/tree/master/data_pipeline_api/registry). When the *session* is initialised a “*run id*” is created to uniquely identify that *code run*. It is constructed by forming the SHA1 hash of the configuration file content, plus the date time string.

## config.yaml file format 

The config file lets users specify metadata to be used during file lookup for read or write, and configure overall API behaviour. A simple example: 

```
data_directory: . 
fail_on_hash_mismatch: True 
run_metadata: 
  description: A test model 
  remote_data_registry_url: https://data.scrc.uk/api/ 
  default_input_namespace: SCRC 
  default_output_namespace: johnsmith

read: 
- data_product: human/commutes
    use:
      version: 1.0
- data_product: human/population
    use:
      namespace: eera
      data_product: scotland/human/population
- data_product: human/health
    use:
      cache: ~/local/file.h5
- external_object: crummy_table
    use:
      doi: 10.1111/ddi.12887
      title: Supplementary Table 2
- external_object: secret_data
    use:
      doi: 10.1111/ddi.12887
      title: Supplementary Table 3
      cache: ~/local/secret.csv
- object: weird_lost_file
    use:
      hash: b5a514810b4cb6dc795848464572771f
write:
- data_product: human/outbreak-timeseries
    use:
      data_product: scotland/human/outbreak-timeseries
- data_product: human/outbreak/simulation_run
    use:
      data_product: scotland/human/outbreak/simulation_run-{run_id}
- external_object: beautiful_figure
    use:
      unique_name: My amazing figure
      version: minor
```

- `data_directory:` specifies the file system root used for data access (default “.”). It may be relative; in which case it is relative to the directory containing the config file.

- Any part of a `use:` statement may contain the string `{run_id}`, which will be replaced with the run id.

- `fail_on_hash_mismatch:` will, if set to True (the default), cause the API to fail is an attempt is made to read a file whose computed hash differs from the one stored in the local registry.

- `run_id` specifies the run id to be used, otherwise a hash of the config contents and the date will be used.

- `run_metadata` provides metadata for the run.

The `data_product:` (within `read:` and `write:`), `external_object:` (`read:` and `write:`) and `object:` (`read:` only) sections specify metadata subsets that are matched in the read and write processes. The metadata values may use glob syntax, in which case matching is done against the glob. The corresponding `use:` sections contain metadata that is used to update the call metadata before the file access is attempted. For reads, a `cache:` may be specified directly, in which case it will be used without any further lookup. If a write is carried out to a data product where no such `data_product:` section exists, then a new data product is created with that name in the local namespace, or the version of an existing data product is suitably incremented. If a write is carried out to an object that is not a data product and no such `external_object:` section exists, then a new object is created with no associated external object or data product, and an issue is raised with the object to note the absence of an appropriate reference, referencing the name given in the write API call.

Any other attributes will be ignored.

## Example use of pipeline

One of the simplest possible use cases for the pipeline - read in a value, calculate a new value from it, and write out the new value, creating a provenance for the new value that it depends on the old value and the script being executed.

```julia
using DataPipeline

# Open the connection to the local registry with a given config file
handle = initialise("config.yaml") # Need to resolve how to reference this script

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
