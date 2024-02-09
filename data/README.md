Data

table_Residential

This data set contains information for electricity sales in the residential sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity` : utility company

- `State`: US state in which the utility company operates
- `Ownership`: type of ownership
- `Customers (Count)`: Count of households that receive their electricity from the entity, determined by number of electricity meters; includes private households and apartment buildings where energy is consumed primarily for space heating, water heating, air conditioning, lighting, refrigeration, cooking, and clothes drying
- `Sales (Megawatthours)`: amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)`: average price of electricity in cents/kWh sold by the entity in 2022


table_Commercial

This data set contains information for electricity sales in the commercial sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity`: utility company

- `State`: US state in which the utility company operates
- `Ownership`: type of ownership
- `Customers (Count)`: Count of businesses that receive their electricity from the entity, determined by number of electricity meters, includes public streets and highway lighting, counting one customer per community; includes nonmanufacturing business establishments such as: hotels, motels, restaurants, wholesale businesses, retail stores, and health, social, and educational institutions, public street and highway lighting, municipalities, divisions of agencies of states and federal governments under special contracts or agreements, and other utility departments, such as defined by the pertinent regulatory agency and/or electric utility. 
- `Sales (Megawatthours)` : amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)`: average price of electricity in cents/kWh sold by the entity in 2022


table_Industrial

This data set contains information for electricity sales in the industrial sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity` : utility company

- `State` : US state in which the utility company operates
- `Ownership` : type of ownership
- `Customers (Count)`: Count of companies that receive their electricity from the entity, includes irrigation; includes: manufacturing, construction, mining, agriculture (irrigation), fishing, and forestry establishments. 
- `Sales (Megawatthours)` : amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)` : average price of electricity in cents/kWh sold by the entity in 2022


table_Transportation

This data set contains information for electricity sales in the transportation sector 2022. It contains 1526 rows representing individual utility companies. (Data from forms EIA-861- schedules 4A & 4D and EIA-861S)
- `Entity` : utility company

- `State` : US state in which the utility company operates
- `Ownership` : type of ownership
- `Customers (Count)`: number of customers for electric energy supplied for transportation purposes; includes railroads and railways (the fuel source of propulsion must be electrical like a metro system which exists in large cities), count in number of systems not meters.  
- `Sales (Megawatthours)` : amount of electricity sold by said entity in megawatthours in 2022
- `Revenues (Thousand Dollars)`: profit made by the entity in 2022
- `Average Price (cents/kWh)` : average price of electricity in cents/kWh sold by the entity in 2022

table_Disturbance

This data contains information for electricity disturbance events, so called black-outs, in 2022.

- `Year` : Year in which event occurred
- `Month`: Month in which event occurred
- `Event Date and Time`: Date and Time in which event occurred
- `Restoration Date and Time` : Restoration Date and Time, end of event
- `Duration` : Duration of the event
- `Utility/Power Pool`: Utility or Power Pool in which event occurred
- `NERC Region`: Regional cohort which is assigned by the North American Electric Reliability Corporation (NERC)
- `Area Affected`: Affected area in NERC region
- `Type of Disturbance`: The events are categorized by how many customers are affected by the disturbance event and by the reason for the disturbance, e.g. severe weather. 
- `Loss (megawatts)`: Amount of electricity (in megawatts) lost to the environment due to the event and not distributed to the customer. 
- `Number of Customers Affected`: The number of customers affected by event, includes all the customers mentioned above.

