# climateExtract

R functions to extract climate data from local NETCDF file that you can download from the
ECAD at http://www.ecad.eu/download/ensembles/download.php#datafiles

If you did not already download the data, you can download the data directly with the function extract_nc_value().

# Installation
You will need to install the package "devtools" [install.packages("devtools")] and then use the function install_github()

library(devtools)

install_github("RetoSchmucki/Rpackages/climateExtract")

# Example

1. Get your climate data from the official website http://www.ecad.eu/download/ensembles/download.php#datafiles and
decompress the data to extract the ".nc" file. If you want the function to download the data, set the parameter local_file=FALSE and provide the detail about the data you need.

Note: Read carefully the description of the data to select the right grid size and variable (eg. 0.25 deg. regular grid, "TG" average temperature). If you don't need the complete time serie, 1950-2014, smaller chuncks are also available http://www.ecad.eu/download/ensembles/downloadchunks.php

2. Use the function extract_nc_value() to extract the climate values for a specific time period. This function will open an interactive window to select the .nc file from which you want to extract the data from. Specify the firs and the last years of the time period you are interested. 

climate_data <- extract_nc_value(2010,2014)
OR
climate_data <- extract_nc_value(2010,2014,local_file=FALSE,clim_variable=mean temp,grid_size=0.25)

3. Compute summary value with the function temporal_mean(). This function allow you to compute the annual mean, the monthly mean or a roalling average (moving window) average over the time period extracted in the previous step.

annual_mean <- temporal_mean(climate_data,"annual")

4. Extract time serie for specific points with the function point_grid_extract(). This function extract the value corresponding to your points from either the original climate time serie or the averaged one. You need to provide the geographic coordinates of your points in degree decimal format, using the epsg projection 4326 - wgs 84.

point_coord <- data.frame(site_id=c("site1","site2","site3","site4","site5"), longitude=c(28.620000,6.401499,4.359062,-3.579906,-2.590392), latitude=c(61.29000,52.73953,52.06530,50.43031,52.02951)) 
               
point.ann_mean <- point_grid_extract(annual_mean,point_coord)

# Note

This bundle of functions is a work in progress that might be usefull for some specific task, but comes with
no garantee of fitting all your needs. Suggestions and contributions for improvement are welcome.









