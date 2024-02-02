# Powerplant Generation Dashboard
## By Martin Bauer
![PP_db](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/62e5a462-cc89-417f-adbd-b78e09cadd82)

![PP_genbycountry](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/f0cdbd86-a4e7-4baf-a45d-78c0b0c9a8c0)

![PP_genbycontinent](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/93c70728-f968-49d0-ab5a-7956d0a9bf1e)

### Dashboard Background
I created this interactive report using global powerplant data because I wanted to better understand how geographies differ based on energy sources and generation. This data relies on a collection of data sources ranging from different energy bureaus and organizations. The database includes about 28,500 power plants from 164 countries.

The biggest problem with this dataset is that it does not paint the most accurate picture of global generation. Not all powerplants or countries are included in the dataset as it is difficult to centralize all of that information. Also, there is no electricity generation reported for either China or Russia in any year. For their analysis, the report relies on estimated generation of electricity.

### Steps:
- Extracted data from online sources and connected the datasets as csv files to PowerBI Desktop; Data includes:
        
        - global_powerplant_database_v_1_3 from Kaggle (https://www.kaggle.com/datasets/eshaan90/global-power-plant-database)
        - Bing Map - Rest API

- Transformed the datasets using Power Query editor; ensuring data consistency and quality using query features

- Cleaned the dataset by eliminating outliers, blank values and selecting data types for each column

- Configured a query to call Rest API from Bing maps in order to convert a specific point (longitude/latitude) into an address

        - This enables the functionality for the Azure maps within PowerBI
        - Getting the address allows for more geographic analysis such as: continent, country, city, etc. 

- Loaded the data into the report view and created the model using a star schema design:
#### Data Model
![PP_model](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/95805f4d-f30e-4aa4-9dee-989f2b504159)

- Created a "Years" table to be used within the filter context in order to control how data is used in the report

- Created a new measure called "Generation Amount (value)"
    - Since the generation values for each year are separated into separate columns, a special measure needed to be created that is adjusted based on a year filter selected.
    - Using DAX code, the "Generation Amount (value)" measure changes based on what year is selected in the dashboard.

- Created other useful measures in order to produce visualizations
####  (Scroll down to the bottom of the page to see the DAX code written for each measure created)

- Created the global generation map using the new measure, fuel type, geography, and plant names

- Created visualizations in PowerBI using the data and measures to better understand how powerplants might differ across the world as it pertains to number of plants, energy sources, capacity, and generation.

# Insights

## Global Generation by Fuel Type
![PP_topfuel](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/4503d8a3-3047-41ec-8368-e9c1d678da97)

This first visual gives a high-level view of the yearly reported generation across the globe sliced by fuel type. While the reported generation data is incomplete (no reported data from China or Russia), there is still interesting insights to be gathered. 

It's worth noting that coal was the leading energy source world-wide from 2016 to 2018 and produced almost more than double the energy output compared to the second leading energy source, gas. Coal was eventually overtaken by gas in 2019 as the leading energy source worldwide. 

Nuclear energy has been producing the most energy output compared to other sustainable energy sources like Wind or Solar during that whole timespan.

## 2017 Generation Comparison by Continent
![PP_continent](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/27fbefa9-7279-4082-bb2f-8678e63853cc)

![PP_continent_bar](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/5d70898c-479e-4090-96be-d3f461c3589e)

These visuals highlight the difference between estimated generation compared to reported generation across continents. It's revealing that in 2017 the entire energy output in Asia (9.2M in gwh) almost surpassed the energy output of all other continents combined (9.4M in gwh). 

However, this is not reflected in the reported generation output, which shows Asia's output at 1.1M in gwh. Instead, North America has the highest reported energy output in 2017 at 3.5M in gwh.

## 2017 Generation Comparison by Country
![PP_est_countries](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/bac284f9-2fd2-443a-b4cc-b4bd648a23d6)
![PP_est_countries_bar](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/10b35ddf-0d71-4470-b724-bcc97db05086)

These visuals display the top ten countries with the highest estimated generation outputs as well as their reported generation, and China had the highest estimated output in 2017 at 5.7M, which is about 28% of the world's estimated output. The United States had the next highest estimated output at 4.7M.

These visuals also show which countries lack the reported generation data, most notably China, Russia, Canada, and Japan.

## Electrical Generating Capacity by Country
![PP_capacity_country](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/159525d4-7897-47cd-8439-9d08b5792513)
![PP_capacity_country_%](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/6d405b6b-41ca-4c13-8a11-1ba3f77e0237)

This section describes the differences in how much electricity countries can generate given the estimated powerplant capacity data. China (29%) and the United States (27%) provide almost half of the world's electrical generating capacity, which is consistent with how much estimated power they generated in 2017. Also, 78% of the world's power is generated by these ten countries. The generating capacity of those 10 countries comes out to about 4.5M in gwh.

## Electrical Generating Capacity by Fuel Type
![PP_capacity_fuel_type](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/7355ed24-6a1e-44a4-8146-b48fcce19eca)
![PP_capacity_fuel_type_%](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/ca9d09fa-1b13-4c71-a847-e60101ffe093)


Coal powerplants (38%) have the most electrical generating capacity out of all energy sources worldwide. The next energy sources by capacity include: Gas (24%), Hydro (18%), Nuclear (7%), and Wind (5%).

## Number of Powerplants by Country
![PP_pp_countries](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/e9911210-963a-4eb2-a1de-cf220d06b1ad)
![PP_pp_countries_tb](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/a7819f3f-af9e-464c-9463-fff53db54d80)

Based on the global powerplant database, 37% (or 9,809) of the world's power plants are located in the United States, and about one third of US plants provide Solar energy. While Solar energy sources surpass all other fuel types, Solar has the 6th highest energy producing capacity.

China has the second most power plants worldwide at 15% (or 4,039 plants), and half of Chinese plants provide Solar or Hydro energy. 

## Number of Powerplants by Fuel Type
![PP_pp_fuel](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/34055019-fb84-4f31-9a6d-654b8fd7def7)
![PP_pp_fuel%](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/40542c19-ebdb-4b54-a04f-ca9066361ab8)

Solar (32%), Hydro (21%), and Wind (15%) represent the majority of energy sources by count worldwide. However, these energy sources only carry about 27% of the world's electrical generating capacity. 

Another interesting insight gathered from the data is that of the 1,976 Coal powerplants worldwide, 45% of these plants are located in China. This means that since Coal energy sources provide 38% of generating power worldwide, China can generate 17% of the world's energy just from Coal plants.


## Dominant Fuel Type by State
![PP_US_map](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/5188a63b-7967-46d4-8968-96cfc4f53138)

Looking solely at the United States, this visual displays which energy source generated the most power in 2017 within that state. While the legend shows each energy source and its corresponding color, the user can hover over each state to view the dominant fuel type as well as how much power was generated within that state.

The first insight that I gathered from this map is that the Southern states, California, Nevada, and Alaska are dominated by Gas while the Northern states and some Western states are dominated by Coal. Hawaii is the only state dominated by Oil.

Some states like Washington, Oregon, and Maine are dominated by Hydro, which makes sense given that they are coastal states. Also, they are many states such as: Illinois, Arizona, the Carlinas, and New York that are dominated by Nuclear. 

![PP_state_fuel](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/d65456d2-d8a6-4bd8-8908-3ae3d37204f5)

While the map can visually show which states rely on certain fuel types, this table highlights the dominant fuel type for each state as well as how much power that fuel type generates within that state. 

Texas leads all states in its dominant fuel type generation with Gas plants generating around 198K in gwh. Texas is followed by: Florida (Gas: 119K in gwh), Washington (Hydro: 106K in gwh), Illinois (Nuclear: 97K in gwh), and California (Gas: 82K in gwh). 

While the fuel type with the most sources in the United States is Solar power, it is overpowered in generation by Gas, Nuclear, and Coal.

### US Nuclear Powerplant View
![PP_nuclear](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/cc42462b-f506-4dca-94a9-e23f2ac7b8cf)

In this view, the map displays blue bubbles across the United States, which represent where nuclear powerplants are located in the US as well as how much power they generated in 2016. 

Although there is a relatively small number of nuclear powerplants in the US (58) compared to other fuel types, they still generated a lot of power (~786K in gwh). Illinois has the most nuclear powerplants in the US at 6 and generates the most nuclear power in the US at about 99K in gwh. It also shows that generation from nuclear powerplants increased +0.18% in 2016 compared to that in 2015.

### US Hydro Powerplants: Niagara Falls
![PP_niagara](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/17ba5d4b-f6e5-4337-9d95-1f0266f21285)

Another interesting insight is that a plant in Niagara Falls generates one of the highest amounts of hydro energy. This plant's name is the Robert Moses Niagara plant, and in 2016, it generated roughly 15K in gwh, which amounted to 6% of the total US hydro energy output.

## Michigan - Nuclear Powerplants
![PP_Michigan](https://github.com/martinbauer1/Powerplant-Data-Dashboard/assets/154390228/63d6f3ed-3513-46c2-982b-409348d7ab7c)

Lastly, I wanted to know how many nuclear powerplants are in my home state of Michigan and how much nuclear power is generated here. There are 3 nuclear powerplants in Michigan:

    1. Donald C Cook (Bridgman, MI): 18K in gwh
    2. Fermi (Frenchtown Charter Township, MI): 9K in gwh
    3. Palisades (Covert, MI): 6K in gwh

### DAX Code:
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

#### Estimated Generation (Value) 
- calculates the estimated generation value for any year selected in the year filter for a given area

        VAR year = SELECTEDVALUE(Years[Years]) 
        VAR Result = 
            IF(
                year = "2019", "Not Available", 
                IF(
                    year = "2018", "Not Available",
                    IF(
                        year = "2017", SUM(power_plant_data[estimated_generation_gwh_2017]),
                        IF(
                            year = "2016", SUM(power_plant_data[estimated_generation_gwh_2016]),
                            IF(
                                year = "2015", SUM(power_plant_data[estimated_generation_gwh_2015]),
                                IF(
                                    year = "2014", SUM(power_plant_data[estimated_generation_gwh_2014]),
                                    IF(
                                        year = "2013", SUM(power_plant_data[estimated_generation_gwh_2013]))
                                )
                            )
                        )
                    )
                )
            )
        RETURN Result

#### Estimated Generation (Format) 
- calculates the estimated generation value for any year selected in the year filter for a given area and displays the value in a consumable format

        VAR year = SELECTEDVALUE(Years[Years]) 
        VAR Check_year = 
            IF(
                year = "2019", "Not Available", 
                IF(
                    year = "2018", "Not Available",
                    IF(
                        year = "2017", SUM(power_plant_data[estimated_generation_gwh_2017]),
                        IF(
                            year = "2016", SUM(power_plant_data[estimated_generation_gwh_2016]),
                            IF(
                                year = "2015", SUM(power_plant_data[estimated_generation_gwh_2015]),
                                IF(
                                    year = "2014", SUM(power_plant_data[estimated_generation_gwh_2014]),
                                    IF(
                                        year = "2013", SUM(power_plant_data[estimated_generation_gwh_2013]))
                                )
                            )
                        )
                    )
                )
            )
        VAR Result = 
            IF(
                Check_year > 1000000, FORMAT(Check_year, "#,##0,,.0M"),
                VAR act_value = DIVIDE(Check_year, 1000)
                RETURN 
                    IF( 
                        act_value > 0, 
                        IF(
                            act_value >= 1,
                            FORMAT(act_value, "#") & "K",
                            "<1K"
                        )
                    )
            )
        RETURN Result

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


## Thank You!
