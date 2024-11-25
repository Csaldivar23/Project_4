# Predicting Avocado Prices with Machine Learning

## Overview
The purpose of this project is to build a Machine Learning model that can predict the future average price of avocados.
Our goal is to build a model with a high predictive accuracy, aiming for an R² value of at least 0.80.

## Data Source
The dataset used in this project originates from the Hass Avocado Board (HAB). However, we sourced our data from Kaggle, 
where it was uploaded by user 'VakhariaPujan'. This dataset is an updated version of two earlier datasets from users 
'Justin Kiggins' and 'Valentin Joseph', all of which are based on the original data from the HAB.

## Cleaning
The first step was to normalize the data into its 3rd normal form. Here's the process we followed:

### Initial Review and Cleaning:

Downloaded the CSV file from Kaggle and loaded it into a notebook.
Converted the date column from an object to datetime format.
Dropped rows with values for 'California', 'GreatLakes', 'Midsouth', 'Northeast', 'Plains', 'SouthCentral', 'Southeast', 'West', and 'TotalUS' 
as they represented total market values for each year, which would skew the year-over-year data analysis.

### Mapping Regions to Markets:

Created a dictionary (country_map) to define which regions belong to each market, based on the Hass Avocado Board's Market Mapping.
Reversed the dictionary to map regions (keys) to their corresponding markets (values).
Added a new market column to the dataframe by mapping each region to its respective market.

### Creating Supporting Tables for SQL Import:

Extracted unique values from key columns (type, region, market) to create separate dataframes:
type_df: Contains unique type values (conventional, organic), saved as type_index.csv.
region_df: Contains unique region names, saved as region_index.csv.
market_df: Contains unique market names, saved as market_index.csv.
Replaced string values in type, region, and market columns in avocado_df with their respective indices from these supporting tables.

### Final Normalized Dataframe:

Saved the updated avocado_df, with type, region, and market columns converted to numerical indices, as avocados_index.csv.
These indices establish relationships with their respective tables (type_df, region_df, market_df) for integration into a PostgreSQL database.

## Database Design and Implementation

### Entity-Relationship Diagram (ERD)


### PostgreSQL Database Setup


## Preprocessing



## Training



## Results



## Summary



## Links

**Data Sources:**

* Kaggle Dataset:

      https://www.kaggle.com/datasets/vakhariapujan/avocado-prices-and-sales-volume-2015-2023

* Hass Avocado Board:

      https://hassavocadoboard.com/

**Cleaning Notebook:**

* Drop Values Reference:

      https://www.youtube.com/watch?v=xCax4DLOKPA
 
* HAB Market Mapping:

      https://hassavocadoboard.com/category-data/

* Binning String Values:

      https://stackoverflow.com/questions/59757095/how-to-bin-string-values-according-to-list-of-strings

**Preprocessing:**

* Connecting PostgreSQL with SQLAlchemy:

      https://www.geeksforgeeks.org/connecting-postgresql-with-sqlalchemy-in-python/