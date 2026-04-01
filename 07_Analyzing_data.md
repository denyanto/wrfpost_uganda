# Analyzing data assimilation (DA) increments, diagnostics, and evaluating forecast impact
1. Introduction

   Weather Research and Forecasting Data Assimilation (WRF-DA) outputs are typically stored in NetCDF format, containing 3D or 4D model variables. Post-processing is critical for:
     - Analyzing DA increments: Understanding how observations impact the model state.
     - Computing diagnostics: Such as innovation (observation minus background), analysis increments, RMSE, and spread.
     - Evaluating forecast impact: Checking if DA improved forecast skill.

     Python, with libraries like netCDF4, numpy, xarray, matplotlib, and cartopy, is perfect for this workflow.
2. Required Python Libraries
   ````console
   import numpy as np
   import netCDF4 as nc
   import xarray as xr
   import matplotlib.pyplot as plt
   import cartopy.crs as ccrs
   ````
3. Reading WRF-DA Output

   WRF-DA outputs include:
   - wrfinput_d01 / wrfda_output (analysis files)
   - Observation files
   Example using xarray:
   ````console
   # Open WRF-DA analysis output
   da_file = 'wrfda_output.nc'
   ds = xr.open_dataset(da_file)
   
   # List variables
   print(ds.variables.keys())
   
   # Extract 3D variable, e.g., temperature at model levels
   temp = ds['T']  # K
   ````
4. Computing Data Assimilation Diagnostics

   4.1 Bias

   Bias is the difference between observations and forecast:

   bias = 𝑦obs − 𝑦mod
   ````console
   # Suppose obs and forecast arrays are available
   ds1 = xr.open_dataset('ERA5_data.nc')
   ds2 = xr.open_dataset('wrfout.nc')
   obs = ds1['T2'].values  # observations
   bkg = ds2['T2'].values  # forecast
   
   bias = obs - bkg
   
   # Example: plot histogram of innovation
   plt.hist(bias.flatten(), bins=50, color='skyblue', edgecolor='black')
   plt.xlabel('Bias (K)')
   plt.ylabel('Frequency')
   plt.title('Bias Histogram')
   plt.show()
   ````
   If you have difference resolution such as:
   
   1 ERA5 data (coarse resolution, e.g., ~0.25°).
   2 WRF output file defining the target 9 km grid.
   You need regridding first before implement above script.
   ````console
   import numpy as np
   import xarray as xr
   from scipy.interpolate import griddata
   
   # -------------------------------
   # 1. Load ERA5 data
   # -------------------------------
   era_file = 'era5_hourly.nc'  # Replace with your ERA5 NetCDF
   era_ds = xr.open_dataset(era_file)
   
   # Example variables
   vars_to_regrid = ['t2m']  # 2m temperature
   
   # ERA5 lat/lon
   era_lat = era_ds['latitude'].values
   era_lon = era_ds['longitude'].values
   
   # Flatten ERA5 grid for interpolation
   ERA_lon_flat, ERA_lat_flat = np.meshgrid(era_lon, era_lat)
   points = np.column_stack((ERA_lon_flat.ravel(), ERA_lat_flat.ravel()))
   
   # -------------------------------
   # 2. Load WRF output grid
   # -------------------------------
   wrf_file = 'wrfout_d01_2024-04-01_00:00:00'
   wrf_ds = xr.open_dataset(wrf_file)
   
   # WRF grid (curvilinear)
   lat_wrf = wrf_ds['XLAT'][0,:,:].values
   lon_wrf = wrf_ds['XLONG'][0,:,:].values
   
   target_points = np.column_stack((lon_wrf.ravel(), lat_wrf.ravel()))
   
   ny, nx = lat_wrf.shape
   nt = era_ds.dims['time']
   
   # -------------------------------
   # 3. Interpolate function
   # -------------------------------
   def regrid_var(var_array):
       """Regrid a single variable from ERA5 to WRF grid for all time steps."""
       regridded = []
       for t in range(var_array.shape[0]):
           data_flat = var_array[t,:,:].ravel()
           interp_values = griddata(points, data_flat, target_points, method='linear')
           regridded.append(interp_values.reshape(ny,nx))
       return np.array(regridded)
   
   # -------------------------------
   # 4. Perform regridding for the variable
   # -------------------------------
   regridded_vars = {}
   for var_name in vars_to_regrid:
       print(f"Regridding {var_name}...")
       var_data = era_ds[var_name].values
       regridded_vars[var_name] = regrid_var(var_data)
   
   # -------------------------------
   # 5. Save to NetCDF
   # -------------------------------
   out_ds = xr.Dataset(
       {var: (('time','y','x'), regridded_vars[var]) for var in vars_to_regrid},
       coords={
           'time': era_ds['time'].values,
           'y': np.arange(ny),
           'x': np.arange(nx),
           'lat': (('y','x'), lat_wrf),
           'lon': (('y','x'), lon_wrf)
       }
   )
   
   out_file = 'era5_regridded_to_wrf.nc'
   out_ds.to_netcdf(out_file)
   print(f"Regridding complete! Saved to {out_file}")
   ````
   
   4.2 Analysis Increment

   Analysis increment is the difference between analysis and background:

   increment = 𝑥𝑎 − 𝑥𝑏

   ````console
   anl = ds['ANL_TEMP'].values  # analysis
   increment = anl - bkg
   
   # Plot spatial map at first vertical level
   plt.figure(figsize=(8,6))
   plt.contourf(increment[0,:,:], cmap='RdBu_r')
   plt.colorbar(label='Analysis Increment (K)')
   plt.title('Temperature Analysis Increment at Level 1')
   plt.show()
   ````

   4.3 Root Mean Square Error (RMSE)

   RMSE is widely used to quantify forecast improvement:

   RMSE = $$\sqrt{ 1/N \sum_{k=1}^N \left(xa​ − yobs​\right)^2}$$

   ````console
   rmse = np.sqrt(np.mean((anl - obs)**2))
   print(f'RMSE of Analysis: {rmse:.2f} K')
   ````
   You can also compute time series RMSE to evaluate forecast impact:
   ````console
   # Suppose we have multiple-time steps
   rmse_ts = [np.sqrt(np.mean((anl[t,:,:] - obs[t,:,:])**2)) for t in range(anl.shape[0])]
   
   plt.plot(rmse_ts, marker='o')
   plt.xlabel('Time step')
   plt.ylabel('RMSE (K)')
   plt.title('RMSE Evolution Over Time')
   plt.grid(True)
   plt.show()
   ````
      
6. Evaluating Forecast Impact

   1 Compute bias before and after assimilation.
   2 Compare RMSE between background and analysis:

   ````console
   rmse_bkg = np.sqrt(np.mean((bkg - obs)**2))
   rmse_anl = np.sqrt(np.mean((anl - obs)**2))
   
   print(f'Background RMSE: {rmse_bkg:.2f} K')
   print(f'Analysis RMSE: {rmse_anl:.2f} K')
   improvement = rmse_bkg - rmse_anl
   print(f'RMSE Reduction after DA: {improvement:.2f} K')
   ````
   3 Map spatial impact:
   ````console
   impact = np.abs(bkg - obs) - np.abs(anl - obs)  # positive = improvement

   plt.figure(figsize=(8,6))
   plt.contourf(impact[0,:,:], cmap='RdBu_r')
   plt.colorbar(label='RMSE Improvement (K)')
   plt.title('Spatial Forecast Improvement After DA')
   plt.show()
   ````
