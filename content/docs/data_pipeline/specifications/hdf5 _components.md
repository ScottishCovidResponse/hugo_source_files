---
title: HDF5 components
weight: 2
---

# HDF5 components

There are only two types of HDF5 file:

* table
* array

You should create a single HDF5 file for a single dataset.

## Single component

If your dataset contains a single component, for `create_table()` call it "`table`" and for `create_array()` call it "`array`".

## Multiple components

If your dataset contains multiple data topics / data items, for example the cases_and_management dataset contains information about nursing homes, testing, nhs24 calls, etc., then these can be included as separate components within a single HDF5 file. In the example linked above, components include:

* `call_centre/date-number_of_calls`
* `confirmed_suspected_total/date-country-hospital`
* `date-country-carehomes-carehomes_submitted_return`  

and so on.

Note the naming convention! `call_centre/date` corresponds to the first and second dimensions of the data (rows and columns), and everything after the dash applies to all elements.