CERA Matplotlib Contouring for ADCIRC NetCDF Data
============================================ 

This script uses the python library matplotlib (http://matplotlib.org/) to create and plot contours from a single ADCIRC netcdf file. If multiple time steps are given in the ADCIRC netcdf file, the first time step will be extracted.

Copyright (C): Carola Kaiser 2014-2023, Louisiana State University.
With special thanks to Ian Thomas from the matplotlib developer team for helping us to create clean geometries.

This script is part of the Coastal Emergency Risks Assessment (**CERA**) software package, a real-time visualization system for ADCIRC storm surge guidance. See https://cera.coastalrisk.live.

CERA is Open Source software; distributed under the Boost Software License, Version 1.0. (See accompanying file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)


## Description

The Coastal Emergency Risks Assessment (CERA) team provides storm surge and wave guidance for the Northern Gulf and the Atlantic Coast through its online portal (https://cera.coastalrisk.live).

We use the Python Matplotlib library (http://matplotlib.org/) to convert the ADCIRC NetCDF format into contours which can then be used to generate several GIS file formats and/or map plots. While working on the algorithms, the CERA team has discovered some specifics which are essential to produce clean geometries. Clean geometries in GIS terminology do not include any self-intersections, overlapping, or duplicate features and ensure the hassle-free usability for GIS specialists and emergency managers.

Here are the specifics and bug fixes that we have addressed:

1. Matplotlib 'tricontourf' expects a data array, but does not support masked arrays. If you pass a masked array, it will be ignored. The triangulation should only contain triangles with valid data at all three vertices. The solution is to either remove invalid triangles from your 'element' array before creating the triangulation, or set a mask on the triangulation once it has been created. 
2. The created contours will contain one or more polygon exteriors and zero or more interiors. They can be in any order (an exterior is not necessarily followed by its interiors). This has to be explicitly tested in your own script. The CERA code takes care of this issue.

## System Requirements

* Python 3
* numpy (http://docs.scipy.org/doc/numpy/user/install.html)
* netCDF4-python and requiered dependencies 
  (https://github.com/Unidata/netcdf4-python/blob/master/README.md)
* matplotlib (http://matplotlib.org/users/installing.html)
* Shapely (https://pypi.python.org/pypi/Shapely)

## Usage

cera_contour_matplotlib.py -i (input datafile) -a (netcdf attribute name) -g (mesh file) -n (intervals) -m (maxlevel)

mandatory:

      -i | infile       name of the NetCDF input file containing the ADCIRC data
      -a | attrname     attribute name of the NetCDF data array
      
      The option -g is only required if the ADCIRC mesh information is not included in the data 
      input file.
      -g | grid         name of the NetCDF file containing the ADCIRC mesh (default: input file)

optional:

      -n | intervals    number of contour intervals in output file (default:30)
      -m | maxlevel     maximum data value to be used for contouring (default:highest value 
                        in data array)

## Example usage

Download the test data file (maxele.63.nc) in your script directory.

[1] cera_contour_matplotlib.py -i maxele.63.nc -a zeta_max
![Example 1](https://github.com/CERA-GROUP/cera_contour/blob/master/example_1_plot.png)

In this example, no optional parameters (-n intervals or -m maxlevel) are specified. The default values for these options will be applied. The maxlevel uses always the original units from the input file (here: meters). The output contours will be classified as 30 levels and stretched between zero and and the maximum data value in the input file (here: 10.62 meters).

[2] cera_contour_matplotlib.py -i maxele.63.nc -a zeta_max -m 2
![Example 2](https://github.com/CERA-GROUP/cera_contour/blob/master/example_2_plot.png)

In this example, the maxlevel is given with the option -m as 2 (meters). The output contours will be classified as 
30 levels (default value for the missing option -n (intervals). The levels will be limited to 0-2m. 

[3] cera_contour_matplotlib.py -i maxele.63.nc -a zeta_max -n 10 -m 1
![Example 3](https://github.com/CERA-GROUP/cera_contour/blob/master/example_3_plot.png)

In this example, the output contours will be classified as 10 levels (option -n intervals) between 0-1m 
(option -m maxlevel). 

=======================================
If you have any questions, please contact:
Carola Kaiser, email: ckaiser@cct.lsu.edu

CERA: https://cera.coastalrisk.live

 
