# Read data output WRF (wrfout*)
1. Read data using netCDF4
   ```console
   from netCDF4 import Dataset
   file_path = "wrfoutput/wrfout_d01_2020-01-01_00:00:00"  # Replace with your file path
   ncfile = Dataset(file_path)

   print(ncfile) #read all data
   ```
3. Read data using xarray

4. Read data using wrf-python
