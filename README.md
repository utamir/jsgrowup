========
jsgrowup
========

jsgrowup is based on pygrowup by Evan Wheeler:
https://github.com/ewheeler/pygrowup

jsgrowup calculates z-scores for the following anthropometric indicators:

* weight-for-age

* length/height-for-age

* weight-for-length/height

* head-circumference-for-age

* body-mass-index-for-age

based on the WHO Child Growth Standards:
* http://www.who.int/childgrowth/standards/en/
* http://www.who.int/entity/childgrowth/standards/technical_report/en/index.html

and can optionally use CDC growth standards:
* http://www.cdc.gov/growthcharts

jsgrowup avoids floating-point operations to eliminate the unwanted rounding
that muddles the precision of some of the igrowup implementations:
* http://docs.sun.com/source/806-3568/ncg_goldberg.html


INSTALLATION
============
`npm install jsgrowup`


EXAMPLE USAGE
=============

index.js includes a sample zscore lookup.


EXCEPTIONS
==========

caller should watch for:

* `AssertionError` raised when caller provides inappropriate parameters

as well as more specific errors (all subclasses of `RuntimeError`):

* `InvalidMeasurement` raised when measurement is invalid for requested indicator

* `InvalidAge` raised when age is invalid for requested indicator

* `DataNotFound` raised when WHO/CDC data is not found for the requested observation (e.g., box-cox, median, coefficient of variance for age)

* `DataError` raised when an error occurs while loading WHO/CDC data into memory


TESTING
=======

simple test script, not using any framework:
`node tests.js`

the included tests use example anthropometric data taken from
demonstration data shipped with WHO's igrowup software.
jsgrowup performs the same calculations and compares the results
to the WHO results.
please see the sofware licence agreement for WHO's igrowup, which
is the source of the test data files:
http://www.who.int/childgrowth/software/license2.pdf

currently, 3 cases fail to produce results within 1 standard deviation
of the WHO resuts. I believe these discrepencies are due to WHO's use
of floating point arithmetic in their igrowup software, which leads to less
precise calculations compared to jsgrowup. In the absence of any other
trusted test data, please be aware that no claims are made to the
accuracy or reliability of jsgrowup's calculations.


DEVELOPING
==========

The source WHO .txt tables can be easily converted to json with the help of
two amazing python utilities:

* The Pyed Piper https://code.google.com/p/pyp/

* csvkit http://pypi.python.org/pypi/csvkit

heres an example one-liner that changes the source .txt from tsv
to csv (with `pyp`) and then to json (with csvkit's `csvjson`)
`$ cat bmi_girls_2_5_zscores.txt | pyp "p.replace('\t', ',')" | csvjson > bmifa_girls_2_5_zscores.json`