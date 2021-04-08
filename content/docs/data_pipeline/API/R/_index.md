---
bookCollapseSection: true
weight: 10
---

# SCRCdataAPI

The `SCRCdataAPI` package contains functions used to interface with the SCRC data pipeline (*e.g.* to upload metadata to the registry or download data products from the Boydorr server) in R.

To install it, run:

``` R
install.packages("devtools")
devtools::install_github("scottishcovidresponse/SCRCdataAPI")
```

To view the package documentation, go [here](https://scottishcovidresponse.github.io/SCRCdataAPI/index.html).

# SCRCdata

The `SCRCdata` package includes submission script templates that can be used to interact with the SCRC data pipeline. These can be accessed by cloning the repo and going to `inst/templates` or by browsing the GitHub [repository](https://github.com/ScottishCovidResponse/SCRCdata/tree/master/inst/templates).

The `SCRCdata` package contains submission scripts and functions used to process specific datasets and upload their metadata to the SCRC data pipeline. If you haven't added code to this package, you likely don't need to install it. If you have added code to this package, please ensure the data registry has been updated appropriately.

To install it (assuming `devtools` is already installed), run:

``` R
devtools::install_github("scottishcovidresponse/SCRCdata")
```
