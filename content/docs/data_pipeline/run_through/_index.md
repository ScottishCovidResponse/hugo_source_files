---
weight: 2
title: "Run through"
---

# Run through

<span style="font-size:14pt; color:red">Note that this is very much a first pass and all of the following is subject to change. At the very least, this is simple example of a registry use case.</span>

My code lives [here](https://github.com/ScottishCovidResponse/SCRCdataAPI/tree/implement_yaml).

Here, I've assumed that `fdp run` will read the `config.yaml` file and generate a working `config.yaml` file with specific version numbers and no aliases. It should save this file in the local data store (in `<local_store>/config/date-time.yaml`) and save the path to this file in the global environment as `$wconfig`.

I've also assumed that a local registry token is saved within `.scrc`. However, I would suggest that `fdp config` should update a user file in `.scrc` containing user information (API token, associated namespace, local data store, login node, and so on).

The following example will download an external object and generates a data product by means of a processing script. In this example the model API includes `initialise()`, `add_to_regsiter()`, `read_link()`, `write_array`, and `finalise()`.

```R
library(SCRCdata) # contains process_scotgov_deaths()
library(SCRCdataAPI)

fdp_run("config.yaml")

# Open the connection to the local registry with a given config file
h <- initialise()
# Download data source, save it in the local data store, and register
# metadata in the local registry
add_to_register(h, "raw-mortality-data")

# Return location of file stored in the pipeline
input_path <- read_link(h, "raw-mortality-data")

h <- process_scotgov_deaths(h, input_path)

finalise(h)
```

## `initialise()` should:
- read the working `config.yaml` file and record its location to the data registry
- get the hash of the latest commit on GitHub and check that the local repository is clean, if it is given (note that either the local or remote repository must always be given. It is possible that the user may reference a script located outside of a repository, but they must work from a repository)
- write the submission script to the data store (in `<local_store>/script/date-time.sh`) and record this location in the data registry (note that `config.yaml` should contain either `script:` that should be saved as the submission script, or `script_path:` that points to the file that should be saved as the submission script)
- return a `handle` containing the working `config.yaml` contents, the object id for this file and the object id for the submission script file

## `add_to_register()` should:
- in most cases you should register an external object, but data products might also be possible to register
- download the external object to the data store, get the file hash, and rename the file
- record the original source in the data registry (the website homepage / organisation)
- record the original source location in the data registry (the link / query)
- record the local data store location in the data registry (note that `fdp pull` and `fdp push` will want to change these values)
- record the external object in the data registry (linking all of the above)
- update the `handle` with the external object id

## `read_link()` should
- find the file referenced by its `alias` (note that the alias is not recorded in the data registry, rather, it's a means to reference items in the `config.yaml`)

## `write_array()` should
- write an array as a component to an hdf5 file
- update the `handle` with the component that was written and its location in the data store

## `finalise()` should
- record the local data store root in the data registry
- rename the data product as `hash.h5`
- record the data product location in the data registry
- record the code run in the data registry