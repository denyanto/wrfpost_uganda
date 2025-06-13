# Adding attribute during plot
1.  Labels and Titles:
    - Use ``` xlabel() ``` to label the x-axis, ``` ylabel() ``` for the y-axis, and ``` title() ``` to set the plot's title.
    - You can customize the font properties of these labels and titles using the ``` fontdict ``` parameter.
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
    font1 = {'family':'serif','color':'blue','size':20}
    font2 = {'family':'serif','color':'darkred','size':15}
    plt.title('Sea Level Pressure (SLP)', fontdict = font1)   # adding a title
    plt.xlabel("X-Axis Label", fontdict = font2)              # adding X-axis label
    plt.ylabel("Y-Axis Label", fontdict = font2)              # adding Y-axis label
    # Save the plot
    plt.savefig('plot1.png')
    
    # Close the file
    ncfile.close()
    ```
2.  Figure Size: Use ``` figure(figsize=(width, height)) ``` to control the size of the plot.
3.  Show Plot: Use ``` plt.show() ``` to display the plot in an interactive window.
