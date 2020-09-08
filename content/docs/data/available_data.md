---
title: Available data
weight: 1
---

# Available data

The following data products can be found in the data registry.

## Population estimates

nrs_demographics
: National Records of Scotland mid-2018 small area population estimates for Scotland, by single year of age, for males, females and persons, for 2011 data zones and council areas. 
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `human/demographics/population/scotland`
       
ons_demographics
: Office for National Statistics small area population estimates for England and Wales, by single year of age, for males, females and persons, for 2011 output areas, 2018 electoral wards, local authority, 2011 lower/mid super output areas, local health boards (2018), 1km grids and 10km grids. 
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `data-product-name`

## Records

human-mortality
: Number of deaths (weekly) associated with Covid-19 and the total number of deaths registered in Scotland. Data are available for the whole of Scotland and by council area and health board. Deaths are broken down by age group (0, 1-14, 15-44, 45-64, 65-74, 75-84 and 85+) for males, females and persons. 
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/human-mortality`

scotgov_dz_lookup
: Geography lookup tables used for aggregation from 2011 data zones to higher level geographies 
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `geography/lookup_table/gridcell_admin_area/scotland`



ambulance
: Numbers of ambulance attendances (total and COVID-19 suspected) and number of people taken to hospital with suspected COVID-19. The number of attendances is defined as the number of incidents recorded by SAS, where a resource arrived at the scene. More information [here][cam]. No longer updated. 
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/ambulance`

calls
: Numbers of calls to NHS 111 and the coronavirus helpline. The number of calls to 111 includes all calls to this line, whether or not they relate to COVID-19. More information [here][cam]. No longer updated. 
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/calls`

carehomes
: More information [here][cam].
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/carehomes`

hospital
: The data include a snapshot of the number of people in ICUs across Scotland with suspected or confirmed COVID-19 and the number of people in hospital across Scotland with confirmed or suspected COVID-19. More information [here][cam].
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/hospital`

mortality
: The data are reported each day by Health Protection Scotland (HPS). This is a cumulative total of deaths since the start of the pandemic in Scotland. Dates shown refer to the day the figure was reported on the SG website. More information [here][cam].
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/mortality`</sub>

nhsworkforce
: NHS staff absences. The figures cover all staff recorded as absent on the system, irrespective of whether they were due to be at work on that particular day. More information [here][cam]. Uploaded weekly as of 22/7.
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/nhsworkforce`

schools
: Local authorities have agreed to the sharing of their pupil attendance and absence data on a daily basis. More information [here][cam].
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/schools`

testing
: The data shows the number of people tested for COVID-19 across Scotland to date, with a breakdown for negative and positive, the daily number of new positive cases of COVID-19 reported in Scotland, the total number of COVID-19 tests with results in the Health Protection Scotland ECOSS system reported to HPS by the laboratories in the 24 hours from 08:00 to 08:00 that day, and the total number of COVID-19 tests carried out each day, and to date by Regional Testing Centres (RTC) as part of the UK Government testing programme. More information [here][cam].
<br>
<sub>*Description last updated*: 08/09/2020</sub>  
<sub>*Data registry*: `records/SARS-CoV-2/scotland/cases-and-management/testing`

[cam]: https://statistics.gov.scot/resource?uri=http%3A%2F%2Fstatistics.gov.scot%2Fdata%2Fcoronavirus-covid-19-management-information

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
