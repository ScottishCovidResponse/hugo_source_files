---
title: Download a data product
weight: 3
---

# How to download a data product

1. Go to the [data registry](https://data.scrc.uk/) and browse the data products.

2. Click on one of the data products and identify its **Data Product Name**:
<br> *e.g.* records/SARS-CoV-2/scotland/cases-and-management/testing.

3. Download the H5 file:

   ``` R
   library(SCRCdataAPI)
   file <- download_dataproduct(name = "records/SARS-CoV-2/scotland/cases-and-management/testing",
                                data_dir = "my_data")
   ```
   where name corresponds to the data product name and data_dir identifies the location in which the data will be saved. If the directory does not already exist, it will be created.

   When this code is run, an hdf5 file will be downloaded, unless a file by the same name already exists at the specified location, in which case a message will notify you. The `download_dataproduct()` function will return a list comprising two named elements, `downloaded_to` (the absolute path of H5 file after downloading) and `components` (the components contained within the H5 file).

4. An H5 file will always contain at least one component, containing of a particular dataset. These are listed in the data registry, or can be listed using:
   ``` R
   get_components(file$downloaded_to)
   ```

4. Now, pick one of these components and read it into R:

   ``` R
   tmp <- read_array(filepath = file$downloaded_to, 
                     component = file$components[4])
   head(tmp)
   ```