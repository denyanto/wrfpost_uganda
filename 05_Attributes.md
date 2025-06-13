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
2.  Line Properties:
    - When using ```plot()```, you can specify line properties such as color, linewidth, [linestyle](https://matplotlib.org/stable/gallery/lines_bars_and_markers/linestyles.html), and [marker](https://matplotlib.org/1.4.1/api/markers_api.html) style. 
    - These can be set directly as keyword arguments in the ```plot()``` function.
    - You can also use ```setp()``` to modify multiple properties of a line or multiple lines at once, such as customizing labels including the tick labels.
    In the example below, ``ax.get_xticklabels()`` grabs the tick labels from the x axis, and then the rotation argument specifies an angle of rotation (e.g. 45), so that the tick labels along the x axis are rotated 45 degrees.
    ```console
    import matplotlib.pyplot as plt
    
    x = [1, 2, 3]
    y = [40, 50, 60]
    
    plt.plot(x, y, color='red', linewidth=2, linestyle='--', marker='o', label='Line 1')
    plt.plot(x, [60, 70, 80], color='blue', linewidth=1, linestyle='-', marker='x', label='Line 2')
    plt.setp(ax.get_xticklabels(), rotation = 45)
    plt.legend()
    plt.show()
    ```
3.  Legends:
    - Use ```legend()``` to add a legend to the plot, which helps in distinguishing between different lines or data sets.
    - You can specify labels for each line in the ```plot()``` function and call ```legend()``` to display them.
4.  Markers:
    - Markers can be added to the plot to highlight specific data points.
    - You can use different shapes and styles for markers by specifying the marker parameter in the ```plot()``` function.
5.  Axes Customization:
    - You can set limits, ticks, and labels for both axes.
    - Use ``ax.set()`` to customize the axes.
    ````console
    fig, ax = plt.subplots()
    ax.plot(x, y)
    ax.set(title = "Customized Plot", xlabel = "X-axis", ylabel = "Y-axis")
    plt.show()
    ````
6.  Figure Size: Use ``` figure(figsize=(width, height)) ``` to control the size of the plot.
7.  Show Plot: Use ``` plt.show() ``` to display the plot in an interactive window.
