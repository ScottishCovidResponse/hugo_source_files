---
title: Helpers
weight: 4
---

# Helper functions

These helpers are mostly for internal use.

Detailed documentation and examples can be found [here](https://scottishcovidresponse.github.io/SCRCdataAPI/reference/index.html).

## Pipeline helpers

`attach_issue()`
: Attach an issue to an object in the data registry

`check_exists()`
: Check if entry exists in the data registry

`create_version_number()`
: Create version number

`get_entry()`
: Return all fields associated with a table entry in the data registry

`get_existing()`
: Return all entries posted to a table in the data registry

`get_file_hash()`
: Calculate hash from file

`get_github_hash()`
: Get current GitHub hash

`get_package_info()`
: Get GitHub package info

`get_url()`
: Get URL

`get_version_numbers()`
: Get data product version numbers

`increment_version()`
: Increment version number

`paper_exists()`
: Check whether paper exists

`upload_toml_to_github()`
: Upload TOML file to GitHub

## Post new entry to table

The following functions can be used to post an entry to a table in the data registry. 

The database schema can be viewed [here](https://data.scrc.uk/static/images/schema.svg).

`new_author()`
: Post entry to author table

`new_code_repo_release()`
: Post entry to author table

`new_coderun()`
: Post entry to code_repo_release table

`new_data_product()`
: Post entry to data_product table

`new_external_object()`
: Post entry to external_object table

`new_issue()`
: Post entry to issue table

`new_keyword()`
: Post entry to keyword table

`new_namespace()`
: Post entry to namespace table

`new_object()`
: Post entry to object table

`new_object_component()`
: Post entry to object_component table

`new_source()`
: Post entry to source table

`new_storage_location()`
: Post entry to storage_location table

`new_storage_root()`
: Post entry to storage_root table

`new_text_file()`
: Post entry to text_file table

## Wrapper functions

The following functions combine the `new_*()` functions to post entries across multiple tables in the data registry.

`upload_data_product()`
: Post data_product metadata to the data registry

`upload_github_repo()`
: Post github_repo metadata to the data registry

`upload_object_links()`
: Post object_links metadata to the data registry

`upload_paper()`
: Post paper metadata to the data registry

`upload_source_data()`
: Post source_data metadata to the data registry

`upload_submission_script()`
: Post submission_script metadata to the data registry

The following function combines the `upload_*()` functions to post entries across all tables in the data registry associated with a particular data product.

`register_everything()`
: Post everything associated with a data product to the data registry

## Dataset manipulation

`bin_ages()`
: Bin ages

`convert_to_grid()`
: Convert census geographies to grid based system

`convert_to_lower()`
: Convert census geographies to lower resolution
