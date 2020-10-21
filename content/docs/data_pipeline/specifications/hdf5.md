---
title: HDF5 files
weight: 2
---

# HDF5 files

An h5/hdf5 file can be either a table or an array. A table is always 2-dimentional and might typically be used when each column contains different classes of data (*e.g.* integers and strings). Conversely, all elements in an array should be the same class, though the array itself might be 1-dimensional, 2-dimensional, or more (e.g. a 3-dimensional array comprising population counts, with rows as area, columns as age, and a third dimension representing gender).

You should create a single h5/hdf5 file for a single dataset. Unless you have a dataset that really should have been generated as multiple datasets in the first place (*e.g.* testing data mixed with carehome data), in which case use your own judgement.

## Components

The file itself should contain components. If your dataset contains multiple data topics / data items, then these can be included as separate components within a single h5/hdf5 file. In this case, a particular naming convention is needed. For example, in the human-mortality data product:

* `age_group/week/gender-country-all_deaths`
* `age_group/week/gender-country-covid_related_deaths`  
* `age_group/week-persons-country-all_deaths`

Taking the first component as an example:

* Dimensionality is represented by `age_group/week/gender`, corresponding to the first, second, and third dimensions of the component (rows, columns, and levels)
* A description of the contents follows the dash, where `-country-all_deaths` describes all elements
* Spaces are replaced with underscores

If your dataset contains a single data topic, then only a single component is needed. If the h5/hdf5 file is generated using `create_table()` then name the component "`table`", likewise when `create_array()` is used then the component should be named "`array`".

The functions `create_array()` and `create_table()` can be used to generate an h5/hdf5 file.

## create_array()

``` R
# Create a fake dataset
df <- data.frame(a = 1:2, b = 3:4)
rownames(df) <- 1:2
array <- as.matrix(df)

# Create an h5 file from a 2-dimensional array
create_array(filename = "0.1.0.h5",
             path = "test",
             component = "row/column-constant",
             array = array,
             dimension_names = list(rowvalue = rownames(df),
                                    colvalue = colnames(df)))
```

## create_table()

``` R
# Create fake data
df <- data.frame(a = 1:2, b = 3:4)
rownames(df) <- 1:2
filename <- "test_table.h5"

# Create an h5 file from a table
create_table(filename = filename,
             path = ".",
             component = "level",
             df = df,
             row_names = rownames(df),
             column_units = c(NA, "m^2"))
```
