---
weight: 7
title: "Jargon buster"
---

# Jargon buster

## C

`config.yaml`
: a file that specifies the metadata used during file lookup (for read, write, and register), used to configure the overall API behaviour.

## D

Data product
: objects that are accessable with the standard API `read_xxx()`, `write_xxx()` calls (*i.e.* toml or hdf5 files)

Data registry
: a database for recording metadata about data and model outputs

## E

External object
: an object that is not in an internal pipeline format (*i.e.* not toml or hdf5 files)

## F

`finalise()`
: calculates the hash of the external objects / data products and renames files, check files exist, etc.

## I

`initialise()`
: reads in the `config.yaml` file, generates a new entry in `code_run` and returns id, creates new `submission_script` (and if necessary creates a new `code_repo`) entry in local registry

## L

`link_read()`
: connects `code_run` to `external_object` as input and returns path location (see also `link_write()`)

`link_write()`
: connects `code_run` to `data_product` as output and returns path location (see also `link_read()`)

## R

`read:`
: a field in `config.yaml` used to specify the reading of an object (a data product or an external object) that belongs to the user (see also `write:` and `register:`)

`register:`
: a field in `config.yaml` used to specify the registration of an object (usually an external object) that exists elsewhere (see also `write:` and `read:`)

`add_to_register()`
: registers the object(s) listed in `register:` (in the `config.yaml` file)

## W

`write:`
: a field in `config.yaml` used to specify the writing of an object (a data product or an external object) that belongs to the user (see also `register:` and `read:`)