
# Powerplant-Generation-Dashboard
![PP_db](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/62e5a462-cc89-417f-adbd-b78e09cadd82)

### Dashboard Background:
I created this interactive report using global powerplant data because I wanted to better understand how geographies differ based on energy sources and generation. This data relies on a collection of data sources ranging from different energy bureaus and organizations.

The biggest problem with this dataset is that there is no electricity generation reported for either China or Russia in any year. For their analysis, the report relies on estimated generation of electricity.

(ADvice or findings)

### Steps Followed:
1. Extracted data from online sources and load datasets as csv files into PowerBI Desktop; Data includes:
        
        - global_powerplant_database_v_1_3 from Kaggle (https://www.kaggle.com/datasets/eshaan90/global-power-plant-database)
        - Bing Map - Rest API

2. Transformed the datasets using Power Query editor; ensuring data consistency and quality using query features

3. Cleaned the dataset by eliminating outliers, blank values and selecting data types for each column

4. Configured a query to call Rest API from Bing maps in order to convert a specific point (longitude/latitude) into an address

        - This enables the functionality for the Azure maps within PowerBI
        - Getting the address allows for more geographic analysis such as: continent, country, city, etc. 

5. Loaded data into PowerBI and created the data model using a star schema design:

![PP_model](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/95805f4d-f30e-4aa4-9dee-989f2b504159)

6. New Measure: Generation Amount
    
    Since the generation values for each year are separated into separate columns, a special measure needed to be created that is adjusted based on a year filter selected.
    Using DAX code, the "Generation Amount (value)" measure changes based on what year is selected in the dashboard:

    Generation Amount (Value) = 

        VAR year = SELECTEDVALUE(Years[Years]) 
        VAR Result = 
            IF(
                year = "2019", SUM(power_plant_data[generation_gwh_2019]), 
                IF(
                    year = "2018", SUM(power_plant_data[generation_gwh_2018]),
                    IF(
                        year = "2017", SUM(power_plant_data[generation_gwh_2017]),
                        IF(
                            year = "2016", SUM(power_plant_data[generation_gwh_2016]),
                            IF(
                                year = "2015", SUM(power_plant_data[generation_gwh_2015]),
                                IF(
                                    year = "2014", SUM(power_plant_data[generation_gwh_2014]),
                                    IF(
                                        year = "2013", SUM(power_plant_data[generation_gwh_2013]))
                                )
                            )
                        )
                    )
                )
            )
        RETURN Result  

7. Created the global generation map using the new measure, fuel type, geography, and plant names:
![PP_nuclear](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/cc42462b-f506-4dca-94a9-e23f2ac7b8cf)

# Global Map

# Worldwide Insights

# United States Insights




