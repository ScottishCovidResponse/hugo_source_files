---
title: Versioning
weight: 3
---

# Versioning

* The version of a data product is identified by its filename
* The version of a raw data file is the same as that of the data product

## For a dataset that is downloaded only once

Filenames are written thus (`major.minor.patch`):

* `0.1.0.toml`
* `0.1.0.h5`
* `0.1.0.csv`

**Major** changes only for the initial stable release (when you go from `0.y.z` to `1.0.0`), and when you have incompatible changes.  
**Minor** changes when you're adding new functionality (*e.g.* a new component) or for the initial release that you think is probably right (when you go from `0.0.z` to `0.1.0`).  
**Patch** changes for small bug fixes.

You can increase minor numbers a bit more freely than that if you're making a bigger change, or you want to make a new and improved script that definitely works better even though the output is technically the same, but those are the "official" rules.

## For a dataset that is downloaded daily

Filenames are written thus (named after the download date):

* `1.20200716.0.csv`
* `1.20200716.0.h5`

For example, you might want to start the disease data with `0.20200722.0`, etc. until you're really confident that it's all good and you're happy to make a `1.2020mmdd.0` release.

If there's a bugfix to the dataset, then you'd go from `1.20200716.0.csv` to `1.20200716.1.csv`.

However if you have a completely new storage format, where you have different components and/or different formats of the components, then you go from `1.20200716.0.csv` to `2.20200716.0.csv`, because major number changes are supposed to not be backward compatible.
