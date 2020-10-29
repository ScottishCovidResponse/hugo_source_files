---
title: Filenames and version numbers
weight: 1
---

# How to assign version numbers and filenames

* The version of a data product is identified by its filename
* The version of a raw data file is the same as that of the data product

## When a dataset is static (downloaded only once)

Filenames are written `major.minor.patch.extension`, *e.g.* `0.1.0.toml`, `0.1.0.h5`, `0.1.0.csv`.

Major
: Changes only for the initial stable release (go from `0.y.z` to `1.0.0`) and when incompatible changes are made

Minor
: Changes when new functionality is added such as a new component, or for the initial release that is *probably* stable, or a script that definitely works better even though the output is technically the same (go from `0.0.z` to `0.y.0`)

Patch
: Changes for small bug fixes

## When a dataset is dynamic (downloaded daily, weekly, etc.)

Filenames are written thus (named after the download date):

* `1.20200716.0.csv`
* `1.20200716.0.h5`

For example, you might want to start the disease data with `0.20200722.0`, etc. until you're really confident that it's all good and you're happy to make a `1.2020mmdd.0` release.

If there's a bugfix to the dataset, then you'd go from `1.20200716.0.csv` to `1.20200716.1.csv`.

However if you have a completely new storage format, where you have different components and/or different formats of the components, then you go from `1.20200716.0.csv` to `2.20200716.0.csv`, because major number changes are supposed to not be backward compatible.
