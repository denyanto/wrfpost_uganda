# Panel plot
  Panel plots, also known as trellis plots or lattice plots, are a way to display multiple plots in a grid, where each small plot represents a different condition or item, while all plots share the same scales.

Here's how you can create panel plots in Python:
1. Using Matplotlib:
   - The matplotlib.pyplot module provides the ``subplots()`` function to create a figure and a grid of subplots.
   - You can specify the number of rows and columns for the grid.
   - Each subplot can be accessed and modified individually.
   ````console
   import matplotlib.pyplot as plt
   import numpy as np
  
   # Sample data
   x = np.linspace(0, 100, 1000)
   y1 = np.sin(x)
   y2 = np.cos(x)
   y3 = np.tan(x)
  
   # Create a figure and a grid of subplots (2 rows, 2 columns)
   fig, axes = plt.subplots(2, 2, figsize=(8, 6))
  
   # Plot data on each subplot
   axes[0, 0].plot(x, y1)
   axes[0, 0].set_title('Sine Function')
  
   axes[0, 1].plot(x, y2)
   axes[0, 1].set_title('Cosine Function')
  
   axes[1, 0].plot(x, y3)
   axes[1, 0].set_title('Tangent Function')
  
   axes[1, 1].plot(x, y1 + y2)
   axes[1, 1].set_title('Sine + Cosine')
  
   # Adjust spacing between subplots
   plt.tight_layout()
  
   # Display the plot
   plt.show()
   ````
   Example using wrfout data:
   ````console
   from netCDF4 import Dataset
   import matplotlib.pyplot as plt
   import cartopy.crs as ccrs
   import wrf
  
   file_path = "wrfoutput/wrfout_d01_2020-01-01_00:00:00"  # Replace with your file path
   ncfile = Dataset(file_path)
   slp = wrf.getvar(ncfile, 'slp')
   t2m = wrf.getvar(ncfile, 'T2')
   rh = wrf.getvar(ncfile, 'rh2')
   wspd = wrf.getvar(ncfile, 'wspd')
   lats, lons = wrf.latlon_coords(slp)
  
   fig, axes = plt.subplots(2, 2, figsize=(8, 6), subplot_kw={'projection': ccrs.PlateCarree()})
   axes[0, 0].contourf(lons, lats, slp, transform=ccrs.PlateCarree(), cmap='jet')
   axes[0, 0].set_title('Sea Level Pressure (SLP)')
   axes[0, 0].coastlines()
   gl=axes[0, 0].gridlines(crs=ccrs.PlateCarree(),draw_labels=True,color='black',alpha=0.5,linestyle='--')
   gl.top_labels = False
   gl.right_labels = False
  
   axes[0, 1].contourf(lons, lats, t2m, transform=ccrs.PlateCarree(), cmap='jet')
   axes[0, 1].set_title('2 meter2 Temperature')
   axes[0, 1].coastlines()
   gl=axes[0, 1].gridlines(crs=ccrs.PlateCarree(),draw_labels=True,color='black',alpha=0.5,linestyle='--')
   gl.top_labels = False
   gl.right_labels = False
  
   axes[1, 0].contourf(lons, lats, rh, transform=ccrs.PlateCarree(), cmap='jet')
   axes[1, 0].set_title('Relative Humidity')
   axes[1, 0].coastlines()
   gl=axes[1, 0].gridlines(crs=ccrs.PlateCarree(),draw_labels=True,color='black',alpha=0.5,linestyle='--')
   gl.top_labels = False
   gl.right_labels = False
  
   axes[1, 1].contourf(lons, lats, wspd[0,...], transform=ccrs.PlateCarree(), cmap='jet')
   axes[1, 1].set_title('wind Speed')
   axes[1, 1].coastlines()
   gl=axes[1, 1].gridlines(crs=ccrs.PlateCarree(),draw_labels=True,color='black',alpha=0.5,linestyle='--')
   gl.top_labels = False
   gl.right_labels = False
  
   plt.tight_layout()
   plt.show()
   plt.savefig('plot2.png')
   ````

2. Meteogram:
   - A meteogram is a type of weather chart that shows how different weather conditions are expected to change over time at a specific location.
   - Instead of just giving a simple forecast (like “rain tomorrow”), a meteogram displays multiple weather variables together in a timeline, making it easier to see patterns and trends..
   - Why it’s useful:
     + Gives a detailed, hour-by-hour forecast.
     + Helps with planning activities like travel, farming, or outdoor events.
     + Lets you see weather trends, not just single predictions.

   Example Creating a Meteogram:
   ````console
    from netCDF4 import Dataset
    import matplotlib.pyplot as plt
    import cartopy.crs as ccrs
    import numpy as np
    import wrf, glob
    from mpl_toolkits.axes_grid1 import make_axes_locatable

    # Read data
    file_list = sorted(glob.glob("wrfoutput/wrfout_d01_*"))  # Replace with your file path
    ncfile = [Dataset(f) for f in file_list]
    times = wrf.getvar(ncfile, "times", timeidx=wrf.ALL_TIMES)
    t = wrf.getvar(ncfile, 'tc', timeidx=wrf.ALL_TIMES)
    u = wrf.getvar(ncfile, "ua", timeidx=wrf.ALL_TIMES)
    v = wrf.getvar(ncfile, "va", timeidx=wrf.ALL_TIMES)
    rh = wrf.getvar(ncfile, 'rh', timeidx=wrf.ALL_TIMES)
    p = wrf.getvar(ncfile, 'pressure', timeidx=wrf.ALL_TIMES)
    u10 = wrf.getvar(ncfile, "U10", timeidx=wrf.ALL_TIMES)
    v10 = wrf.getvar(ncfile, "V10", timeidx=wrf.ALL_TIMES)
    wspd = np.sqrt(u10**2 + v10**2)
    slp = wrf.getvar(ncfile, 'slp', timeidx=wrf.ALL_TIMES)
    t2 = wrf.getvar(ncfile, "T2", timeidx=wrf.ALL_TIMES)-273.16
    td2 = wrf.getvar(ncfile, "td2", timeidx=wrf.ALL_TIMES)
    rh2 = wrf.getvar(ncfile, 'rh2', timeidx=wrf.ALL_TIMES)
    cf = wrf.getvar(ncfile, 'cloudfrac', timeidx=wrf.ALL_TIMES)
    rain = wrf.getvar(ncfile, 'RAINC', timeidx=wrf.ALL_TIMES)+wrf.getvar(ncfile, 'RAINNC', timeidx=wrf.ALL_TIMES)
    lats, lons = wrf.latlon_coords(slp)
    levels = np.arange(1000, 99, -50)

    # Filling the certain location longitude and latititude
    point = [106.99, -7.41]
   
    # Finding the point location
    iy = np.nanargmin(np.abs(lats[:,0] - point[1]))
    ix = np.nanargmin(np.abs(lons[0,:] - point[0]))
   
    # Calculating the interpolation vertical levels
    rh_interp = [];t_interp = [];u_interp = [];v_interp = []
    for lev in levels:
        rh_lev = wrf.interplevel(rh, p, lev)
        t_lev = wrf.interplevel(t, p, lev)
        u_lev = wrf.interplevel(u, p, lev)
        v_lev = wrf.interplevel(v, p, lev)
        rh_interp.append(rh_lev)
        t_interp.append(t_lev)
        u_interp.append(u_lev)
        v_interp.append(v_lev)
    rh_interp = np.array(rh_interp)
    t_interp = np.array(t_interp)
    u_interp = np.array(u_interp)
    v_interp = np.array(v_interp)

    # Getting the point data
    irh=rh_interp[:,:,iy,ix]
    it=t_interp[:,:,iy,ix]
    iu=u_interp[:,:,iy,ix]
    iv=v_interp[:,:,iy,ix]
    iu10=u10[:,iy,ix]
    iv10=v10[:,iy,ix]
    iws=wspd[:,iy,ix]
    islp=slp[:,iy,ix]
    it2=t2[:,iy,ix]
    itd2=td2[:,iy,ix]
    irh2=rh2[:,iy,ix]
    icf=cf[:,:,iy,ix]*100
    irain=np.array(rain[:,iy,ix])
    irain[1:]=irain[1:]-irain[:-1]
   
    # Plotting a Meteogram
    fig, axes = plt.subplots(7, 1, figsize=(8,10),gridspec_kw={'height_ratios': [6, 1, 1, 1, 1, 1, 1]}, layout='constrained')
    cs=axes[0].contourf(times,levels,irh,cmap='Greens')
    cx=axes[0].contour(times,levels,it)
    axes[0].clabel(cx, inline=True, fontsize=10)
    axes[0].barbs(times,levels, iu, iv, length=4)
    plt.colorbar(cs,ax=axes[0])
    axes[0].invert_yaxis() 
    axes[0].set_title('Meteogram')
    axes[0].set_ylabel('RH(shaded,%) Temp(line, \u00B0C) Wind(barb, m/s)\nmilibars')
    axes[0].set_xticklabels([]) 
    axes[1].plot(times,iws,'o-y')
    axes[1].barbs(times,iws.mean(),iu10,iv10)
    axes[1].set_ylabel('10m Wind\nSpeed & Barb\n(m/s)')
    axes[1].grid(True)
    axes[1].minorticks_on()
    axes[1].set_xticklabels([]) 
    axes[2].plot(times,islp,'o-b')
    axes[2].set_ylabel('MSLP\n(mb)')
    axes[2].grid(True)
    axes[2].minorticks_on()
    axes[2].invert_yaxis() 
    axes[2].set_xticklabels([]) 
    axes[3].plot(times,it2,'-r')
    axes[3].plot(times,itd2,'--r')
    axes[3].set_ylabel('2m Temp\n2m DewPoint\n(\u00B0C)')
    axes[3].grid(True)
    axes[3].minorticks_on()
    axes[3].set_xticklabels([]) 
    axes[4].plot(times,irh2,'o-g')
    axes[4].set_ylabel('2m RH\n(%)')
    axes[4].grid(True)
    axes[4].minorticks_on()
    axes[4].set_xticklabels([]) 
    x = np.arange(len(times))
    colors = ['#add8e6', '#1e90ff', '#00008b'] 
    axes[5].bar(x-0.2,icf[0,:],0.15,label='low',color=colors[0])
    axes[5].bar(x,icf[1,:],0.15,label='middle',color=colors[1])
    axes[5].bar(x+0.2,icf[2,:],0.15,label='high',color=colors[2])
    axes[5].set_ylim(0, 120)
    axes[5].set_ylabel('Cloud Cover\n(%)')
    axes[5].grid(True)
    axes[5].minorticks_on()
    axes[5].set_xticklabels([]) 
    axes[5].legend(loc=0, ncol=3, markerscale=0.5)
    axes[6].bar(times,irain,0.01,color='lightgreen')
    axes[6].set_ylabel('Rain\n(mm)')
    axes[6].grid(True)
    axes[6].minorticks_on()
    axes[6].set_ylim(0, round(irain.max()))
    wkt=[str(wrf.to_np(i))[0:-16] for i in times]
    axes[6].set_xticklabels(wkt, rotation=60)
    # Saving the figure
    plt.savefig('plot22.png')
   ````
