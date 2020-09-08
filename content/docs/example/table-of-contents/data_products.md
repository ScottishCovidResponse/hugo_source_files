---
title: Data Products
weight: 1
---
# Data Products

## On the data registry

Submission scripts for following datasets are available in the `inst/scripts` directory.

<table>
    <tr>
        <th>Dataset</th>
        <th>Description</th>
        <th>Updated</th>
    </tr>
    <tr>
        <td>nrs_demographics</td>
        <td>National Records of Scotland mid-2018 small area population estimates for Scotland, by single year of age, for males, females and persons, for 2011 data zones and council areas</td>
        <td>static</td>
    </tr>
    <tr>
        <td>ons_demographics</td>
        <td>Office for National Statistics small area population estimates for England and Wales, by single year of age, for males, females and persons, for 2011 output areas, 2018 electoral wards, local authority, 2011 lower/mid super output areas, local health boards (2018), 1km grids and 10km grids</td>
        <td>static</td>
    </tr>
    <tr>
        <td>scotgov_deaths</td>
        <td>Number of deaths (weekly) associated with Covid-19 and the total number of deaths registered in Scotland. Data are available for the whole of Scotland and by council area and health board. Deaths are broken down by age group (0, 1-14, 15-44, 45-64, 65-74, 75-84 and 85+) for males, females and persons</td>
        <td>Wednesday</td>
    </tr>
    <tr>
        <td>scotgov_dz_lookup</td>
        <td>Geography lookup tables used for aggregation from 2011 data zones to higher level geographies</td>
        <td></td>
    </tr>
    <tr>
    <td colspan = 3>records/SARS-CoV-2/scotland/cases-and-management/</td>
    </tr>
    <tr>
        <td>ambulance</td>
        <td></td>
        <td>static</td>
    </tr>
    <tr>
        <td>calls</td>
        <td></td>
        <td>static</td>
    </tr>
    <tr>
        <td>carehomes</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>hospital</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>mortality</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>nhsworkforce</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>schools</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>testing</td>
        <td></td>
        <td></td>
    </tr>
</table>

## Part of SCRCdata

<table>
    <tr>
        <td>ukgov_eng_oa_shapefile</td>
        <td></td>
        <td>dataSCRC</td>
    </tr>
    <tr>
        <td>ukgov_scot_dz_shapefile</td>
        <td>2011 data zone boundaries</td>
        <td>dataSCRC</td>
    </tr>
</table>

## Not yet processed

| Dataset                   | Description                                    | Uploaded |
| ---                       | ---                                            | ---      |
| scotgov_simd_income       | The Scottish Government’s official tool for identifying concentrations of deprivation in Scotland. Scottish Index of Multiple Deprivation (SIMD) 2020v2 for 2011 data zones. Ranks (1=most deprived to 6,976=least deprived) and groups (quintiles, deciles and vigintiles) are based on the income domain |  |
| scotgov_ur_classification | The Scottish Government’s 6-fold Urban Rural Classification (2016) provides a standard definition of rural areas in Scotland. Areas (2011 data zones) are classified as either large urban (1), other urban (2), accessible small towns (3), remote small towns (4), accessible rural (5) or remote rural (6)                    |          |
| ukgov_eng_lookup          |                                                |          |
| defra_pollution           |                                                |          |
|                           | Schools in Scotland                            |          |
|                           | Care home data                                 |          |
|                           | Proportion of workers in each industry sector  |          |
|                           | Death rate / birth rate                        |          |

<sup>1</sup> Cron job running on the Boydorr server every Wednesday at an arbitrarily chosen time of 15:00 

<sup>2</sup> Cron job running on the Boydorr server every day at 14:30
