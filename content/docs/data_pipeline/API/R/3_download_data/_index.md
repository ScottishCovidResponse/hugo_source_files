---
title: Download a data product
weight: 3
---

# How to download a data product

1. Go to the [data registry](https://data.scrc.uk/) and browse the data products.

2. Identify the name of the data product (**Data Product Name**):
<br> *e.g.* records/SARS-CoV-2/scotland/cases-and-management/testing.

3. Download the H5 (or TOML) file:

   ``` R
   library(SCRCdataAPI)
   file <- download_data_product(name = "records/SARS-CoV-2/scotland/cases-and-management/testing",
                                 data_dir = "my_data")
   ```

   where name corresponds to the data product name and data_dir identifies the location in which the data will be saved. If the directory does not already exist, it will be created.

   When this code is run, an H5 (or TOML) file will be downloaded to `data_dir`, unless a file by the same name already exists at the specified location, in which case a message will notify you. The `download_data_product()` function will return a list comprising two named elements: `downloaded_to`, the absolute path of H5 file after downloading; and `components`, the components contained within the H5 (or TOML) file.

   Note that, omitting the `version` argument will result in the most recent version of the file being downloaded. If you want to specify a particular version number however, you can do so.

4. An H5 (or TOML) file will always contain at least one component, each containing of a particular dataset. These are listed in the data registry, or can be listed in R using:

   ``` R
   get_components(file$downloaded_to)
   ```

4. Now, pick one of these components and read it into R using either `read_array()`, `read_table()`, `read_estimate()`, or `read_distribution()`:

   ``` R
   tmp <- read_array(filepath = file$downloaded_to, 
                     component = file$components[4])
   head(tmp)
   ```

## How to download raw data (an external object)

You shouldn't need to download the raw data file, since the data product should already be available, but for the sake of completeness...

An external object is an original source file. This might be a reference to a paper or a raw, unedited csv file. Papers are not stored by the SCRC. Raw data objects are and can be downloaded in the following way.

1. Go to the [data registry](https://data.scrc.uk/) and browse the external objects.

2. Identify the name of the external object (**External Object DOI or Unique Name**):
<br> *e.g.* scottish coronavirus-covid-19-management-information.

3. Download the data file (*e.g.* csv file):

   ``` R
   library(SCRCdataAPI)
   file <- download_external_object(name = "scottish coronavirus-covid-19-management-information",
                                    data_dir = "my_data")
   ```

   where name corresponds to the data product name and data_dir identifies the location in which the data will be saved. If the directory does not already exist, it will be created.

   When this code is run, an hdf5 file will be downloaded, unless a file by the same name already exists at the specified location, in which case a message will notify you. The `download_external_object()` function will return a list comprising two named elements, `downloaded_to` (the absolute path of H5 file after downloading) and `components` (the components contained within the H5 file).

   Note that, omitting the `version` argument will result in the most recent version of the file being downloaded. If you want to specify a particular version number however, you can do so.
