Installation:

python setup.py build
sudo python setup.py install

This will install pyFWI to the standard distribution location (/usr/local/lib/python2.6/dist-packages/) and FWI.py to a standard location on your path (/usr/local/bin/FWI.py).

Alternatively, run the code in a Docker container using this image [py3-fwi](https://hub.docker.com/r/cheewai/py3-fwi/) built from https://github.com/cynici/py3-fwi/.

Usage:


pyFWI:

import pyFWI

or

from pyFWI import *


FWI Functions:

FFMC - takes temperature, relative humidity, wind speed, rain, and a previous FFMC value to produce the current FFMC value
     - FFMC(17,42,25,0,85) = 87.692980092774448

DMC - takes temperature, relative humidity, rainfall, previous DMC value, latitude, and current month to produce the current DMC value
   - DMC(17,42,0,6,45.98,4) = 8.5450511359999997

DC - takes temperature, rain, the previous DC value, latititude, and current month to produce the current DC value
  - DC(17,0,15,45.98,4) = 19.013999999999999

ISI - takes the wind speed and current FFMC value to produce the current ISI value
   - ISI(25,87.692980092774448) = 10.853661073655068

BUI - takes the current DMC and DC values to produce the current BUI value
   - BUI(8.5450511359999997,19.013999999999999) = 8.4904265358371838

FWI - takes the current ISI and BUI values to produce the current FWI value
   - FWI(10.853661073655068,8.4904265358371838) = 10.096371392382368

calcFWI - this function returns the current FWI value given all of the input values: 
             month, temperature, relative humidity, wind speed, rain, previous FFMC, DMC, DC, and latitude
       - calcFWI(4,17,42,25,0,85,6,15,45.98) = 10.096371392382368



Lawson equations:

All of these equations take the current DMC and DC values and return moisture content as a % value

LawsonEq1 - DMC National Standard and Coastal B.C. CWH (2.5-4 cm)^2
         - LawsonEq1(8.5450511359999997)  = 250.7553985454235

LawsonEq2 - Southern Interior B.C.3 (2-4 cm)^2
         - LawsonEq2(8.5450511359999997)  = 194.93023948344205

LawsonEq3 - Southern Yukon - Pine/White Spruce
                            Feather moss, Sphagnum and Undifferentiated duff (2-4 cm)^2
         - LawsonEq3(8.5450511359999997)  = 442.82109267231488

LawsonEq4 - Southern Yukon - Pine/White Spruce
                            Reindeer lichen (2-4 cm)^2
         - LawsonEq4(8.5450511359999997)  = 746.02210402093272

LawsonEq5 - Southern Yukon - White Spruce
                            White spruce/feather moss (2-4 cm)^2
         - LawsonEq5(8.5450511359999997)  = 853.2397847094652

 

Command Line:

FWI.py is a command line script that will run the FWI calculations on a CSV file, which must follow a specific format:

date format, latitude, FFMC init value, DMC init value, DC init value
date, temperature, relative humidity, wind speed, and rain
...

The script will add the results of the calculations to the end of each line so the output format will be:

date format, latitude, FFMC init value, DMC init value, DC init value
date, temperature, relative humidity, wind speed, and rain, FFMC, DMC, DC, ISI, BUI, FWI
...

It is perfectly acceptable to modify the starting parameters of a file and rerun it without removing the calculated values, they will simply overwritten.

Date format strings are the same as the python datetime format strings.
EX) %m/%d/%Y is month/day/Year


A sample csv file has been provided, testBatch.csv, to run it:

FWI.py testBatch.csv

If everything installed correctly you will get the following as the last line in the file:

05/31/2000,18,36,5,0,88.459218921423258,16.306350982650134,112.613439577131,4.4216825593812317,23.944744397369018,7.8745783992324254
