# NASA Pale Blue Dot Data Visualization Challenge
This is a repository for our team's submission files, source data, and Python Notebooks for the NASA Pale Blue Dot Data Visualization Challenge hosted on DrivenData: https://www.drivendata.org/competitions/256/pale-blue-dot/. 

## Background
The aim of the challenge was to develop a compelling data visualization using Earth observation data that advances one of three Sustainable Development Goals (SDGs): Zero Hunger, Clean Water and Sanitation, or Climate Action.  We chose to focus on wildfires, a threat that is both dramatically striking and relatively suited for Earth-observation data analysis, and their presence in the eastern half of our continent. Our visualization involves not only the spread of the fires themselves, but also tracks the corresponding effects on air quality that can impact populations far removed from the fire itself. Our aim was to raise awareness of under-studied Eastern US wildfires and provide a tool to track this trend, we demonstrate the growing dual risk of catastrophic localized fire damage and persistent broad-reaching air quality detriment.

## Data Sources
Our data visulization leveraged three primary data sources:
1. [Environmental Protection Agency (EPA) Air Quality System Data](https://www.epa.gov/aqs): The Air Quality System (AQS) contains ambient air pollution data collected by EPA, state, local, and tribal air pollution control agencies from over thousands of monitors.
2. [Moderate Resolution Imaging Spectroradiometer (MODIS) Active Fire Data Product](https://firms.modaps.eosdis.nasa.gov/active_fire/): The MODIS active fire product detects fires in 1-km pixels that are burning at the time of overpass under relatively cloud-free conditions using a contextual algorithm.
3. [MODIS Burned Area Data Prodcut](https://lpdaac.usgs.gov/products/mcd64a1v061/): The MODIS Burned Area data product is a monthly, global gridded 500 meter (m) product containing per-pixel burned-area and quality information.

We accessed EPA AQS data via the the repository which contains [pre-generated data files](https://aqs.epa.gov/aqsweb/airdata/download_files.html), and the CSV files are stored within this github repository [here](https://github.com/bwalzer4/NASA-Pale-Blue-Dot/tree/main/EPA%20Air%20Quality/AQI%20Data). The EPA AQS data ranges from 2000 - 2023 for every Core-Based Statistical Area (CBSA) in the United States.

The MODIS Active Fire data product was accessed directly from the Fire Information for Resource Management System (FIRMS) website, which is linked above. The data covers 2000-2023 and covers the entire United States and Canada.

The MODIS Burned Area data product was downloaded as Hierarchical Data Format (HDF) files from the University of Maryland sftp server. The files are too large to be hosted on a public Github repository, however details on accessing the Burned Area data prpduct can be found in Section 4 of the [Burned Area Product User’s Guide](https://modis-fire.umd.edu/files/MODIS_C61_BA_User_Guide_1.1.pdf). 

## Data Preprocessing
Our air quality data processing involved gathering, cleaning, and combining several datasets into one. The first piece of the puzzle consisted of 23 separate Comma-Separated Variable (CSV) files of daily air quality indices from the EPA’s AQS repository. We processed the data using the pandas package in Python, looping through each of the files to create one consolidated data frame with a narrowed focus on particulate matter of 2.5 micrometers or less (PM2.5) and carbon monoxide (CO). The second portion of the processing  of the puzzle involved reading the [dataset of Core Based Statistical Areas (CBSAs)](https://github.com/bwalzer4/NASA-Pale-Blue-Dot/blob/main/EPA%20Air%20Quality/2023_Gaz_cbsa_national.txt) and associated global coordinates from the United States Census Bureau into a data frame. We used pandas to standardize the CBSA naming convention of both datasets and merge the two data frames into one. Though there were some missing coordinates and some wayward CBSA names, we were able to troubleshoot all of the problems until we ended up with a complete, concise, AQI by global coordinates database. The Python Notebook can be accessed [here](https://github.com/bwalzer4/NASA-Pale-Blue-Dot/blob/main/EPA%20Air%20Quality/Air%20Quality%20Data%20Analysis%20Final.ipynb(https://github.com/bwalzer4/NASA-Pale-Blue-Dot/blob/main/MODIS/burned%20area%20notebook%20v2.ipynb).

The MODIS Active Fire data did not require significant data processing as the format consisted of a CSV single file - the only procssing was to convert the file from CSV format to Parquet using the Pandas Python package, which signifcantly reduce file size and improve performance of the Power BI file. The parquet file which is utilized in our submission is located [here](https://github.com/bwalzer4/NASA-Pale-Blue-Dot/blob/main/MODIS/fire_archive_M-C61_combined_v2.parquet).

To process the MODIS Burned Area data the HDF files were pulled into Python, using osego and pandas. The osego/gdal library was utilized in navigating through the MODIS Burned Area HDF files. The MODIS Burned Area data is formatted in 720 x 1440 array, where each element in the array represents a grid of 0.25 degrees. We subset this array based on the bounding box mentioned above and reshaped it into a data frame where each row represents the burned area of a single grid for a given month, with the latitude and longitude representing the center of the grid. The Python Notebook for this processing can be found [here](https://github.com/bwalzer4/NASA-Pale-Blue-Dot/blob/main/MODIS/burned%20area%20notebook%20v2.ipynb).






