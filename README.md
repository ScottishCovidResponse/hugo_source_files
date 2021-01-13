# hugo_source_files

The [SCRC Docs website](https://github.com/ScottishCovidResponse/ScottishCovidResponse.github.io) contains information related to datasets, the data pipeline, and various other technical documents associated with the SCRC project.

* [ScottishCovidResponse/ScottishCovidResponse.github.io](https://github.com/ScottishCovidResponse/ScottishCovidResponse.github.io) contains website source files.

* [ScottishCovidResponse/hugo_source_files](https://github.com/ScottishCovidResponse/hugo_source_files) contains Hugo source files 
  * When pushes are made to this repository, a GitHub actions workflow automatically deploys source files to ScottishCovidResponse/ScottishCovidResponse.github.io

* [ScottishCovidResponse/hugo-book](https://github.com/ScottishCovidResponse/hugo-book) contains Hugo theme files
  * This theme is based on the Hugo [Book](https://github.com/alex-shpak/hugo-book) theme, with the search bar from the Jekyll [Just the Docs](https://github.com/pmarsceill/just-the-docs) theme, and some extra tweaks
  * Forked from [RyanJField/hugo-book](https://github.com/RyanJField/hugo-book)

## Building the site locally

It is not necessary to build the site in order to contribute changes, but if you want to see how these will be rendered, you will need to cary out the folowing steps:

1. [Install `hugo extended`](https://gohugo.io/getting-started/installing/).
2. Clone this repository `git clone https://github.com/ScottishCovidResponse/hugo_source_files.git`.
3. In the folder that contains this repository run the command `hugo server`. This will return a URL for a locally hosted site.

Changes to source files should be automatically propogated into served HTML as you work.