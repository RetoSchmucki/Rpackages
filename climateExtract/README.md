# climateExtract

R functions to extract climate data from local NETCDF file that you can download from the
ECAD at [http://www.ecad.eu/download/ensembles/download.php#datafiles](http://www.ecad.eu/download/ensembles/download.php#datafiles)

If you did not already download the data, you can download the data directly with the function extract_nc_value().

#### Installation
You will need the package `devtools` and then use the function install_github()
```
install.packages("devtools")
library(devtools)
install_github("RetoSchmucki/Rpackages/climateExtract")
```

This package depends on the `ncdf4` package. For Linux or MacOS users, the `ncdf4` can be installed directly from CRAN.

**Windows** users, you should refer to the instructions available at http://cirrus.ucsd.edu/~pierce/ncdf/ and install the `ncdf4` package manually from the appropriate `.zip` file.

**Before extracting any data, please read carefully the description of the datasets and the different grid size available (eg. 0.25 deg. regular grid, "TG" average temperature).** 
**Note** that shorter time-series are also available [http://www.ecad.eu/download/ensembles/downloadchunks.php](http://www.ecad.eu/download/ensembles/downloadchunks.php)


#### Example

You can get your climate data from the web repository http://www.ecad.eu/download/ensembles/download.php#datafiles and decompress the data to extract the `.nc` file.

Or you can use the function `extract_nc_value()` to download the data directly by setting the parameter local_file to FALSE and adding the details of the data you want to be extracted.




**1.** To extract climate values for a specific time period, use the function `extract_nc_value()`. By default this function will open an interactive window asking you to select a local `.nc` file from which you want the data to extract from, in this case you just have to specify the firs and the last years of the time period you are interested. 
```
climate_data <- extract_nc_value(2010,2014)
```
**2.** If you don't have a local .nc file, you can ask the function to download the desired data directly from the web repository.
```
climate_data <- extract_nc_value(2010,2014,local_file=FALSE,clim_variable='precipitation',grid_size=0.25)
```
*where clim_variable set to:*
* "mean temp" extract the daily mean temperature
* "mim temp" extract the daily minimum temperature
* "max temp extract the daily maximum temperature
* "precipitation" extract the daily precipitation

*where grid_size set to:*
* 0.25 extract a grid with a 0.25-degree resolution
* 0.50 extract a grid with a 0.50-degree resolution

**3.** To compute summary value of the daily values, use the function `temporal_mean()` for temperature or `temporal_sum()` for precipitation . This function computes the mean for a specified time period, monthly or annual or for specified window computing a rolling average over a specific number of days. **NOTE** This function use the data extracted with the function `extract_nc_value`.

```
annual_mean <- temporal_mean(climate_data,"annual")
monthly_sum <- temporal_sum(climate_data,"monthly")
```
**4.** To extract the weather data for a set of specific locations (points), use the function `point_grid_extract()`. With this function, you can extracts either the original or the summary values corresponding to the points, depending on the data object provided in the first argument. The second argument is a `data.frame` with the coordinates of the points in a degree decimal format and using the epsg projection 4326 - **wgs 84** 

```
point_coord <- data.frame(site_id=c("site1","site2","site3","site4","site5"), longitude=c(28.620000,6.401499,4.359062,-3.579906,-2.590392), latitude=c(61.29000,52.73953,52.06530,50.43031,52.02951)) 
>
point.ann_mean <- point_grid_extract(annual_mean,point_coord)
point.month_sum <- point_grid_extract(monthly_sum,point_coord)
```

*This is a work in progress that is good for some tasks, but this comes with no guarantee. Suggestions and contributions for improvement are welcome.*
