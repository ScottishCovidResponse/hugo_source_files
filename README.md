# hugo_source_files

The [SCRC Docs website](https://github.com/ScottishCovidResponse/ScottishCovidResponse.github.io) contains information related to datasets, the data pipeline, and various other technical documents associated with the SCRC project.

* [ScottishCovidResponse/ScottishCovidResponse.github.io](https://github.com/ScottishCovidResponse/ScottishCovidResponse.github.io) contains website source files

* [ScottishCovidResponse/hugo_source_files](https://github.com/ScottishCovidResponse/hugo_source_files) contains Hugo source files 
  * When pushes are made to this repository, a GitHub actions workflow automatically deploys source files to ScottishCovidResponse/ScottishCovidResponse.github.io

* [ScottishCovidResponse/hugo-book](https://github.com/ScottishCovidResponse/hugo-book) contains Hugo theme files
  * This theme is based on the Hugo [Book](https://github.com/alex-shpak/hugo-book) theme, with the search bar from the Jekyll [Just the Docs](https://github.com/pmarsceill/just-the-docs) theme, and some extra tweaks
  * Forked from [RyanJField/hugo-book](https://github.com/RyanJField/hugo-book)

## Building the site locally

1. [Install `hugo extended`](https://gohugo.io/getting-started/installing/).
2. Clone this repository `git clone https://github.com/ScottishCovidResponse/hugo_source_files.git`.
3. In the folder that contains this repository run the command `hugo server`. This will return a URL for a locally hosted site e.g.:
   
```
PS C:\Users\Bob Turner\Documents\hugo_source_files> hugo server
Start building sites … 

                   | EN  
-------------------+-----
  Pages            | 37
  Paginator pages  |  0
  Non-page files   |  3
  Static files     | 82
  Processed images |  0
  Aliases          |  2
  Sitemaps         |  1
  Cleaned          |  0

Built in 182 ms
Watching for changes in C:\Users\Bob Turner\Documents\hugo_source_files\{archetypes,content,themes}
Watching for config changes in C:\Users\Bob Turner\Documents\hugo_source_files\config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

Changes to source files should be automatically propogated into served HTML as you work.