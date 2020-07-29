Zip Code Lookups
================

Note: This is a fork of [davglass' zipcode lookup project](https://github.com/davglass/zipcodes).

[![Build Status](https://travis-ci.org/davglass/zipcodes.svg?branch=master)](https://travis-ci.org/davglass/zipcodes)

A localized (flatfile) zipcode lookup.

USA zip codes data was taken from here: http://federalgovernmentzipcodes.us/

Canada zip codes data was taken from here: https://www.aggdata.com/download_sample.php?file=ca_postal_codes.csv

It was then transformed into a JSON object and then wrapped with some helper methods.

> In addition to the data provided above, this fork includes county names and county codes.

This has the inclusion of county data (fips county code and county name) which was taken from here: https://www.nrcs.usda.gov/wps/portal/nrcs/detail/national/home/?cid=nrcs143_013697#
* This includes county FIPS Code to County name and state

The zip code to FIPS code data was taken from here: https://www.huduser.gov/portal/datasets/usps_crosswalk.html#data
* This includes the Zip code to FIPS County code

After combining these two datasets, I ended up with a new dataset of Zip Code, FIPS County Code, County Name, and State.

This data has been included in the existing data from the forked project to add additional detail to the zip code lookup

Usage
-----

    var zipcodes = require('zipcodes-nrviens');


Zipcode Lookup
--------------

    var motown = zipcodes.lookup(26505);

    {
        "zip": "26505",
        "latitude": 39.6505,
        "longitude": -79.944,
        "city": "Morgantown",
        "state": "WV",
        "country": "US",
        "fips": 54061,
        "county": "Monongalia"
    }

Distance
--------

This is not driving distance, it's line of sight distance


    var dist = zipcodes.distance(62959, 90210); //In Miles
    // dist = 1662

    var kilo = zipcodes.toKilometers(dist); //Convert to Kilometers
    // kilo = 2675

    var miles = zipcodes.toMiles(zipcodes.toKilometers(dist)); //Convert to Kilometers, then to miles
    // miles = 1662


Lookup By Name
--------------

*This does not work on the Canada data, the data file doesn't include this much detail.*

    var l = zipcodes.lookupByName('Cupertino', 'CA');
    
    //Always returns an array, since cities can have multiple zip codes
    [
        {
            "zip": "95014",
            "latitude": 37.318,
            "longitude": -122.0449,
            "city": "Cupertino",
            "state": "CA",
            "country": "US",
            "fips": "06085",
            "county": "Santa Clara"
        },
        {
            "zip": "95015",
            "latitude": 37.323,
            "longitude": -122.0527,
            "city": "Cupertino",
            "state": "CA",
            "country": "US",
            "fips": "06085",
            "county": "Santa Clara"
        }
    ]


Lookup by Radius
----------------

Get all zipcodes within the milage radius of this zipcode

    var rad = zipcodes.radius(95014, 50);
    // rad.length == 385

    [ '93901',
      '93902',
      '93905',
      '93906',
      '93907',
      '93912',
      '93933',
      '93942',
      '93944',
      '93950',
      ...
      '95377',
      '95378',
      '95385',
      '95387',
      '95391' 
    ]

Zipcode Random
--------------

    var zipObj = zipcodes.random();

    {
        "zip": "60944",
        "latitude": 41.0634,
        "longitude": -87.625,
        "city": "Hopkins Park",
        "state": "IL",
        "country": "US",
        "fips": 17091,
        "county": "Kankakee"
    }

Development
-----------

The original CSV file that I am using for this data is not included in this repo, but I did wrap up
the best way to get the data and how to convert it into the format that this module uses.

To develop with this module, just `make` it and it will fetch the latest zipcodes and reprocess them.

    make

To just fetch and process the zipcodes:

    make codes

To run the very simple test suite:

    make tests