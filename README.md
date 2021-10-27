# City_of_Pittsburgh
There is no real immediate goal/purpose for this map other than to display Pittsburgh's urban landscape.
Neighborhood boundaries and Parks are fairly common layers in map-making, but I specifically chose bridges since Pittsburgh has more bridges than any other city in the world (surprisingly, even more than Venice, Italy). 

# Metadata
## Data Sources
Three datasets were used to cover the city limits: 
+ Neighborhoods: (Neighborhoods_-shp.zip)
+ Bridges: (Pittsburgh_Bridges_-shp.zip)
+ Parks: (Parks.zip)

**All files were downloaded as SHP** 

**Website:** https://data.wprdc.org/dataset

# Setup

## GDAL/OGR
*if GDAL/OGR is **NOT** installed*
+ run command *gdalinfo --version*
+ if cmd prompt says it can't be found, install it
+ it is recommended to check your PC's environmental variables and that GDAL's files are pathed (e.g.):
    + C:\Program Files\GDAL
    + C:\Program Files\GDAL\projlib

**if installed as stand-alone program, be sure to have GDAL and its project library pathed**

# Data Wrangling
Data management process:
+ transform file CRS using OGR
+ converting SHP to geojson

## Checking SHP info using OGR
Before getting too far ahead, take a glance at the information for each SHP using OGR (e.g.):
+ navigate your dir into the unzipped data folder with the desired SHP and accessory files
    + this directory change is important, otherwise OGR won't have access to the SHP 
+ run the command *ogrinfo -so Parks.shp Parks*
+ take note of the CRS - *if not in WGS84, it will need converted to it to allow stable web-mapping*
* the above applies to accessing info from all SHP (except changing command where appropriate)

## Projection Transformation using OGR
If a SHP is not in the WGS84 datum, we can tranform it using OGR (e.g.):
+ run the command *ogr2ogr NEW.shp -t_srs "EPSG:4326" OLD.shp*
+ run the command *ogrinfo -so NEW.shp FOLDER* to verify the transformation worked

## Converting SHP to GeoJSON using OGR
OGR is also useful in converting file types:
+ run the command *ogr2ogr -f "GeoJSON" ../NEW.json NEW_CRS.shp
+ note that ogr will output the new geojson one directory up (data directory)

## Extracting Features using OGR
To shrink our new geojson files AND reduce upload times on the server, OGR can create a new geojson and filter for any selected attributes (e.g.):
+ ogr2ogr -f "GeoJSON" -select hood pgh-areas.json  pgh-areas.json
+ ogr2ogr -f "GeoJSON" -select hood pgh-areasN.json pgh-areas.json
+ ogr2ogr -f "GeoJSON" -select origpkname pgh-parksN.json pgh-parks.json

# Map-making
Be sure to include the following installs within your HTML:
+ Leaflet & Leaflet CSS **OR** D3
+ JQUERY

**Happy Mapping**