<div>

# StrainTool

Software package to calculate strain tensor parameters

[![License MIT](http://img.shields.io/badge/license-MIT-brightgreen.svg)](https://github.com/DSOlab/StrainTool/blob/dev-danast/LICENSE)
[![](https://img.shields.io/github/release/DSOlab/StrainTool.svg)](https://github.com/DSOlab/StrainTool/releases/latest)
[![](https://img.shields.io/github/tag/DSOlab/StrainTool.svg)](https://github.com/DSOlab/StrainTool/tags) 
[![](https://img.shields.io/github/stars/DSOlab/StrainTool.svg)](https://github.com/DSOlab/StrainTool/stargazers)
[![](https://img.shields.io/github/forks/DSOlab/StrainTool.svg)](https://github.com/DSOlab/StrainTool/network)
[![](https://img.shields.io/github/issues/DSOlab/StrainTool.svg)](https://github.com/DSOlab/StrainTool/issues)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1297569.svg)](https://doi.org/10.5281/zenodo.1297569)
# General

StrainTool allows the estimation of Strain Tensor parameters, on the earth's crust, given a list of data point, aka points on the earth along with their tectonic velocities.

StrainTool has three basic components:

*   A python package, named `pystrain`
*   The main executable, named `StrainTensor.py`
*   A list of shell scripts to plot results from `StrainTensor.py`

## pystrain Package

`pystrain` is a package written in [python](https://www.python.org/), defining a variety of functions, classes, etc to enable the computation of strain tensors.

When installing this package, all of its modules will be available to the user. It is the _core_ part of the project and should be installed before the main program (`StrainTensor.py`) can be used.

The package is found under `StrainTool/pystrain`

## StrainTensor.py

This is the _main program_, i.e the program that the user will run to produce strain tensor results. It is heavily dependant on the `pystrain` package (so any change there will affect `StrainTensor.py`'s behaviour

This program does not need installation; it is just a pyrhon script. The user may only need to change priviledges to make it executable

`StrainTensor.py` is located at `StrainTool/bin`

## Plot Tools

By 'Plot Tools', we mean an array of shell ([Bash](https://www.gnu.org/software/bash/)) scripts that can be used to plot `StrainTensor.py` results. These scripts actualy use [GMT](http://gmt.soest.hawaii.edu/) to perform the plotting. The scripts are an efficient, ready-to-use

It is by no means mandatory to use these scripts to plot the results; users can use their own tools and/or scripts to do so. Actualy, this is a totaly independent part of the Project and can be skipped alltogether.

These scripts, are found under `StrainTool/plot`

# Installation

## Prerequisites

To install the Project, you will need:

*   [python](https://www.python.org/downloads/); the installation has been tested under Versions 2.7 and 3.6 and both work fine.
*   [NumPy](http://www.numpy.org/); NumPy is the fundamental package for scientific computing with Python.
*   [SciPy](https://www.scipy.org/); SciPy is a Python-based ecosystem of open-source software for mathematics, science, and engineering. Actualy, we are only using the [SciPy.linalg](https://docs.scipy.org/doc/scipy/reference/linalg.html) and [SciPy.spatial](https://docs.scipy.org/doc/scipy/reference/spatial.html) modules.

Both `NumPy` and `SciPy` have extended installation guides for most, if not all, operating systems.

To use the [Plot Tools](#plot_tools_pck) bundle, you must have [GMT](http://gmt.soest.hawaii.edu/) (> 5.0.0) installed, allong with a variety of UNIX-based tools. If you are on a UNIX/Linux operating system, most or all of them are pre-installed.

## Installation Procedure

To install the Project, you actualy only need to install the [pystrain](pystrain_pkg) package. What this means, is that python will see that the package modules are build and extracted to a suitable folder on your local system (something like `/usr/lib/python2.7/site-packages/` on a UNIX-like system). After this step, you will be able to make use of the modules in the package, both for running `StrainTool.py` and for any new program you may wish to code. [StrainTensor.py](#straintensor_prg) and [Plot Tools](#plot_tools_pck) are programs, so you do not to "install" them.

To install [pystrain](#pystrain_pkg), do the following:

1.  go to the directory the package is located, i.e. `StrainTool/pystrain`
2.  the package comes with a `setup.py` script, so you only need to run it (as root/administrator):

        $> python setup.py install

That's it! The package modules should now be in place and you should be able to use the main script, [StrainTensor.py](#straintensor_prg)

## Example

To validate that the installation has succeded, you can run a "test case" scenario. Under the folder `data/`, you will find a file named `CNRS_midas.vel`. This ascii file, contains velocity and coordinate information for a large list of GPS/GNSS stations around the globe. We will use it to perform a test run of `StrainTensor.py`.

Go to the `bin/` folder and execute the command:

    $> ./StrainTensor.py -i ../data/CNRS_midas.vel -r 18.75/30.25/32.75/42.25 --x-grid-step=0.5 --y-grid-step=0.5 --dmin=1 --dmax=500 --dstep=1 --Wt=24 -c

_(You may not need the ./ part at the begining if you are on a Wnidows system)_. This command will compute Strain Tensor parameters for approximately the whole of Greece, using the input file `CNRS_midas.vel`. The results will be written in a (newly created) file, named `strain_info.dat`, while station information will be written in the file `station_info.dat` (this latter file is mainly used for plotting). You should be able to verify that both files have been succesefuly created.

Under `data/` you will find the "reference" output files for checking, named `strain_info.dat.ref` and `station_info.dat.ref`. Verify that the files you have just created (placed under `bin/`) contain the same results as the "reference" files (this can be very easily performed using the [diff](https://www.gnu.org/software/diffutils/) command).

### TODO: plot example files and compare

# How to use StrainTensor.py

`StrainTensor.py` computes strain tensor parameters on a selcted grid. The program comes with an extended help message, that can be displayed via the <kbd>-h</kbd> or <kbd>--help</kbd> switch (aka

    $> ./StrainTensor.py [...] --help

will show the help message on screen). Users can controll `StrainTensor.py`'s behaviour via a variety of available switches.

## Basic Usage

To use the program, the only mandatory switch is the <kbd>-i</kbd> or <kbd>--input-file</kbd>, which specifies the input file to be used. All other options will automaticaly assume default values (see [Options](#straintool_prg_opt) for a detailed list). Basic usage is described below:

<pre id="block-samp"><samp>usage: StrainTensor.py [-h] -i INPUT_FILE [--x-grid-step X_GRID_STEP]
                       [--y-grid-step Y_GRID_STEP] [-m METHOD] [-r REGION]
                       [-c] [-b] [--max-beta-angle MAX_BETA_ANGLE]
                       [-t WEIGHTING_FUNCTION] [--Wt Wt] [--dmin D_MIN]
                       [--dmax D_MAX] [--dstep D_STEP] [--d-param D_PARAMETER]<samp></samp></samp></pre>

## Options

The whole list of available options, is:

<pre id="block-samp"><samp>optional arguments:
  -h, --help            show this help message and exit
  -i INPUT_FILE, --input-file INPUT_FILE
                        The input file. This must be an ascii file containing the columns: 'station-name longtitude latitude Ve Vn SigmaVe SigmaVn Sne time-span'. Longtitude and latitude must be given in decimal degrees; velocities (in east and north components) in mm/yr. Columns should be seperated by whitespaces. Note that at his point the last two columns (aka Sne and time-span) are not used, so they could have random values.
  --x-grid-step X_GRID_STEP
                        The x-axis grid step size in degrees. This option is only relevant if the program computes more than one strain tensors. Default is 0.5(deg).
  --y-grid-step Y_GRID_STEP
                        The y-axis grid step size in degrees. This option is only relevant if the program computes more than one strain tensors. Default is 0.5(deg)
  -m METHOD, --method METHOD
                        Choose a method for strain estimation. If 'shen' is passed in, the estimation will follow the algorithm described in Shen et al, 2015, using a weighted least squares approach. If 'veis' is passed in, then the region is going to be split into delaneuy triangles and a strain estimated in each barycenter. Default is 'shen'.
  -r REGION, --region REGION
                        Specify a region; any station (in the input file) falling outside will be ommited. The region should be given as a rectangle, specifying min/max values in longtitude and latitude (using decimal degrees). E.g. "[...] --region=21.0/23.5/36.0/38.5 [...]"
  -c, --cut-excess-stations
                        This option is only considered if the '-r' option is set. If this this option is enabled, then any station (from the input file) outside the region limit (passed in via the '-r' option) is not considered in the strain estimation.
  -b, --barycenter      Only estimate one strain tensor, at the region's barycentre.
  --max-beta-angle MAX_BETA_ANGLE
                        Only relevant for '--mehod=shen'. Before estimating a tensor, the angles between consecutive points are computed. If the max angle is larger than max_beta_angle (in degrees), then the point is ommited (aka no tensor is computed). This option is used to exclude points from the computation tha only have limited geometric coverage (e.g. the edges of the grid). Default is 180 deg.
  -t WEIGHTING_FUNCTION, --weighting-function WEIGHTING_FUNCTION
                        Only relevant for '--mehod=shen'. Choose between a 'gaussian' or a 'quadratic' spatial weighting function. Default is 'gaussian'.
  --Wt Wt               Only relevant for '--mehod=shen' and if 'd-param' is not passed in. Let W=Σ_i*G_i, the total reweighting coefficients of the data, and let Wt be the threshold of W. For a given Wt, the smoothing constant D is determined by Wd=Wt . It should be noted that W is a function of the interpolation coordinate, therefore for the same Wt assigned, D varies spatially based on the in situ data strength; that is, the denser the local data array is, the smaller is D, and vice versa. Default is Wt=24.
  --dmin D_MIN          Only relevant for '--mehod=shen' and if 'd-param' is not passed in. This is the lower limit for searching for an optimal d-param value. Unit is km. Default is dmin=1km.
  --dmax D_MAX          Only relevant for '--mehod=shen' and if 'd-param' is not passed in. This is the upper limit for searching for an optimal d-param value. Unit is km. Default is dmax=500km.
  --dstep D_STEP        Only relevant for '--mehod=shen' and if 'd-param' is not passed in. This is the step size for searching for an optimal d-param value. Unit is km. Default is dstep=2km.
  --d-param D_PARAMETER
                        Only relevant for '--mehod=shen'. This is the 'D' parameter for computing the spatial weights. If this option is used, then the parameters: dmin, dmax, dstep and Wt are not used.</samp></pre>

For example, the command we used on the [Example](#straintensor_prg_example) section:

    $> ./StrainTensor.py -i ../data/CNRS_midas.vel -r 18.75/30.25/32.75/42.25 --x-grid-step=0.5 --y-grid-step=0.5 --dmin=1 --dmax=500 --dstep=1 --Wt=24 -c

meant that we are estimating strain tensor parameters for the region within min/max longtitude: 18.75/30.25 degrees and min/max latitude: 32.75/42.25 degrees. We are seperating the region into cells of size 0.5 degrees and estimating one strain tensor in each cell centre (aka the first cell centre is $lon = lon_{min} + lon_{step}/2 = 18.75 + 0.5/2=19.0deg.$ and $lat = lat_{min} + lat_{step}/2 = 32.75 + 0.5/2 = 33.0deg.$, then next one will be at lon=19.5, lat=33.5, etc...). To estimate each strain tensor, the program will search for a "optimal" D coefficient within the range [1, 500) km with a step of 1km. This "optimal" D will be found when the condition $W = \sum_{n=1}^{\#sta*2} Z(i)*L(i) \geq W_t$ is met. The stations that will be used for the calculations are only those that fall within the specified longtitude/latitude range. For more information, see [Background and Algorithms](#bck_and_algorithms) section.

## Input Files

To perform the computations, `StrainTensor.py` needs an input file, that holds input data. Usually, this implies a list of GPS/GNSS stations with their ellipsoidal coordinates (aka longtitude and latitude) and their respective tectonic velocities (usually estimated using position time-series) allong with the corresponding standard deviation values. The format of these files, should follow the convention:

<pre id="block-samp" <samp="">        station-name  longtitude   latitude   Ve     Vn    SigmaVe  SigmaVn  Sne  time-span 
         string           deg.        deg.   mm/yr  mm/yr   mm/yr    mm/yr    /   dec. years</pre>

Station coordinates are provided in longtitude/latitude pairs in decimal degrees. Velocities and velocity standard deviations are provided in mm per years (mm/yr). `Sne` is the correlation coefficient between East and North velocity components and `time-span` is the total time span of the station timeseries in decimal degrees. _Note that at his point the last two columns (aka `Sne` and `time-span`) are not used, so they could have random values._

There are no strict formating rules on how the individual elements should be printed (i.e. how many fields, decimal places, etc). The only condition is that fields are seperated by whitespace(s). To see an example of a valid input file, you can check `data/CNRS_midas.vel`.

## Output Files

Results of `StrainTensor.py` are recorded in the following three files:

*   **strain_info.dat :** This file includes strain tensor parameters, principal axis, rotational rates, dilatation etc.  
    The columns of the file are structured as below:

<pre id="block-samp" <samp="">Latitude  Longtitude     vx+dvx          vy+dvy           w+dw          exx+dexx        exy+dexy        eyy+deyy       emax+demax      emin+demin       shr+dshr        azi+dazi      dilat+ddilat   sec. invariant 
   deg       deg         mm/yr           mm/yr          nrad/yr       nstrain/yr      nstrain/yr      nstrain/yr      nstrain/yr      nstrain/yr      nstrain/yr         deg.         nstrain/yr      nstrain/yr   
	        </pre>

*   **station_info.dat :** Stations' data used for the calculation of strain tensor are written at htis file. Format is:

<pre id="block-samp" <samp="">Code Longtitude Latitude Ve Vn
                </pre>

*   **StrainTensor.out :** Parameters used to modigy grid, calculate Strain tensors etc.

# How to use Plot Tools

Plotting scripts are placed under `plot/` directory. They are:

*   `gmtstrainplot.sh`, and
*   `plotall.sh`

All of the scripts need the file named `default-param` to be in the same folder.**default-param file options**

<pre id="block-samp" <samp="">        pth2inptf=../data/  # set default folder for input files (strain_info.dat, station_info.dat)
	west, east, south, north, projscale, frame, sclength: set region parameters
        STRSC=0.01 : set principal axis plot scale
        ROTSC=.7 : set rotational rates plot scale
        </pre>

**gmtstrainplot.sh options:**

<pre id="block-samp" <samp="">/*** Basic Plots & Background ***********************************/
     -r | --region : region to plot (default Greece)
         usage: -r west east south north projscale frame

/*** PLOT STATIONS **********************************************/
     -psta [:=stations] plot only stations from input file

/*** PLOT VELOCITIES ********************************************/
     -vhor (station_file)[:= horizontal velocities]
     -vsc [:=velocity scale] change valocity scale default 0.05

/*** PLOT STRAIN PARAMETERS *************************************/
     -str (strain file)[:= strains] Plot strain rates
     -rot (strain file)[:= rots] Plot rotational rates
     -gtot(strain file)[:=shear strain] plot total shear strain rate contours
     -gtotaxes (strain file) dextral and sinistral max shear strain rates
     -dil (strainfile)[:= dilatation] Plot dilatation and principal axes
     -secinv (strain file) [:=2nd invariand] Plot second invariand
     -strsc [:=strain scale]
     -rotsc [:=rotational scales]

/*** OTHER OPRTIONS *********************************************/
     -o | --output : name of output files
     -l | --labels : plot labels
     -jpg : convert eps file to jpg
     -h | --help : help menu
 Exit Status:    1 -> help message or error
 Exit Status:  = 0 -> sucesseful exit
/***************************************************************/
        </pre>

For exanple, to plot the principal axis fo strain rates for the example case above you can use the following command:

<pre id="block-samp" <samp="">$>./gmtstrainplot.sh -jpg -str strain_info.dat -psta -l
        </pre>

# TODO: Background and Algorithms

</div>

<div class="container">

## Strain Weighting Functions - Demo

<div id="chart_div">

<canvas id="myChart"></canvas>

</div>

<div id="dparam_div">Parameter D: <input type="number" id="dparam" value="15"> <button onclick="submit_data();">Try it</button>

<div>

### Weighting Functions

The weighting functions are computed as follows: Gaussian Weighting Function: $L_i = \exp(-{\Delta}R_i^{2} / D^{2})$ Quadratic Weighting Function: $L_i = 1/(1 + {\Delta}R_i^{2} / D^{2})$

</div>

</div>

</div>

<div class="container">

## Azimouthal Weighting Function - Demo

<div id="chart_div2">

<canvas id="myChart2"></canvas>

</div>

</div>

# License

# Authors & Bug Reports
