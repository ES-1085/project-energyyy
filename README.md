The Grid
================
by Energyyy

## Summary

The U.S.A. has one of the most diverse electric grids in the world. With
that many stakeholders it is interesting to know which factors affect
electricity distribution and pricing in the US and to what extent. As a
group we looked into data from the U.S. Energy Information
Administration (EIA). The EIA collects, analyzes, and disseminates
independent and impartial information on the US energy sector. The
utility companies are required to self-report this data. In our analysis
we take data from six different datasets published by the EIA for the
year 2022, four concerning four different sectors, namely residential,
commercial, industrial, and transportation, which included the following
variables: ownership type, customers, sales (Megawatt hours), revenues
(Thousands \$), and average price (cents/kWh), and US state. The other
two contain information on the reliability of the US electricity
distribution system, one listing every disturbance that occurred in the
system and the second one listing the CAIDI - a reliability index - for
each utility company.

**Analysis**

After joining the four datasets for each energy sector, we checked for
missing data points. The datasets are relatively complete, with only 3%
missing data points.

We created a boxplot faceted by sector looking at the average price per
ownership to see how ownership affects price. We found that
federally-owned utility companies offer below average prices in all
sectors. All other ownerships do not deviate significantly from the
average price. We do not pay a lot of attention to the transportation
sector, because there are only 43 data points in comparison to more than
1500 data entries for the commercial and residential sector.

To figure out how blackouts, revenue and ownership relate to each other,
we made a scatter plot with ownership type versus revenue and point size
representing the number of disturbances. We saw that most disturbances
occur in investor-owned companies. Revenue does not seem to play a major
role in how many disturbances occur. Analysing our data further, we saw
that there are not more investor-owned companies than other ownership
types, but we found that they are the ones with the most customers.
Plotting the average number of customers versus the average number of
disturbances per type of ownership, we concluded that there is a slight
positive correlation between number of customers per type of ownership
and number of disturbances.

To visualize how reliability varies throughout the US, we joined our
data of CAIDI with a shapefile and plotted it in a leaflet and as a
ggplot. West Virginia and Michigan are consistently suffering from low
reliability in electricity supply while Florida and North Dakota have a
low number of interruptions. There are no dramatic changes for any of
the states over time.

To see how the amount of electricity consumed per customer affects
price, we plotted average consumption per customer versus average price
and found that a historic tendency towards the industrial sector still
exists today as the industrial sector with the highest electricity
consumption per consumer still gets the lowest average prices. There
does not, however, seem to be a general trend of lower prices for higher
electricity consumption per customer as none of the facets show a
negative trendline.

Focusing on the residential sector, we visualized the number of
customers of each utility company against average price to see whether
there is a positive effect of scale on the price. Scale does not seem to
play an important role in determining prices as average price does not
lower with an increase in the number of customers. In fact, there is a
slight opposite trend. We also looked at average electricity consumption
per customer versus average price to see whether price affects people’s
consumption. Using the trendline, we concluded that there is a slight
negative correlation between average electricity consumption per
customer and average price and the per household consumption of
electricity is slightly lower when the price is high.

**Main findings**

The industrial sector has the lowest average price. There is no evidence
in the data that we found, that a specific ownership type coincides with
lower prices. Neither is ownership related to more reliability in the
electricity distribution system.

**Limitations and implications**

How the price comes together, depends on many different factors, many of
which are not under the control of the utility companies and thus, are
not part of our analysis. We would have liked to do more spatial and
economic analysis if time allowed and specifically, we would like to
build on our last visualization to see whether we can see a starker
contrast between the poorest and richest US states and people’s behavior
in relation to low and high prices.

## Presentation

Our presentation can be found
[here](https://www.canva.com/design/DAF_JI5ifwk/mX4bhovu_dcmp6Dzkyk_5w/edit?utm_content=DAF_JI5ifwk&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)
on canva. The leaflet can be found
[here](https://rawcdn.githack.com/ES-1085/project-energyyy/d3fe2af451a85b6fd8cb3f8b6c724995ff545780/workspace/leaflet.html)
when you scroll to the bottom.

## Data

Energy Information Administration. 2023. 2022 Utility Bundled Retail
Sales- Residential \[Data set\]. EIA.
<https://www.eia.gov./electricity/sales_revenue_price/pdf/table_6.pdf>.
Retrieved on 01/30/2024.

Energy Information Administration. 2023. 2022 Utility Bundled Retail
Sales- Commercial \[Data set\].
EIA.https://www.eia.gov./electricity/sales_revenue_price/pdf/table_7.pdf.
Retrieved on 01/30/2024.

Energy Information Administration. 2023. 2022 Utility Bundled Retail
Sales- Industrial \[Data set\].
EIA.https://www.eia.gov./electricity/sales_revenue_price/pdf/table_8.pdf.
Retrieved on 01/30/2024.

Energy Information Administration. 2023. 2022 Utility Bundled Retail
Sales- Transportation \[Data set\].
EIA.https://www.eia.gov./electricity/sales_revenue_price/pdf/table_9.pdf.
Retrieved on 01/30/2024.

Energy Information Administration. 2023. Table 11.6 CAIDI Values
(Minutes Per Interruption) of U.S. Distribution System by State, 2013 -
2022 \[Data set\].
EIA.https://www.eia.gov./electricity/annual/html/epa_11_06.html.
Retrieved on 01/30/2024.

Energy Information Administration. 2023. Table B.2 Major Disturbances
and Unusual Occurrences, 2022 \[Data set\].
EIA.https://www.eia.gov./electricity/monthly/xls/table_b_2.xlsx.
Retrieved on 01/30/2024.

ESRI Data and Maps. 2017. USA States (Generalized) \[Data set\]. Esri.
<https://hub.arcgis.com/datasets/esri>::usa-states-generalized/explore.
Retrieved on 01/30/2024.

## References

Bakke, G., 2016. “The Grid: The Fraying Wires Between Americans and Our
Energy Future”. Bloomsbury, USA.
