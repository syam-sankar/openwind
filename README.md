# README  

OpenWind depends on Nansat (https://github.com/nansencenter/nansat). 

# Installation

- Add the top level folder openwind/ to your PYTHONPATH
- Add the subfolder openwind/openwind/ to your PATH (for command line usage)

# Command line usage:

```
./sar_wind.py -s SAR_image -w winddir -f figure_filename -n netCDF_filename -p pixelsize
```

- SAR_image is a file readable by Nansat, containing NRCS in VV polarisation

- winddir is either:
  - a file readable by Nansat, containing wind direction (U10 and V10)
  - an integer indicating constant wind direction (0 from North, 90 from East etc)
  - None or omitted [DEFAULT]: NCEP GFS model wind is downloaded from NCEP NOMADS, if available for the time of the SAR image

- pixelsize is given in meters (500 m by default). Use pixelsize='fullres' for no resizing (usually not recommended).

To see explanation of all features:
```
./sar_wind.py -h
```


# Python usage:
```
>>> from openwind import SARwind

>>> s = SARwind(SAR_image, winddir, pixelsize)
```

See above (command line usage) for specification of the input parameters.


To plot the result and save as figure:
```
>>> plt = s.plot() # to plot SAR wind overlaid wind vectors.

>>> plt.savefig(filename, bbox_inches='tight', dpi=300) # Save to file
```

If winddir is not specified when the SARWind object is generated with the Python API, wind speed is automatically calculated using ncep model wind downloaded from NCEP NOMADS. You can modify the SAR image (e.g. resizing or cropping) before calling SARWind as long as the SAR image file is not a previously exported reprojected/resized/cropped image:
```
>>> from openwind import SARWind

>>> from nansat import Nansat

>>> n = Nansat(SAR_image)

>>> n.crop(lonlim=[5, 6], latlim=[60, 61])

>>> s = SARWind(n)
```

Note that this is slower than first calculating the wind, then doing the modifications.


# Notes:
- GDAL might need to be compiled with the option --with-jasper to be able to read the downloaded NCEP GFS GRIB2-files

# Acknowledgments

Thanks to the Royal Netherlands Meteorological Institute
(http://www.knmi.nl/scatterometer/cmod5/) for making the cmod5 software
available.

# References

Hersbach, H., Comparison of C-Band Scatterometer CMOD5.N Equivalent Neutral
Winds with ECMWF J. Atm. Oceanic Technol., 2010, 27, 721-736,
doi:10.1175/2009JTECHO698.1.

Portabella, M. and A.C.M. Stoffelen, On Scatterometer Ocean Stress J. Atm.
Oceanic Technol., 2009, 26, 2, 368-382, doi:10.1175/2008JTECHO578.1.

Verhoef, A., M. Portabella, A. Stoffelen and H. Hersbach, CMOD5.n - the CMOD5
GMF for neutral winds Document external project: 2008,
SAF/OSI/CDOP/KNMI/TEC/TN/165, EUMETSAT, 2008.

Hersbach, H., A. Stoffelen and S. de Haan, An Improved C-band scatterometer
ocean geophysical model function: CMOD5 J. Geophys. Res., 2007, 112, C03006,
doi:10.1029/2006JC003743.

Portabella, M., Wind field retrieval from satellite radar systems. Thesis:
University of Barcelona, 2002, Barcelona, Spain, 207p.

Haan, S. de and A. Stoffelen, CMOD5 Document external project: 2001,
SAF/OSI/KNMI/TEC/TN/140, EUMETSAT, 2001.
