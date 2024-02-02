# Powerplant Generation Dashboard
## By Martin Bauer
![PP_db](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/62e5a462-cc89-417f-adbd-b78e09cadd82)

### Dashboard Background
I created this interactive report using global powerplant data because I wanted to better understand how geographies differ based on energy sources and generation. This data relies on a collection of data sources ranging from different energy bureaus and organizations.

The biggest problem with this dataset is that there is no electricity generation reported for either China or Russia in any year. For their analysis, the report relies on estimated generation of electricity.

### Steps Followed
1. Extracted data from online sources and load datasets as csv files into PowerBI Desktop; Data includes:
        
        - global_powerplant_database_v_1_3 from Kaggle (https://www.kaggle.com/datasets/eshaan90/global-power-plant-database)
        - Bing Map - Rest API

2. Transformed the datasets using Power Query editor; ensuring data consistency and quality using query features

3. Cleaned the dataset by eliminating outliers, blank values and selecting data types for each column

4. Configured a query to call Rest API from Bing maps in order to convert a specific point (longitude/latitude) into an address

        - This enables the functionality for the Azure maps within PowerBI
        - Getting the address allows for more geographic analysis such as: continent, country, city, etc. 

5. Loaded data into PowerBI and created the model using a star schema design:
### Data Model
![PP_model](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/95805f4d-f30e-4aa4-9dee-989f2b504159)

6. Created a "Years" table to be used within the filter context in order to control how data is used in the report:
### "Years" table
![PP_year](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/87b4a024-5528-41aa-8189-61936f047f24)

7. New Measure: Generation Amount
    
    Since the generation values for each year are separated into separate columns, a special measure needed to be created that is adjusted based on a year filter selected.
    Using DAX code, the "Generation Amount (value)" measure changes based on what year is selected in the dashboard:

#### Generation Amount (Value): 

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

8. Created the global generation map using the new measure, fuel type, geography, and plant names:

### US Nuclear Powerplant View
![PP_nuclear](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/cc42462b-f506-4dca-94a9-e23f2ac7b8cf)

In this view, the map displays blue bubbles across the United States, which represent where nuclear powerplants are located in the US as well as how much power they generated in 2016. 

Although there is a relatively small number of nuclear powerplants in the US (58) compared to other fuel types, they still generated a lot of power (~786K in gwh or gigawatts per hour). Illinois has the most nuclear powerplants in the US at 6 and generates the most nuclear power in the US at about 99K in gwh. It also shows that generation from nuclear powerplants increased +0.18% in 2016 compared to that in 2015.

### US Nuclear Powerplants: Niagara Falls
![PP_niagara](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/17ba5d4b-f6e5-4337-9d95-1f0266f21285)

Another interesting insight is that a plant in Niagara Falls generates one of the highest amounts of hydro energy. This plant's name is the Robert Moses Niagara plant, and in 2016, it generated roughly 15K in gwh, which amounted to 6% of the total US hydro energy output.

9. Created other useful measures:

#### Generation Amount (YoY%): 
- calculates the percentage change in electricity generation from any year selected from the year filter compared to the year prior

        VAR thisyear = [Generation Amount (Value)]
        VAR year = SELECTEDVALUE(Years[Years])
        VAR lastyear = 
            IF(
                year = "2019", SUM(power_plant_data[generation_gwh_2018]), 
                IF(
                    year = "2018", SUM(power_plant_data[generation_gwh_2017]),
                    IF(
                        year = "2017", SUM(power_plant_data[generation_gwh_2016]),
                        IF(
                            year = "2016", SUM(power_plant_data[generation_gwh_2015]),
                            IF(
                                year = "2015", SUM(power_plant_data[generation_gwh_2014]),
                                IF(
                                    year = "2014", SUM(power_plant_data[generation_gwh_2013])
                                )
                            )
                        )
                    )
                )
            )
        VAR diff = thisyear - lastyear
        VAR Result = DIVIDE(diff, lastyear)
        RETURN 
            IF(
                year = "2013", "Not available", Result
            )

#### Comparison (%) 
- calculates how accurate the reported generation amount of any given year is to the estimated generation amount, given as a percentage

        VAR actual = [Generation Amount (Value)] 
        VAR est = [Estimated Generation (Value)]
        VAR year = SELECTEDVALUE(Years[Years])
        RETURN 
            IF(
                NOT year IN {"2018", "2019"}, 
                DIVIDE(actual, est),
                "Not Available"
            )

#### Number of Powerplants 
- calculates the number of power plants in a given area

        DISTINCTCOUNT(power_plant_data[name])

#### Rank Countries
- rank orders all countries given in a filter context by their estimated generation amount in a given year

        RANKX(
            ALLSELECTED(Location[Country (Long)]), 
            [Estimated Generation (Value)]
        )

#### Capacity
- calculates the generating capacity of any given variable

        SUM(power_plant_data[Electrical Generating Capacity])

#### Capacity %
- calculates any given country's generating capacity compared to that of all other countries, as a percentage

        VAR cap = SUM(power_plant_data[Electrical Generating Capacity])
        VAR total_cap =
            CALCULATE(
                [Capacity],
                ALLSELECTED(Location[Country (Long)])
            )
        RETURN 
            DIVIDE(cap, total_cap)

#### Powerplant %
- calculates any given country's number of powerplants compared to that of all other countries, as a percentage

        VAR pp = [Powerplants]
        VAR total_pp =
            CALCULATE(
                [Powerplants],
                ALLSELECTED(Location[Country (Long)])
            )
        RETURN 
            DIVIDE(pp, total_pp)

#### Fuel Type Max
- calculates the generation amount of the fuel type that generates the most power for any given variable

        MAXX(
            VALUES(power_plant_data[Fuel Type]),
            [Generation Amount (Value)]
        )

#### Fuel Type Max Name
- provides the name of the fuel type that generates the most power for any given variable

        VAR max_val = [Fuel Type Max]
        VAR fuel = 
            FILTER(
                VALUES(power_plant_data[Fuel Type]),
                [Generation Amount (Value)] = max_val
            )
        VAR Result =
            IF( max_val > 0, fuel, "Gas")
        Return Result

10. Created visualizations using the data and measures to better understand how powerplants might differ across the world as it pertains to number of plants, energy sources, capacity, and generation.

# Insights



