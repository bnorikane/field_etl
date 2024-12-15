# BCDP Field Map Data

Create all the data files required for a map of BCDP Field Team Areas and Boulder County Election Districts

- This notebook reads input files from various soures
- Transforms the input data to the columns and formats required for a Leaflet or Google Map
- Saves files in file formats ready to be used directly in an online map
  - GeoJSON output files for Leaflet
  - KML files for Google Maps
- Started: August 10, 2022
- Updated: December 15, 2024

## Minimum Viable Product

- Government Districts data files for Boulder County
  - Boulder County
  - Boulder County Precincts
  - Congressional District
  - Colorado State Senate Districts
  - Colorado State House Districts
- BCDP Information files
  - Field Team Areas
- Mapping software support
  - Google Maps KML files
  - Leaflet.js GeoJSON files

## Potential Enhancements

- Field Team Coverage and Vacancies
  - Orphan Areas
  - Orphan Precincts
  - Single Precinct Organizer Vacancy
  - BCDP Field Team Member Data
    - AC Names
    - PCP Names
  - BCDP Caucus SuperSite Boundaries (list of precincts)

## Input Files

- Boulder County boundary
- CD Boundary
- SD Boundaries
- HD Boundaries
- Boulder County Precincts
- BCDP Field Team Areas (list of precincts in each Area)

## Output Files

- Leaflet.js GeoJSON files
  - CD boundary
  - SD boundaries
  - HD Boundaries
  - Boulder County precinct boundaries
  - BCDP Field Team Areas
    - list of precincts
    - boundaries?

## ISSUES

### Issue1: get errors in ETL of Redistricting Commission shape files

- STATUS: fixed
- FIX: upgraded to GeoPandas 0.11.1 and numpy 1.22.3
- ISSUE:
  - RC shape files read into GeoPandas without issue
  - RC shape files create valid GeoDataFrames that can plot, etc...
  - When these GDF's are written to geojson files, they cannot be read in correctly
- Workaround: Convert Redistricting shape files to KML or geojson externally

### Issue2: geojson files saved in wrong CRS

- STATUS: Fixed

  - SOLUTION: convert all geometry to EPSG:4326
  - After reading shape file:
    - check crs
    - convert gpd crs to Leaflet compatible 4326
      - gpd.to_crs(4326)
      - ESPG:4326 (WGS 84 long/lat)

- Description:

  - Redistricting shape files are stored with CRS = EPSG:2232
  - coordinates not long/lat
  - coordinates are northing/easting

  - SD geojson files use urn:ogc:def:crs:EPSG::2232, not compatible with Leaflet display
  - need to convert these files
  - Leaflet's default projection is EPSG:3857, also known as "Google Mercator" or "Web Mercator" and sometimes designated with the number "900913". This projection is what most slippy tile based maps use, including the common tile sets from Google, Bing, OpenStreetMap, and others. You can easily use this projection in QGIS by selecting "Google Mercator EPSG:900913". Leaflet has some basic support for displaying maps in other projections. Most folks who do that seem to use the addon Proj4Leaflet to perform the projection.

- Diagnosis:
  <Derived Projected CRS: EPSG:2232>
  Name: NAD83 / Colorado Central (ftUS)
