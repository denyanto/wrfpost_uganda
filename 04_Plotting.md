# Simple plotting example
1. Visualising the Sea level Pressure
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
2. Visualising the Temperature 2 m
   ![plot1](https://github.com/user-attachments/assets/8d563c2c-20f5-49f6-b10a-c41badeec1fe)

   Create a file plot2.py with this example:
   ```console
   import netCDF4
   import wrf
   import matplotlib.pyplot as plt
   from matplotlib.cm import get_cmap
   import cartopy.crs as ccrs
   import numpy as np
   
   # Load the NetCDF file using netCDF4
   ncfile = netCDF4.Dataset('wrfoutput/wrfout_d01_2020-01-01_00:00:00')
   
   # Extract a variable (e.g., temperature 2 m, 't2m')
   t2 = wrf.getvar(ncfile, 'T2', timeidx=0)
   
   # Get lat/lon coordinates and map projection
   lats, lons = wrf.latlon_coords(t2)
   cart_proj = wrf.get_cartopy(t2)
   
   # Create the plot
   fig = plt.figure(figsize=(10, 8))
   ax = plt.axes(projection=cart_proj)
   
   # Plot contours
   plt.contourf(wrf.to_np(lons), wrf.to_np(lats), wrf.to_np(t2), 10, 
                   transform=ccrs.PlateCarree(), cmap=get_cmap("Reds"))
   
   # Add a color bar
   cbar = plt.colorbar(ax=ax, shrink=.62)
   cbar.set_label(t2.units)
   
   # Add coastlines, gridlines, and title
   ax.coastlines()
   gl=ax.gridlines(crs=ccrs.PlateCarree(),draw_labels=True,color='black',alpha=0.5,linestyle='--')
   plt.title(t2.description+'\n'+str(t2.Time.values))
  
   # Save the plot
   plt.savefig('plot2.png')
  
   # Close the file
   ncfile.close()
   ```
3. Visualising the WRF Domain
   ![plot1](https://github.com/user-attachments/assets/2347f977-3beb-48e5-970e-57584450194d)

   Create a file plot3.py with this example:
   ```console
   import netCDF4
   import wrf
   import matplotlib.pyplot as plt
   from matplotlib.cm import get_cmap
   import matplotlib as mpl
   import cartopy.crs as ccrs
   import numpy as np
   
   # Load the NetCDF file using netCDF4
   
   def get_plot_element(infile):
       rootgroup = netCDF4.Dataset(infile, 'r')
       p = wrf.getvar(rootgroup, 'RAINNC')
       #lats, lons = wrf.latlon_coords(p)
       cart_proj = wrf.get_cartopy(p)
       xlim = wrf.cartopy_xlim(p)
       ylim = wrf.cartopy_ylim(p)
       rootgroup.close()
       return cart_proj, xlim, ylim
    
   infile_d01 = 'wrfoutput/wrfout_d01_2020-01-01_00:00:00'
   cart_proj, xlim_d01, ylim_d01 = get_plot_element(infile_d01)
    
   infile_d02 = 'wrfoutput/wrfout_d02_2020-01-01_00:00:00'
   _, xlim_d02, ylim_d02 = get_plot_element(infile_d02)
    
   infile_d03 = 'wrfoutput/wrfout_d03_2020-01-01_00:00:00'
   _, xlim_d03, ylim_d03 = get_plot_element(infile_d03)
   
   # Create the plot
   fig = plt.figure(figsize=(10, 8))
   ax = plt.axes(projection=cart_proj)
   
   ax.coastlines('10m', linewidth=0.8)
    
   # d01
   ax.set_xlim([xlim_d01[0]-(xlim_d01[1]-xlim_d01[0])/15, xlim_d01[1]+(xlim_d01[1]-xlim_d01[0])/15])
   ax.set_ylim([ylim_d01[0]-(ylim_d01[1]-ylim_d01[0])/15, ylim_d01[1]+(ylim_d01[1]-ylim_d01[0])/15])
    
   # d01 box
   ax.add_patch(mpl.patches.Rectangle((xlim_d01[0], ylim_d01[0]), xlim_d01[1]-xlim_d01[0], ylim_d01[1]-ylim_d01[0],
                fill=None, lw=3, edgecolor='blue', zorder=10))
   ax.text(xlim_d01[0]+(xlim_d01[1]-xlim_d01[0])*0.05, ylim_d01[0]+(ylim_d01[1]-ylim_d01[0])*0.9, 'D01',
           size=15, color='blue', zorder=10)
    
   # d02 box
   ax.add_patch(mpl.patches.Rectangle((xlim_d02[0], ylim_d02[0]), xlim_d02[1]-xlim_d02[0], ylim_d02[1]-ylim_d02[0],
                fill=None, lw=3, edgecolor='black', zorder=10))
   ax.text(xlim_d02[0]+(xlim_d02[1]-xlim_d02[0])*0.05, ylim_d02[0]+(ylim_d02[1]-ylim_d02[0])*1.1, 'D02',
           size=15, color='black', zorder=10)
    
   # d03 box
   ax.add_patch(mpl.patches.Rectangle((xlim_d03[0], ylim_d03[0]), xlim_d03[1]-xlim_d03[0], ylim_d03[1]-ylim_d03[0],
                fill=None, lw=3, edgecolor='red', zorder=10))
   ax.text(xlim_d03[0]+(xlim_d03[1]-xlim_d03[0])*0.1, ylim_d03[0]+(ylim_d03[1]-ylim_d03[0])*0.1, 'D03',
           size=15, color='red', zorder=10)
   gl = ax.gridlines(crs=ccrs.PlateCarree(), draw_labels=True,
                        linewidth=1, color='gray', alpha=0.5, linestyle='--') 
   ax.set_title('WRF nested domain setup', size=20)
  
   # Save the plot
   plt.savefig('plot3.png')
   ```
4. Visualising the Wind Speed
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
5. Another Visualising the Wind
   ![plot1](https://github.com/user-attachments/assets/db8c88ec-5cc7-4435-84d1-5ccab4a7ece2)

   Create a file plot6.py with this example:
   ```console
   import xarray as xr
   import matplotlib.pyplot as plt
   import matplotlib.cm as cm
   from matplotlib.lines import Line2D
   import numpy as np
   import cartopy.crs as ccrs
   import cartopy.feature as cfeature
   import pandas as pd
   from cartopy.vector_transform import vector_scalar_to_grid
   
   # Load the WRF output file
   ds = xr.open_dataset('wrfoutput/wrfout_d01_2020-01-01_00:00:00')
   
   # Extract the wind speed and direction variables
   u_wind = ds.U10.values
   v_wind = ds.V10.values
   
   # Calculate the wind speed and direction
   wind_speed = (u_wind ** 2 + v_wind ** 2) ** 0.5
   wind_direction = 180 + (180 / np.pi) * np.arctan2(v_wind, u_wind)
   
   # Create a map projection
   proj = ccrs.PlateCarree()
   # Create a figure and axis object
   fig, ax = plt.subplots(figsize=(10, 10), subplot_kw=dict(projection=proj))
   
   # Set the plot extent
   ax.set_extent([ds.XLONG.min(), ds.XLONG.max(), ds.XLAT.min(), ds.XLAT.max()])
   
   # Add map features
   ax.add_feature(cfeature.LAND, facecolor='lightgray')
   ax.add_feature(cfeature.COASTLINE, linewidth=0.5)
   ax.add_feature(cfeature.BORDERS, linewidth=0.5)
   
   # Add the latitude and longitude grid
   gl = ax.gridlines(crs=proj, draw_labels=True,
                     linewidth=1, color='gray', alpha=0.5, linestyle='--')
   gl.top_labels = False
   gl.right_labels = False
   
   # Add the wind vectors
   new_x, new_y, new_u, new_v,c = vector_scalar_to_grid(proj,proj,15,ds.XLONG.values,ds.XLAT.values,u_wind,v_wind, wind_speed)
   Q=ax.quiver(new_x,new_y,new_u,new_v,c, transform=proj, regrid_shape=30, cmap='plasma', pivot='middle') 
   qk = plt.quiverkey(Q, 0.1, 0.2, 10, r'10 m/s', labelpos='E', coordinates='figure')
   # Add a colorbar
   cbar = fig.colorbar(Q, orientation='horizontal', fraction=0.05, pad=0.1)
   cbar.ax.set_xlabel('Wind Speed (m/s)')
   
   # Create a string with the title and date information
   title_str = f'Mean of Wind Speed and Direction'
   
   # Add a title
   plt.title(title_str)
   
   # Save the plot
   plt.savefig('plot6.png')
   
   # Close the file
   ds.close()
   ```
7. 
