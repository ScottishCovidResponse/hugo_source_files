---
title: HDF5 files
weight: 2
---

# HDF5 files

An h5/hdf5 file can be either a table or an array. A table is always 2-dimentional and might typically be used when each column contains different classes of data (*e.g.* integers and strings). Conversely, all elements in an array should be the same class, though the array itself might be 1-dimensional, 2-dimensional, or more (e.g. a 3-dimensional array comprising population counts, with rows as area, columns as age, and a third dimension representing gender).

You should create a single h5/hdf5 file for a single dataset. Unless you have a dataset that really should have been generated as multiple datasets in the first place (*e.g.* testing data mixed with carehome data), in which case use your own judgement.

The file itself should contain components. If your dataset contains multiple data topics / data items, then these can be included as separate components within a single HDF5 file, *e.g.* cases-and-management/schools contains four components; pupil absences for covid reasons, percentage absences for covid reasons,  percentage absences for non-covid reasons, and percentage attendance.

## Single component

If your dataset contains a single component, for `create_table()` call it "`table`" and for `create_array()` call it "`array`".

## Multiple components

If your dataset contains a multiple components, then a particular naming convention is needed. For example, in the human-mortality data product:

* `age_group/week/gender-country-all_deaths`
* `age_group/week/gender-country-covid_related_deaths`  
* `age_group/week-persons-country-all_deaths`

Note the naming convention! The first component is described thusly:
* `age_group/week/gender` corresponds to the first, second, and third dimensions of the data (rows, columns, and levels) 
* `-country-all_deaths` applies to all elements in the array
* spaces are replaced with underscores