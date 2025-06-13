# Adding attribute during plot
1.  X-label and Y-label
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
    plt.xlabel("X-Axis Label")      # adding X-axis label
    plt.ylabel("Y-Axis Label")      # adding Y-axis label
    # Save the plot
    plt.savefig('plot1.png')
    
    # Close the file
    ncfile.close()
    ```
3. 
