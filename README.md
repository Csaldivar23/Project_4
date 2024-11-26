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

![avocado_ERD](https://github.com/user-attachments/assets/fc26dfd6-56ee-4327-b0bb-202a15325598)

### PostgreSQL Database Setup

The schema consists of four tables: type, region, market, and avocados.
Type, region, and market are lookup tables containing descriptive data (e.g., avocado types, regions, and markets).
Avocados contains sales data, including price, volume, and bag types, and links to the other tables through foreign keys.
The avocados table has foreign keys that link to type, region, and market by their respective Index values, establishing relationships between the data.

## Preprocessing
Using SQLalchemy we created an engine and reflected our database into all of our classes. We ended up with four classes representing each table from our Avacado Database.
We used read_sql and created a series of joins for all ofo ur classes to create the avocado_df. We dropped the unneccesry columns from the joins we excecuted. 'type_', 'region_', 
'market_', 'index_1', 'index_2', 'index_3' We then coverted the date column to datetime so that we could extract the month. The new month column was binned to Quarter 1, 2, 3, and 4.
It was then cast as a string in order for our model to read it. We then dropped the index because it contains unique information which would interfere with the models accuracy.

## Training
We created a scaler from StandardScaler using scikit-learn. Using the scaler we scaled 'plu4046', 'plu4225', 'plu4770', 'totalbags'. We then transformed the scaled data and labeled it scaled data.
We defined our 'y' (terget variable) as 'averageprice'. Our 'X' (features) consisted of 'plu4046', 'plu4225', 'plu4770', 'totalbags', 'region', 'market', 'type', 'quarter'.

`Every region is within a market and every market cotains the regions. So one of these features must always be dropped when defining X to avoid duplicate data.`

We then casted all of our object types using get_dummies. It is important to note that the data must be scaled prior to using get_dummies to avoid scaling the dummies.
We then split the dataset using scikit-learn's train_test_split.

After training the data was fit to the model and predictions were made. 

## Results

* Random Forest Regressor

        The mean absolute value is: 0.1158009761226277
        The mean square error is: 0.026179730858605355
        The root mean square error is: 0.16180151686126232
        The R squared is: 0.836470979103074
  
* Decision Tree Regression

* Linear Regression

* Nueral Network


## Summary
Over the course of this project we've concluded that the Random Forest Regression Model yielded our highet r2 score at '0.84'.

This model can used to price the avocado markets.

Moving forward, some considerations for improving prediction accuracy could be:

* hyperparameter tuning
* obtaining more data and features
* using a time analysis model


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

