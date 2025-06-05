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
   ```
   Then execute it by typing:
   ```console
   >> python read1.py
   ```
3. Read data using xarray

4. Read data using wrf-python
