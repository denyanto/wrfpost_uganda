# Read data output WRF (wrfout*)
1. Read data using netCDF4

   Create a file read1.py:
   ```console
   >> vi read1.py
   ```
   and type following instruction!
   ```console
   from netCDF4 import Dataset
   
   file_path = "wrfoutput/wrfout_d01_2020-01-01_00:00:00"  # Replace with your file path
   ncfile = Dataset(file_path)

   print(ncfile) #read all data
   print(ncfile.variables['T2']) #read temperature at 2 meters in unit Kelvin
   print(ncfile.variables['XLONG']) #read longitude
   print(ncfile.variables['XLAT']) #read latitude
   ```
   Then execute it by typing:
   ```console
   >> python read1.py
   ```
3. Read data using xarray

   Create a file read2.py:
   ```console
   >> vi read2.py
   ```
   and type following instruction!
   ```console
   import xarray as xr
   
   file_path = "wrfoutput/wrfout_d01_2020-01-01_00:00:00"  # Replace with your file path
   ncfile = xr.open_dataset(file_path)
   
   print(ncfile) #read all data
   print(ncfile.variables['T2']) #read temperature at 2 meters in unit Kelvin
   print(ncfile['T2']) #read temperature at 2 meters in unit Kelvin
   print(ncfile['XLONG']) #read longitude
   print(ncfile['XLAT']) #read latitude
   ```
   Then execute it by typing:
   ```console
   >> python read2.py
   ```
4. Read data using wrf-python

   Create a file read3.py:
   ```console
   >> vi read3.py
   ```
   and type following instruction!
   ```console
   from netCDF4 import Dataset
   import wrf
   
   file_path = "wrfoutput/wrfout_d01_2020-01-01_00:00:00"  # Replace with your file path
   ncfile = Dataset(file_path)
   temperature = wrf.getvar(ncfile, "T")  
   pressure = wrf.getvar(ncfile, "pres")    
   lats, lons = wrf.latlon_coords(temperature)
   print(ncfile) #read all data
   print(temperature) 
   print(pressure) 
   print(lats) 
   print(lons) 
   ```
   Then execute it by typing:
   ```console
   >> python read3.py
   ```
   For another variables using wrf-python, please read following link: https://wrf-python.readthedocs.io/en/latest/diagnostics.html. 
