---
weight: 20
title: "Core FDP functionality"
---

<span style="font-size:12pt; color:red">Note that this is a living document and the following is subject to change.</span>

# Core `fdp` functionality

A simple example of how the data pipline should run from the command line:

```bash
fdp pull config.yaml
fdp run config.yaml
fdp push config.yaml
```

## `fdp pull`

- download any data required by `read:` from the remote data store and record metadata in the data registry (whilst editing relevant entries, *e.g.* `storage_root`)
- pull meta data associated with all previous versions of these objects listed in `write:` from the remote data registry
- download any data listed in `register:` from the original source and record metadata in the data registry

## `fdp run`

- read (and validate) the *config.yaml* file
- generate a working *config.yaml* file (see [Working example]({{% ref "/docs/data_pipeline/interface/example1" %}}))
  - globbing (`*` and `**` replaced with all matching objects, all components listed), specific version numbers, and any variables in `run_metadata:`, `register:`, and `read:` (not `write:`) are replaced with true values
    - *e.g.* `{CONFIG_DIR}` is replaced by the directory within which the working *config.yaml* file resides, `release_date: {DATETIME}` is replaced by `release_date: 2021-04-14 11:34:37`, and `version: 0.{DATETIME}.0` is replaced by `version: 0.20210414.0`
  - `register:` is removed and external objects / data products are written in `read:`
- save the working *config.yaml* file in the local data store (in *<local_store>/coderun/\<date>-\<time>/config.yaml*) and register metadata in the data registry
- save the submission script to the local data store in *<local_store>/coderun/\<date>-\<time>/script.sh* (note that *config.yaml* should contain either `script:` that should be saved as the submission script, or `script_path:` that points to the file that should be saved as the submission script) and register metadata in the data registry
- save the path to *<local_store>/coderun/\<date>-\<time>/* in the global environment as `$FDP_CONFIG_DIR` so that it can be picked up by the script that is run after this has been completed
- `local_repo:` must always be given in the *config.yaml* file
  - get the remote repo url from the local repo
  - get the hash of the latest commit on GitHub and store this in the data registry, associated with the submission script `storage_location` (this is where the script should be stored)
  - note that there are exceptions and the user may reference a script located outside of a repository
- execute the submission script

## `fdp push`

- push new files (generated from `write:` and `register:`) to the remote data store
- record metadata in the data registry (whilst editing relevant entries, *e.g.* `storage_root`)
