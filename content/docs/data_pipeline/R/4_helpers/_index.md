---
title: Helpers
weight: 4
---

# Helper functions

These helpers are mostly for internal use.

## Post entry to table

The following functions can be used to post an entry to a table in the data registry. The table schema can be viewed [here](https://data.scrc.uk/static/images/schema.svg).

<table>
<tr><th>Function</th><th>Description</th></tr>
<tr><td>`new_author()`</td><td>Post to author table</td></tr>
<tr><td>`new_code_repo_release()`</td><td>Post to author table</td></tr>
<tr><td>`new_coderun()`</td><td>Post to code_repo_release table</td></tr>
<tr><td>`new_data_product()`</td><td>Post to data_product table</td></tr>
<tr><td>`new_external_object()`</td><td>Post to external_object table</td></tr>
<tr><td>`new_issue()`</td><td>Post to issue table</td></tr>
<tr><td>`new_keyword()`</td><td>Post to keyword table</td></tr>
<tr><td>`new_namespace()`</td><td>Post to namespace table</td></tr>
<tr><td>`new_object()`</td><td>Post to object table</td></tr>
<tr><td>`new_object_component()`</td><td>Post to object_component table</td></tr>
<tr><td>`new_source()`</td><td>Post to source table</td></tr>
<tr><td>`new_storage_location()`</td><td>Post to storage_location table</td></tr>
<tr><td>`new_storage_root()`</td><td>Post to storage_root table</td></tr>
<tr><td>`new_text_file()`</td><td>Post to text_file table</td></tr>
</table>

## Dataset manipulation

<table>
<tr><th>Function</th><th>Description</th></tr>
<tr><td>`bin_ages()`</td><td>Bin ages</td></tr>
<tr><td>`convert_to_grid()`</td><td>Convert census geographies to grid based system</td></tr>
<tr><td>`convert_to_lower()`</td><td>Convert census geographies to lower resolution</td></tr>
</table>

## Get

<table>
<tr><th>Function</th><th>Description</th></tr>
<tr><td>`get_entry()`</td><td>Get entry</td></tr>
<tr><td>`get_existing()`</td><td>Get existing entries</td></tr>
<tr><td>`get_file_hash()`</td><td>Calculate hash from file</td></tr>
<tr><td>`get_github_hash()`</td><td>Get current GitHub hash</td></tr>
<tr><td>`get_package_info()`</td><td>Get GitHub package info</td></tr>
<tr><td>`get_url()`</td><td>Get URL</td></tr>
<tr><td>`get_version_numbers()`</td><td>Get data product version numbers</td></tr>
</table>

## Check

<table>
<tr><th>Function</th><th>Description</th></tr>
<tr><td>`check_exists()`</td><td>Check if entry exists</td></tr>
<tr><td>`create_version_number()`</td><td>Create version number</td></tr>
<tr><td>`increment_version()`</td><td>Increment version number</td></tr>
<tr><td>`paper_exists()`</td><td>Check whether paper exists</td></tr>
</table>

## Data pipeline

<table>
<tr><th>Function</th><th>Description</th></tr>
<tr><td>`attach_issue()`</td><td>Attach issue to object</td></tr>
<tr><td>`register_everything()`</td><td>Upload everything associated with a data product to the data registry</td></tr>
<tr><td>`upload_data_product()`</td><td>Upload data_product metadata to the data registry</td></tr>
<tr><td>`upload_github_repo()`</td><td>Upload github_repo metadata to the data registry</td></tr>
<tr><td>`upload_object_links()`</td><td>Upload object_links metadata to the data registry</td></tr>
<tr><td>`upload_paper()`</td><td>Upload paper metadata to the data registry</td></tr>
<tr><td>`upload_source_data()`</td><td>Upload source_data metadata to the data registry</td></tr>
<tr><td>`upload_submission_script()`</td><td>Upload submission_script metadata to the data registry</td></tr>
<tr><td>`upload_toml_to_github()`</td><td>Upload TOML file to GitHub</td></tr>
</table>
