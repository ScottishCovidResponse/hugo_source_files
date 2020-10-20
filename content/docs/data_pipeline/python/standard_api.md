---
weight: 1
---

# Standardised data type API 

Unless otherwise specified this document uses the “current” metadata scheme defined in the "Metadata and data representation” document. You do not need to have read that document to be able to read and understand (the majority of) this one. 

The standardised data type API is defined more in terms of file formats than it is in terms of data types. There are two file formats: parameter files, and hdf5 files. Parameter files store “small” parameter data in toml format, representing individual parameters. HDF5 files are used to store structured data, encoded either as “arrays” or “tables”. Both formats are described in more detail below, alongside API functions used to interact with them. 

## Initialisation 

The standard API must be initialised with the model URI and git sha, which should then be set as run-level metadata using the file API set_metadata function. 

## Additional metadata on write 

The write functions all accept `description` and `issues` arguments, which are passed through to the file API as component-level metadata. 

## parameter files 

A parameter file contains representations of one or more parameters, each a single number, possibly with some associated uncertainty. Parameters may by represented as point-estimates, parametric distributions, and sample data. 

## Metadata 

```
extension = "toml" 
```

## Alternative metadata 

```
format = "parameter"  
extension = "toml" (could be inferred) 
```

## File format 

Parameters are stored in toml-formatted files, with the extension “toml”, containing sections corresponding to different components. The following is an example of the internal encoding, defining three components: "`my-point-estimate`", "`my-distribution`", and "`my-samples`": 

```
[my-point-estimate] 
type = "point-estimate" 
value = 0.1 

[my-distribution] 
type = "distribution" 
distribution = "gamma" 
shape = 1 
scale = 2 
 
[my-samples] 
type = "samples" 
samples = [1.0, 2.0, 3.0, 4.0, 5.0] 
```

Point estimates are used when our knowledge of the parameter is only sufficient for a single value, with no notion of uncertainty. A point estimate component must have type = "point-estimate" and a value that is either a float or an integer. 

Distributions are used when our knowledge of a parameter can be represented by a parametric distribution. A distribution component must have type = "distribution", a distribution set to a string name of the distribution, and other parameters determined by the distribution. The set of distributions required to be supported is currently undefined. 

Samples are used when our knowledge of a parameter is represented by samples, from either empirical measurements, or a posterior distribution. A samples component must have type = "samples" and a value that is a list of floats and integers. 

## Distributions 

The supported distributions,each with a link to information about their parameterisation, and their standardised parameter names are as follows: 


| Distribution                  | Standardised parameter names                  |
| ----------------------------- | --------------------------------------------- |
| categorical (non-standard)    | bins (string array), weights (float array)    |
| gamma                         | k (float), theta (float)                      |
| normal                        | mu (float), sigma (float)                     |
| uniform                       | a (float), b (float)                          |
| poisson                       | lambda (float)                                |
| exponential                   | lambda (float)                                |
| beta                          | alpha (float), beta (float)                   |
| binomial                      | n (int), p (float)                            |
| multinomial                   | n (int), p (float array)                      |

## API functions 

`read_estimate(data_product, component) -> float or integer`

If the component is represented as a point estimate, return that value. 

If the component is represented as a distribution, return the distribution mean. 

If the component is represented as samples, return the sample mean. 

`read_distribution(data_product, component) -> distribution object`

If the component is represented as a point estimate, fail. 

If the component is represented as a distribution, return an object representing that distribution. 

If the component is represented as samples, failreturn an empirical distribution. 

`read_samples(data_product, component) -> list of floats or integers` 

If the component is represented as a point estimate, fail. 

If the component is represented as a distribution, fail. 

If the component is represented as samples, return the samples. 

`write_estimate(data_product, component, estimate, description, issues)` 

`write_distribution(data_product, component, distribution object, description, issues)`

`write_samples(data_product, component, samples, description, issues)` 

## HDF5 files 

HDF5 files contain structured data, encoded as either an “array”, or a “table”, both of which are described in more detail below. 

## Metadata 

extension = "h5"

## Alternative metadata 

format = "hdf5"  
extension = "h5" (could be inferred)

## File format 

HDF5 files are stored with the extension “h5”. Internally, each component is stored in a different (possibly nested) group, where the full path defines the component name (*e.g.* “path/to/component”). Inside the group for each component is either a value named “array”, or a value named “table”. It is an error for there to be both.

## array format

{component}/array
: An n-dimensional array of numerical data

{component}/Dimension_{i}_title
: The string name of dimension {{< katex >}}i{{< /katex >}}

{component}/Dimension_{i}_names
: String labels for dimension {{< katex >}}i{{< /katex >}}

{component}/Dimension_{i}_values
: Values for dimension {{< katex >}}i{{< /katex >}}

{component}/Dimension_{i}_units  
: Units for dimension {{< katex >}}i{{< /katex >}}

{component}/units
: Units for the data in array

## table format

{component}/table
: A dataframe

{component}/row_names
: String labels for the row axis

{component}/column_units
: Units for the columns

## API functions

`read_array(data_product, component) -> array`

If the component does not terminate in an array-formatted value, raise an error. 

Return an array, currently with no structural information. 

`read_table(data_product, component) -> dataframe`

If the component does not terminate in a table-formatted value, raise an error. 

Return a dataframe, with labelled columns. 

`write_array(data_product, component, array, description, issues)` 

`write_tab`
