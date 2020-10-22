---
title: Submission script templates
weight: 5
---

# Submission script templates

Submission scripts are used to upload metadata to the data registry. A number of submission script templates are available in the SCRCdata package. To access them either clone the [repository](https://github.com/ScottishCovidResponse/SCRCdata) and go to the `inst/templates` directory, or just follow the links on this page.

## Five stars, great job!

Use these scripts if you have a single original source (*e.g.* a website or database), a single raw data file (*e.g.* a csv file), a single processing script (to convert the raw data file into an h5/hdf5 file), and a single data product (*e.g.* an h5/hdf5 file).

[upload_everything2][10]
: You have everything mentioned above.

[upload_everything][9]
: You have everything mentioned above. This is an unwrapped version of `upload_everything2` with more scope for tweaking.

[upload_distribution_from_paper][8]
: You have a distribution (raw data) taken from a paper (original source). You can generate the toml file (data product) here so you don't need a processing script.

[upload_multiple_estimates_from_paper][7]
: You have multiple point-estimates (raw data) taken from a paper (original source). You can generate the toml file (data product) here so you don't need a processing script.

[upload_estimate_from_paper][6]
: You have a point-estimate (raw data) taken from a paper (original source). You can generate the toml file (data product) here so you don't need a processing script.

:exclamation: If you have a more complicated situation, *e.g.* [multiple original sources](https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/SCRC/scotgov_dz_lookup.R), please seek guidance.

## One star, needs more work...
Use these scripts if you don't have an original source or a processing script and want to upload a data product anyway. Bad!

[upload_array][5]
: You have an array converted to an h5 file (data product), but no original source, no raw data, and no processing script.

[upload_table][4]
: You have a table converted to an h5 file (data product), but no original source, no raw data, and no processing script.

[upload_multiple_parameters_in_one_toml][3]
: You have a toml file containing multiple point-estimates and/or distributions (data product), but no original source, no raw data, and no processing script.

[upload_distribution][2]
: You have a distribution (that you made up in your head) and you want to create a toml file (data product). You have no original source, no raw data, and no processing script.

[upload_estimate][1]
: You have a point-estimate (that you made up in your head) and you want to create a toml file (data product). You have no original source, no raw data, and no processing script.

[1]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_estimate.R
[2]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_distribution.R
[3]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_multiple_parameters_in_one_toml.R
[4]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_table.R
[5]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_array.R
[6]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_estimate_from_paper.R
[7]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_multiple_estimates_from_paper.R
[8]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_distribution_from_paper.R
[9]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_everything.R
[10]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_everything2.R