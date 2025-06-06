# Simple plotting example
1. Sea level Pressure
   ![plot1](https://github.com/user-attachments/assets/1cfd8627-73d0-401d-9879-f063caf8c4de)

   Create a file plot1.py with this example:
   ```console
   import netCDF4
   import wrf
   import matplotlib.pyplot as plt
   import cartopy.crs as ccrs
   import numpy as np
  
   # Load the NetCDF file using netCDF4
   ncfile = netCDF4.Dataset('wrfoutput/wrfout_d01_2020-01-01_00:00:00')
  
   # Extract a variable (e.g., sea level pressure, 'slp')
   slp = wrf.getvar(ncfile, 'slp')
  
   # Get lat/lon coordinates and map projection
   lats, lons = wrf.latlon_coords(slp)
   cart_proj = wrf.get_cartopy(slp)
  
   # Create the plot
   fig = plt.figure(figsize=(10, 8))
   ax = plt.axes(projection=cart_proj)
  
   # Plot the variable
   plt.contourf(lons, lats, slp, transform=ccrs.PlateCarree(), cmap='jet')
   cbar = plt.colorbar()

   # Add coastlines, gridlines, and title
   ax.coastlines()
   gl=ax.gridlines(crs=ccrs.PlateCarree(),draw_labels=True,color='black',alpha=0.5,linestyle='--')
   plt.title('Sea Level Pressure (SLP)')
  
   # Save the plot
   plt.savefig('plot1.png')
  
   # Close the file
   ncfile.close()
   ```
2. 
3.
4. Wind Speed
   ![plot1](https://github.com/user-attachments/assets/e0977195-3bf2-4cee-8798-78336405fef0)

   Create a file plot5.py with this example:
   ```console
   from netCDF4 import Dataset
   import matplotlib.pyplot as plt
   import cartopy.crs as ccrs
   from wrf import (getvar, interplevel, to_np, latlon_coords)
   import numpy as np
   # read the file
   path_file='wrfoutput/wrfout_d01_2020-01-01_00:00:00'
   ncfile = Dataset(path_file)
   # select your timestep (index) and pressure level
   i = 0
   p_level = 500
   # get the variables
   p = getvar(ncfile, "pressure", timeidx=i)
   ua = getvar(ncfile, "ua", units="kt", timeidx=i)
   va = getvar(ncfile, "va", units="kt", timeidx=i)
   # interpolate ua and va to pressure level
   u_500 = interplevel(ua, p, p_level)
   v_500 = interplevel(va, p, p_level)
   
   # get the lat, lon grid
   lats, lons = latlon_coords(u_500)
   # specify your colormap and projection
   cmap = plt.get_cmap('Reds')
   crs = ccrs.PlateCarree()
   # plot
   fig = plt.figure(figsize=(10,6))    
   ax = fig.add_subplot(111, facecolor='None', projection=crs)
   ax.coastlines(resolution='10m', alpha=0.5)
   plot_uv500 = ax.pcolormesh(lons, lats, np.sqrt(u_500**2+v_500**2), cmap=cmap)
   cbar = fig.colorbar(plot_uv500)
   cbar.ax.set_ylabel('Wind speed (kts)')
   # some fancy schmancy grid lines
   gl = ax.gridlines(crs=crs, draw_labels=True, alpha=0.5)
   
   plt.title('Wind Speed 500mb')
   
   # Save the plot
   plt.savefig('plot5.png')
   
   # Close the file
   ncfile.close()
   ```
