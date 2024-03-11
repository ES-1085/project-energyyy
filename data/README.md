**Data**

**table_Residential**

This data set contains information for electricity sales in the residential sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity` : utility company

- `State`: US state in which the utility company operates
- `Ownership`: type of ownership, `Political subdivision` is a sovereign subdivision of any State or local government; `Behind the Meter` is a microsystem disconnected from the national electricity grid, such as a residential unit providing and consuming their own electricity; `Retail Power Marketer` are intermediaries between the retail buyer and the generator
- `Customers (Count)`: Count of households that receive their electricity from the entity, determined by number of electricity meters; includes private households and apartment buildings where energy is consumed primarily for space heating, water heating, air conditioning, lighting, refrigeration, cooking, and clothes drying
- `Sales (Megawatthours)`: amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)`: average price of electricity in cents/kWh sold by the entity in 2022


**table_Commercial**

This data set contains information for electricity sales in the commercial sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity`: utility company

- `State`: US state in which the utility company operates
- `Ownership`: type of ownership, `Political subdivision` is a sovereign subdivision of any State or local government; `Behind the Meter` is a microsystem disconnected from the national electricity grid, such as a residential unit providing and consuming their own electricity; `Retail Power Marketer` are intermediaries between the retail buyer and the generator
- `Customers (Count)`: Count of businesses that receive their electricity from the entity, determined by number of electricity meters, includes public streets and highway lighting, counting one customer per community; includes nonmanufacturing business establishments such as: hotels, motels, restaurants, wholesale businesses, retail stores, and health, social, and educational institutions, public street and highway lighting, municipalities, divisions of agencies of states and federal governments under special contracts or agreements, and other utility departments, such as defined by the pertinent regulatory agency and/or electric utility. 
- `Sales (Megawatthours)` : amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)`: average price of electricity in cents/kWh sold by the entity in 2022


**table_Industrial**

This data set contains information for electricity sales in the industrial sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity` : utility company

- `State` : US state in which the utility company operates
- `Ownership` : type of ownership, `Political subdivision` is a sovereign subdivision of any State or local government; `Behind the Meter` is a microsystem disconnected from the national electricity grid, such as a residential unit providing and consuming their own electricity; `Retail Power Marketer` are intermediaries between the retail buyer and the generator
- `Customers (Count)`: Count of companies that receive their electricity from the entity, includes irrigation; includes: manufacturing, construction, mining, agriculture (irrigation), fishing, and forestry establishments. 
- `Sales (Megawatthours)` : amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)` : average price of electricity in cents/kWh sold by the entity in 2022


**table_Transportation**

This data set contains information for electricity sales in the transportation sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity` : utility company

- `State` : US state in which the utility company operates
- `Ownership` : type of ownership, `Political subdivision` is a sovereign subdivision of any State or local government; `Behind the Meter` is a microsystem disconnected from the national electricity grid, such as a residential unit providing and consuming their own electricity; `Retail Power Marketer` are intermediaries between the retail buyer and the generator
- `Customers (Count)`: number of customers for electric energy supplied for transportation purposes; includes railroads and railways (the fuel source of propulsion must be electrical like a metro system which exists in large cities), count in number of systems not meters.  
- `Sales (Megawatthours)` : amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)` : average price of electricity in cents/kWh sold by the entity in 2022

**table_Disturbance**

This data contains information for electricity disturbance events, so called black-outs, in 2022.

- `Year` : Year in which event occurred
- `Month`: Month in which event occurred
- `Event Date and Time`: Date and Time at which event occurred
- `Restoration Date and Time` : Amount of time it took the provider to restore electricity flow and time of end of disturbance event
- `Duration` : Duration of the event
- `Utility/Power Pool`: Utility or Power Pool in which event occurred
- `NERC Region`: Regional cohort which is assigned by the North American Electric Reliability Corporation (NERC)
- `Area Affected`: Affected area in NERC region
- `Type of Disturbance`: The events are categorized by how many customers are affected by the disturbance event and by the reason for the disturbance, e.g. severe weather. 
- `Loss (megawatts)`: Amount of electricity (in megawatts) lost to the environment due to the event and not distributed to the customer. 
- `Number of Customers Affected`: The number of customers affected by event, includes all the customers mentioned above.

**table_CAIDI**

This data contains information for Customer Average Interruption Duration Index (CAIDI) from 2013 to 2022 for each US state collected by the EIA - Energy Information Administration (Source: https://youtu.be/oVH9L0fCMTU?si=MPggdP48hKrQpERI). 

- `Census Division\r\nand State` The state or the Census Division e.g New England. 
- `Major Event Days` TRUE = CAIDI calculated includes Major Event Days, such as storms, hurricanes, etc., FALSE = CAIDI calculated excludes Major Event Days; Major Event Days are calculated using the formula SAIDI > Tmed = e^(α+2.5β) 
To minimize weather impact on the CAIDI. 
- `Year` The year of CAIDI index.
- `CAIDI` is calcualted from the SAIFI and SAIDI which are the following values: 
SAIFI – The System Average Interruption Frequency Index

SAIDI – The System Average Interruption Duration Index

The SAIFI and SAIDI collected are the yearly summed values of all non-momentary outages. For utilities that use IEEE standards, non-momentary is defined by any outage lasting more than five minutes.

SAIFI: number of customers affected/number of customers in the system or  % of customers affected

SAIDI: Minutes of power out * (number of customers affected/ number of customers in the system) OR Minutes of power out * SAIFI

**CAIDI = SAIDI/SAIFI**


**USA_States_Generalized**

This dataframe contains general data for each of the US states and gives us the geometry to connect to our data. 