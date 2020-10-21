---
title: Generate a data product
weight: 3
---

# How to generate a data product

The data product itself should be producted in the correct format:

* Point estimates, distributions, and samples should be generated as TOML files (`*.toml`)
* Tables and arrays should be generated as HDF5 files (`*.h5`/`*.hdf5`)
* The filename (of the TOML or HDF5 file) should be the version number of the data product (see [Filenames and versioning]({{< ref "docs/data_pipeline/R/1_versioning" >}}))

## TOML files

There are only three types of toml file:

* point-estimate
* distribution
* samples

These are all different ways of representing the estimate for a value, which can be anything - the mean of something, the standard deviation, etc.

### Components

You could have a data product called `latent-period` with a single point estimate:

``` toml
[latent-period]
type = "point-estimate"
value = 1.0
```

In this case, the component is taken as the last part of the name (in the above example, period).

Alternatively, the data product could have several components, for instance:

``` toml
[latent-period]
type = "distribution"
distribution = "gamma"
shape = 1.0
scale = 1.0

[mean]
type = "point-estimate"
value = 1.0

[standard-deviation]
type = "point-estimate"
value = 1.0
```

and so on.

As far as units are concerned, there will be a unit-respecting system in that (if we get the data pipeline grant) every data product component will have to say what units it is in, and the pipeline API will do conversions and error if the units aren't compatible (or aren't provided). However, it's not there yet. The only thing we can suggest at the moment is to decide to use the component names to specify the units, so add an additional component to the TOML file such as:

``` toml
[hours]
type = "point-estimate"
value = 24.0
```

### create_estimate()

Write a single estimate into a toml file:

``` 
create_estimate(filename = "0.1.0.toml",
                path = "human/infection/SARS-CoV-2/asymptomatic_period",
                parameters = list(`asymptomatic-period` = 192.0))
```

Write multiple estimates into a toml file:

```
create_estimate(filename = "0.1.0.toml",
                path = "human/infection/SARS-CoV-2/asymptomatic_period",
                parameters = list(`asymptomatic-period` = 192.0,
                                  `standard-deviation` = 10.2))
```

### create_distribution()

Write a single distribution into a toml file

```
create_distribution(filename = "0.1.0.toml",
                    path = "human/infection/SARS-CoV-2/latency",
                    distribution = list(name = "latency-period",
                                        distribution = "gamma",
                                        parameters = list(shape = 2.0,
                                                          scale = 3.0)))
```

Write multiple distributions into a toml file

```
dist1 <- list(name = "latency-period-1",
              distribution = "gamma",
              parameters = list(shape = 2.0, scale = 3.0))
dist2 <- list(name = "latency-period-2",
              distribution = "gamma",
              parameters = list(shape = 2.2, scale = 4.0))

create_distribution(filename = "0.1.0.toml",
                    path = "human/infection/SARS-CoV-2/latency",
                    distribution = list(dist1, dist2))
```

## HDF5 files

An h5/hdf5 file can be either a table or an array. A table is always 2-dimentional and might typically be used when each column contains different classes of data (*e.g.* integers and strings). Conversely, all elements in an array should be the same class, though the array itself might be 1-dimensional, 2-dimensional, or more (e.g. a 3-dimensional array comprising population counts, with rows as area, columns as age, and a third dimension representing gender).

You should create a single h5/hdf5 file for a single dataset. Unless you have a dataset that really should have been generated as multiple datasets in the first place (*e.g.* testing data mixed with carehome data), in which case use your own judgement.

### Components

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

### create_array()

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

### create_table()

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
