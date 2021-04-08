---
weight: 2
---

# File API

## The file API manages file access, provenance, and metadata

The API is accessed as a "session". All reads and writes are recorded and logged into a file when the session closes. Files are identified by their metadata, though the metadata is handled differently for reads (where the files are expected to exist) and writes (where they typically do not), described in more detail below. 

The file API behaviour is entirely determined by a yaml configuration file (referred to here as a “config.yaml” file, and described below) provided at initialisation. This configuration file defines the “data directory” that the file API should interact with. That directory must contain a file called “metadata.yaml”, described below, that defines the metadata associated with the files in the data directory. The data directory and the metadata.yaml file can be automatically created by a [download script](https://github.com/ScottishCovidResponse/data_pipeline_api/tree/master/data_pipeline_api/registry) which reads the config.yaml file and downloads appropriate data and metadata. 

When a model or script is run (in the “run”), any output files are written to the data directory, and an “access.yaml” file is created that enumerates exactly which files were read and written to during the run. The access.yaml file contains sufficient information to upload all of the data and metadata from the run to the data store and data registry respectively. This can be carried out automatically using an [upload script](https://github.com/ScottishCovidResponse/data_pipeline_api/tree/master/data_pipeline_api/registry) if desired. Note that the access.yaml file may not be written until the connection to the API is closed (this is certainly true for the python implementation). When the file API is initialised a “run_id” is created to uniquely identify that invocation. It is constructed by forming the SHA1 hash of the configuration file content, plus the date time string. 

For normal modelling runs, the only interaction with the File API happens through setting the config.yaml file (and running the download and upload scripts), but the rest of the information (formats of the metadata.yaml and access.yaml file, and the low-level File API calls themselves are provided here for completeness). 

## config.yaml file format 

The config file lets users specify metadata to be used during file lookup, and configure overall file API behaviour. A simple example: 

```
data_directory: . 
access_log: access-{run_id}.yaml 
fail_on_hash_mismatch: True 
run_metadata: 
  description: A test model 
  data_registry_url: https://data.scrc.uk/api/ 
  default_input_namespace: SCRC 
  default_output_namespace: model_test 
  submission_script: model.py 
  remote_uri: ssh://boydorr.gla.ac.uk/srv/ftp/scrc/ 
  remote_uri_override: ftp://boydorr.gla.ac.uk/scrc/ 

read: 
- where: 
    data_product: human/commutes 
  use: 
    version: 1.0 
- where: 
    data_product: human/population 
  use: 
    filename: my-human-population.csv 

write:  
- where:  
    data_product: human/outbreak-timeseries  
    Component:  
  use: 
    namespace: simple_network_sim 
    data_product: human/outbreak-timeseries 
```

`data_directory` specifies the file system root used for data access (default “.”). It may be relative; in which case it is relative to the directory containing the config file. The data directory must contain a `metadata.yaml` file. 

`access_log` specifies the filename used to record the access log (default “access-{run_id}.yaml”). It may be relative; in which case it is relative to the directory containing the config file. It may contain the string `{run_id}`, which will be replaced with the run id. It may be set to the boolean value False to indicate that no access log should be written. 

`fail_on_hash_mismatch` will, if set to True (the default), cause the API to fail is an attempt is made to read a file whose computed hash differs from the one stored in the `metadata.yaml` file. 

`run_id` specifies the run id to be used, otherwise a hash of the config contents and the date will be used. 

`run_metadata` provides metadata for the run that will be passed through to the access log. 

The `where` sections specify metadata subsets that are matched in the read and write processes. The metadata values may use glob syntax, in which case matching is done against the glob. The corresponding `use` sections contain metadata that is used to update the call metadata before the file access is attempted. A `filename` may be specified directly, in which case it will be used without any further lookup. 

Any other attributes will be ignored. 

## metadata.yaml file format 

The metadata file contains metadata for all files in the file system “database”. A simple example: 

```
-  
  data_product: human/commutes 
  version: 1 
  extension: csv 
  filename: human/commutes/1.csv 
  verified_hash: 075abd810909918419cf7495c16f1afec6fa010c 
-  
  data_product: human/compartment-transition 
  version: 1 
  extension: csv 
  filename: human/compartment-transition/1.csv 
  verified_hash: 65662d0461471f36a06b32ca6d4003ca4493848f 
```

Each section defines the metadata for a single file, including its filename, relative to the directory containing the metadata.yaml file. 

## access.yaml format 

The access file is generated whenever `close()` is called on the API. It records basic information about the run, and a log of file accesses. An example: 

```
data_directory: . 
run_id: 84b87c5f60 
open_timestamp: 2020-06-24 14:30:22.010927 
close_timestamp: 2020-06-24 14:30:22.038766 
config: 
  ... 
run_metadata: 
  git_repo: https://github.com/ScottishCovidResponse/simple_network_sim 
  git_sha: 353697d0a04ef5d6d5a04ef9aef514cbd72a55fd 
  ... 
io: 
- type: read 
  timestamp: 2020-06-24 14:30:22.018370 
  call_metadata: 
    data_product: human/mixing-matrix 
    extension: csv 
  access_metadata: 
    data_product: human/mixing-matrix 
    version: 1.0.0 
    extension: csv 
    filename: human/mixing-matrix/1.csv 
    verified_hash: 075abd810909918419cf7495c16f1afec6fa010c 
    calculated_hash: 075abd810909918419cf7495c16f1afec6fa010c 
- type: write 
  timestamp: 2020-06-24 14:30:22.038511 
  call_metadata: 
    data_product: human/estimatec 
    extension: csv 
  access_metadata: 
    data_product: human/estimatec 
    extension: csv 
    source: simple_network_sim 
    filename: human/estimatec/84b87c5f60.csv 
    calculated_hash: 91a6791ab4f6d3a4616066ffcae641ca3da79567 
```

`data_directory` specifies the file system root used for data access, either as an absolute path, or relative to the config.yaml file used to generate the run. 

`run_id` specifies the run id of the run. 

`config` reproduces the config.yaml used to generate this run verbatim. 

`run_metadata` contains additional metadata about the file API execution taken from the config and possibly overridden using the `set_run_metadata` function. 

`open_timestamp` and `close_timestamp` record time at which the file API was initialised, and the time at which `close()` was called. 

`io` points to a list of file access sections containing a common format: 

`type` is either read or write. 

`timestamp` is the timestamp of the access. 

`call_metadata` contains the metadata provided to the `open_for_read` or `open_for_write` call. 

`access_metadata` contains the metadata used to open the file. The process for obtaining this metadata is described in the `open_for_read` process and `open_for_write` process sections below. 

## open_for_read process 

1. Search config for all read sections that are a subset of the given metadata and update the call metadata with the corresponding overrides. 
2. Search the metadata file for all metadata sections that are a superset of the updated metadata; if any results, use the metadata section with the highest version. 
3. Use the filename defined in the metadata; fail if there is no filename specified, or if the file is not found. 
4. Calculate the hash of the file and store it in the metadata. 
5. If hash verification is enabled, check that there is a verified hash in the metadata, and that it matches the calculated hash; fail otherwise. 
6. Record the read. 
7. Open the file for read and return the file handle. 

## open_for_write process 

1. Search config for all write sections that are a subset of the given metadata and update the call metadata with the corresponding overrides. 
2. If the metadata does not contain a filename, use the metadata and the run id to construct a standard filename, and add it to the metadata. 
3. Create all missing parent directories. 
4. If the file already exists, open the file for update, else open the file for write (and thus create it implicitly). 
5. Register a call-back to record the write on close and return the open file handle. 

The File API consists of five logical functions (in python, implementation details may vary): 


| Function                      | Description                                   |
| ----------------------------- | --------------------------------------------- |
| init(configuration_filename)  | Initialise the API with a configuration file. |
| open_for_read(metadata)       | Use metadata to open a file for reading.      |
| open_for_write(metadata)      | Use the metadata to open a file for writing.  |
| close()                       | Write the access log to disk. May be called again. |
| set_run_metadata(key, value)  | Associate a (key, value) metadata pair with the run. This is used by the Standard API to transmit the model uri and git_sha to the access.yaml. |

