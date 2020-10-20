---
title: Submission Script Templates
weight: 4
---

The following submission script templates are available in the `inst/templates` directory. These templates are used to upload metadata to the data registry. Depending on what you have, you need to use a different template.

| Template                                    | What do you have?                                |
| ------------------------------------------- | ------------------------------------------------ |
| [upload_estimate][1]                        | A point-estimate (but no source)                 |
| [upload_distribution][2]                    | A distribution (but no source)                   |
| [upload_multiple_parameters_in_one_toml][3] | A toml file containing multiple point-estimates and/or distributions (but no source) |
| [upload_table][4]                           | A table converted to an h5 file (but no source)  |
| [upload_array][5]                           | An array converted to an h5 file (but no source) |
| [upload_estimate_from_paper][6]             | A point-estimate taken from a paper              |
| [upload_multiple_estimates_from_paper][7]   | Multiple point-estimates taken from a paper      |
| [upload_distribution_from_paper][8]         | A distribution taken from a paper                |
| [upload_everything][9]                      | An original source (e.g. website), a raw data file (e.g. csv file), a processing script, and a data product (e.g. h5 file) |

[1]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_estimate.R
[2]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_distribution.R
[3]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_multiple_parameters_in_one_toml.R
[4]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_table.R
[5]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_array.R
[6]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_estimate_from_paper.R
[7]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_multiple_estimates_from_paper.R
[8]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_distribution_from_paper.R
[9]: https://raw.githubusercontent.com/ScottishCovidResponse/SCRCdata/master/inst/templates/upload_everything.R
