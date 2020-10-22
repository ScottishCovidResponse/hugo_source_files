---
title: Upload a data product
weight: 4
---

# How to upload a data product

1. Push metadata associated with your data product to the data registry
   * Choose a [submission script template]({{< ref "docs/data_pipeline/R/5_templates" >}})
   * Edit the script
   * Push the script to GitHub
   * Check to make sure the version of the script you're about to run is exactly the same as the one you've pushed to GitHub – if it isn't, push all changes now!
   * Run the script

2. Upload your raw data (*e.g.* a csv file) to the Boydorr server
   * Ask Richard Reeve for access to the server
   * Upload your file, in terminal (MacOS):

     ``` bash
     scp my/local/directory/0.1.0.csv myusername@boydorr.gla.ac.uk:/srv/ftp/scrc/[data_product_name]/0.1.0.csv
     ```

     or at the command prompt (Windows):
     ``` bash
     # Ryan?
     ```

3. Upload your data product
   * If it's an HDF5 file, upload it to the Boydorr server
   * If it's a TOML file, push it to the [ScottishCovidResponse/DataRepository](https://github.com/ScottishCovidResponse/DataRepository) repository on GitHub
