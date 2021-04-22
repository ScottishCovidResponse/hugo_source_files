---
weight: 2
title: "Run through"
---

# Run through

<span style="font-size:14pt; color:red">Note that this is very much a first pass and all of the following is subject to change. At the very least, this is simple example of a registry use case.</span>

My code lives [here](https://github.com/ScottishCovidResponse/SCRCdataAPI/tree/implement_yaml) and uses the `config.yaml` shown [here](https://scottishcovidresponse.github.io/docs/data_pipeline/interface/#example-register-a-new-external-object-and-write-a-data-product-component).

I've assumed that a local registry token is saved within `.scrc`. However, I would suggest that `fdp config` should update a user file in `.scrc` containing user information (API token, associated namespace, local data store, login node, and so on).

The following example (submission script) will download an external object and generate a data product by means of a processing script. In this example the model API includes `initialise()`, `add_to_register()`, `read_link()`, `write_array`, and `finalise()`.

```R
library(SCRCdata) # contains process_scotgov_deaths()
library(SCRCdataAPI)

# Open the connection to the local registry with a given config file
h <- initialise()

# Return location of file stored in the pipeline
input_path <- read_link(h, "raw-mortality-data")

process_scotgov_deaths(h, input_path)

finalise(h)
```

## `fdp pull`

- download any data required by `read:` from the remote data store and record metadata in the data registry (whilst editing relevant entries, *e.g.* `storage_root`)
- pull data associated with all previous versions of these objects from the remote data registry
- download any data listed in `register:` from the original source and record metadata in the data registry
- `local_repo:` must always be given in the *config.yaml* file
  - get the remote repo url from the local repo
  - get the hash of the latest commit on GitHub and store this in the data registry, associated with the submission script `storage_location` (this is where the script should be stored)
  - note that there are exceptions and the user may reference a script located outside of a repository

## `fdp run`

- read (and validate) the *config.yaml* file
- generate a working *config.yaml* file  
  - globbing (`*` and `**` replaced with all matching objects, all components listed), specific version numbers, and any variables (*e.g.* `{CONFIG_PATH}`, `{VERSION}`, `{DATETIME}`) is replaced with true values
  - `register:` is removed and external objects / data products are written in `read:`
- save the working *config.yaml* file in the local data store (in *<local_store>/coderun/\<date>-\<time>/config.yaml*) and register metadata in the data registry
- save the submission script to the local data store in *<local_store>/coderun/\<date>-\<time>/script.sh* (note that *config.yaml* should contain either `script:` that should be saved as the submission script, or `script_path:` that points to the file that should be saved as the submission script) and register metadata in the data registry
- save the path to *<local_store>/coderun/\<date>-\<time>/* in the global environment as `$fdp_config_dir` so that it can be picked up by the script that is run after this has been completed
- execute the submission script

## `initialise()`

- read the working *config.yaml* file
- return a `handle` containing the working *config.yaml* contents, the object id for this file and the object id for the submission script file

## `read_link()`

- find the location of the file referenced by its `alias` (*e.g.* `raw-mortality-data`, note that the alias is not recorded in the data registry, rather, it's a means to reference items in the *config.yaml*)
```
register:
- external_object: raw-mortality-data
```

## `write_array()`

- write an array as a component to an hdf5 file
- update the `handle` with the component that was written and its location in the data store

## `finalise()`

- rename the data product as *<hash>.h5*
- record data product metadata (*e.g.* location, components, various descriptions, issues) in the data registry
- record the code run in the data registry

## `fdp push()`

- push new files (generated from `write:` and `register:`) to the remote data store
- record metadata in the data registry (whilst editing relevant entries, *e.g.* `storage_root`)
