---
weight: 3
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

## O
`open_for_read()`
: connects `code_run` to `external_object` as input and returns path location

`open_for_write()`
: connects `code_run` to `data_product` as output and returns path location

## R
`read:`
: a field in `config.yaml` used to specify the reading of an object (a data product or an external object) that belongs to the user (see also `write:` and `register:`)

`register:`
: a field in `config.yaml` used to specify the registration of an object (usually an external object) that exists elsewhere (see also `write:` and `read:`)

## W
`write:`
: a field in `config.yaml` used to specify the writing of an object (a data product or an external object) that belongs to the user (see also `write:` and `read:`)