---
title: Upload a data product
weight: 4
---

# How to upload a data product

1. Push metadata associated with your data product to the data registry
   * Choose a [submission script template]({{< ref "/docs/data_pipeline/R/5_templates" >}})
   * Edit the script
   * Push the script to GitHub
   * Check to make sure the version of the script you're about to run is exactly the same as the one you've pushed to GitHub – if it isn't, push all changes now!
   * Run the script

2. Upload your raw data (*e.g.* a csv file) to the Boydorr server
   * Ask Richard Reeve for access to the server
   * Upload your file to the Boydorr server, at this location:<br>
   `/srv/ftp/[namespace]/[data_product_name]/[version_number].csv`
        
     In terminal (MacOS):

     ``` bash
     scp mylocaldir/[data_product_name]/[version_number].csv myusername@boydorr.gla.ac.uk:/srv/ftp/[namespace]/[data_product_name]/[version_number].csv
     ```

     or at the command prompt (Windows):
     ``` cmd
     scp c:\path\to\local\folder\[data_product_name]\[version_number].csv myusername@boydorr.gla.ac.uk:/srv/ftp/[namespace]/[data_product][version_number].csv
     ```

3. Upload your data product
   * If it's an HDF5 file, upload it to the Boydorr server at this location:<br>
   `/srv/ftp/[namespace]/[data_product_name]/[version_number].h5`
   * If it's a TOML file, push it to the [ScottishCovidResponse/DataRepository](https://github.com/ScottishCovidResponse/DataRepository) repository on GitHub, at this location:<br>
   `/[namespace]/[data_product_name]/[version_number].toml`
