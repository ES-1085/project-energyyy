Project Workspace
================
Energyyy

``` r
library(knitr)
knitr::opts_chunk$set(message = FALSE, warning = FALSE)
```

``` r
library(tidyverse)
library(broom)
library(tidyr)
library(readxl)
#install.packages("leaflet")
library(leaflet)
#install.packages("sf")
library(sf)
#install.packages("gganimate")
library(gganimate)
library(ggthemes)
library(gapminder)
```

``` r
table_Residential <- read_excel("/cloud/project/data/table_Residential.xlsx",
                                na = c("."))
table_Commercial <- read_excel("/cloud/project/data/table_Commercial.xlsx",
                               na = c("."))
table_Industrial <- read_excel("/cloud/project/data/table_Industrial.xlsx", 
                               na = c("."))
table_Transportation <- read_excel("/cloud/project/data/table_Transportation.xlsx", 
                                   na = c("."))
table_Disturbance <- read_excel("/cloud/project/data/table_Disturbance.xlsx",
                                na= c(".", ". Hours,  . Minutes", "Unknow", ".        ."))
table_CAIDI <- read_excel("/cloud/project/data/table_CAIDI.xlsx")

USA_df <- sf::st_read("/cloud/project/data/USA_States_Generalized.shp")
```

    ## Reading layer `USA_States_Generalized' from data source 
    ##   `/cloud/project/data/USA_States_Generalized.shp' using driver `ESRI Shapefile'
    ## Simple feature collection with 51 features and 56 fields
    ## Geometry type: MULTIPOLYGON
    ## Dimension:     XY
    ## Bounding box:  xmin: -178.2176 ymin: 18.92179 xmax: -66.96927 ymax: 71.40624
    ## Geodetic CRS:  WGS 84

This mutate function adds a column which contains the sector of the
data.

``` r
USA_df <- USA_df %>%
 rename(`Census Division\r\nand State` = STATE_NAME)
residential <- table_Residential %>%
  mutate(Sector = c("Residential"))
commercial <- table_Commercial %>%
  mutate(Sector = c("Commercial"))
transportation <- table_Transportation %>%
  mutate(Sector = c("Transportation"))
industrial <- table_Industrial %>%
  mutate(Sector = c("Industrial"))
```

This code binds all the tables with sectors together.

``` r
energy_sector <- bind_rows(residential, commercial, industrial, transportation)
```

``` r
table_Disturbance <- table_Disturbance |>
  mutate(Entity = `Utility/Power Pool`)

energy_sector_rev_sum <- energy_sector |> 
  select(Entity, Ownership, `Revenues (Thousands Dollars)`) |>
  group_by(Entity, Ownership) |>
  summarise(sum_revenue = sum(`Revenues (Thousands Dollars)`)) |>
  as.data.frame()
```

``` r
table_CAIDI <- table_CAIDI |>
 pivot_longer(cols = `2013`:`2022`,
              names_to = "Year",
              values_to = "CAIDI")
```

``` r
our_theme <- theme_grey() + 
  theme(text = element_text(color = "#1d91c0"), 
        legend.text = element_text(color = "black"))
```

## 1. Ownership vs price of electricity /according to sector

- Q: Does ownership e.g. cooperatives influence the average price of
  electricity, does it benefit individuals/residential sector or is it
  more geared towards commercial sector…?\`

``` r
energy_sector |>
  filter(!is.na(Ownership)) |>
  mutate(
    Ownership = fct_relevel(
      Ownership,
      "Cooperative", "Municipal", "Political Subdivision", "State", "Federal", "Behind the Meter", "Retail Power Marketer", "Investor Owned"
    )) |>

 ggplot(aes(x = as.numeric(Ownership), y = `Average Price (cents/kWh)`)) +
  geom_boxplot(outlier.shape = NA, aes(color = Ownership)) +
  geom_smooth(method = "loess", color = "black") +
  ylim(0,25)+
  facet_wrap(~Sector) +
  scale_color_manual(values = c("Cooperative" = "#ffffd9", "Municipal" = "#edf8b1", "Political Subdivision" = "#c7e9b4", "State" = "#7fcdbb", "Federal" = "#41b6c4", "Behind the Meter" = "#1d91c0", "Retail Power Marketer" = "#225ea8", "Investor Owned" = "#0c2c84")) +
  labs(
    title = "Average Price (cents/kWh) according to Type of Ownership",
    subtitle = "Per Sector based on 2022 data",
    caption = "Source : https://www.eia.gov/electricity/data.php#sales",
    x = "") +
  our_theme +
  theme(axis.text.x = element_blank(), 
        axis.ticks.x = element_blank(),
        panel.grid = element_blank())
```

<img src="workspace_files/figure-gfm/ownership-average-price-state-1.png" alt="Boxplot with trendline of average price in cents per kilowatthour in 2022 in each sector according to type of ownership. Types of ownership are ordered on a spectrum from mor cooperative to more investor-owned. Federally-owned companies and companies owned by political subdivisions seem to offer lower than average prices in all sectors. The industrial sector generally has lower average prices than the other sectors."  />

``` r
#ggsave(filename = "Ownership vs price.png", device = "png")
```

# 2a. Revenue and actual blackouts and ownership

- Q: does less profit and community owned mean less blackouts? Or does
  ownership tell us that big companies have more means to prevent
  blackouts?

``` r
disturbance_count <- table_Disturbance |>
  group_by(Entity) |>
  count()

disturbance_complete <- disturbance_count |> 
  inner_join(energy_sector_rev_sum)
```

For the next plot we want to count the disturbances per utility company.

``` r
ggplot(disturbance_complete, aes(
  y = sum_revenue/1000000, 
  x = `Ownership`,
  size = n)) +
  geom_point(color = "#1d91c0", alpha = 0.8) +
  labs(title = "Number of Disturbances per Utility Company", 
       subtitle = "in relation to Ownership Type and Revenue", 
       x = "Type of Ownership", y = "Revenue in Million Dollars ($)", 
       size = "Number of\nDisturbances",
       caption = "Source : https://www.eia.gov/electricity/data.php#sales") +
  our_theme
```

<img src="workspace_files/figure-gfm/disturbances-vs-revenue-and-ownership-1.png" alt="Scatterplot of number of Disturbances per utility company in relation to the type of ownership of the utility company and the revenue made by the company in 2022 in Millions of dollars. Size of the points reflects number of disturbances experienced in 2022. Investor-owned companies experience by far the most disturbances."  />

# 2b. CAIDI reliability in electricity networks throughout the US based on states

Q: How does CAIDI vary throughout the US states? Animate leaflet from
2013-2022.

``` r
# Create a base plot
CAIDI_leaflet_df$Year <-as.numeric(CAIDI_leaflet_df$Year)

p <- CAIDI_leaflet_df |>
  ggplot(aes(fill = CAIDI,
             group = Year)) +
  geom_sf() +
  xlim(125, 68) +
  ylim(24.5, 50) +
  scale_fill_viridis_c() +
  our_theme +
  theme(axis.text.x = element_blank(), 
        axis.ticks.x = element_blank(),
        axis.text.y = element_blank(), 
        axis.ticks.y = element_blank(),
        panel.grid = element_blank(),
        panel.background = element_rect(fill='white')) +
  labs(title = "Customer Average Interruption Duration Index (CAIDI) map of lower 48 US states", 
       subtitle = "Year: {closest_state}",
       caption = "Source: https://www.eia.gov/electricity/data/annual/") 

# Create an animated plot
p <- p + transition_states(
  states = Year,
  transition_length = 3,
  state_length = 3)

#animate(p, duration = 30, renderer = gifski_renderer("map.gif"))
```

## 3. Amount of electricity used, cheaper prices?

- Q: Is it cheaper to buy electricity depending on the sector? Is buying
  electricity in bulk cheaper?

- compare this to experience in history: companies that consume lots of
  electricity usually get cheaper electricity prices because utilities
  had reliable demand from the industries

- electricity producers want reliable consumers –\> does this bias still
  exist? Book: “The Grid”

``` r
#creating the mean of average price in each sector
line_test <- energy_sector |>
  summarise(mean(`Average Price (cents/kWh)`, na.rm = TRUE),.by = Sector)

line_test$mean <- line_test$`mean(\`Average Price (cents/kWh)\`, na.rm = TRUE)` 

line_test$label <-
  round(line_test$mean, 2)

#create graph using mean of average price in each sector
energy_sector |>
  ggplot(aes(x = (
    `Sales (Megawatthours)`*1000/`Customers (Count)`), y = `Average Price (cents/kWh)`, color = Sector)) +
  geom_point() +
  geom_smooth() +
  geom_hline(data = line_test, aes(yintercept = mean)) +
  geom_text(data = line_test, aes(50000000, y = label, label = label, vjust = -1), color = "black") +
  facet_wrap(~Sector) +
  scale_color_viridis_d() +
  xlim(0, 400000000) +
  labs(
    title = "Average KWh Consumption per Customer vs Average Price",
    subtitle = "by Sector in 2022",
    x = "Average kWh Consumption per Customer",
    y = "Average Price (cents/kWh)",
    caption = "Source : https://www.eia.gov/electricity/data.php#sales"
  ) +
  our_theme +
  theme(panel.grid = element_blank())
```

<img src="workspace_files/figure-gfm/average-price-in-kWH-consumption-1.png" alt="Scatterplot showing average kilowatthour consumption per customer versus average price in cents per kilowatthour, faceted by sector. Each facet includes the mean average price offered by the companies to each sector. The industrial and transportation sector have the widest spread of electricity use while the Residential and Commercial sector use less electricity per customer. The industrial sector pays on average the least for their electricity with 9.25 cents per kilowatthour in comparison to 13.55 cents per kilowatthour in the residential sector where there are many customers which consume smaller amounts of electricity each."  />

## \[4. Number of customers vs price by sector – if we are bored, we can do this!

- Q: Do bigger utility companies (meaning more customers) offer lower
  avg. prices due to scale?

``` r
energy_sector |>
  filter(Sector == "Residential") |>
  ggplot(aes(x = `Customers (Count)`, y = `Average Price (cents/kWh)`)) +
  geom_point() +
  geom_smooth() +
  xlim(0, 250000) +
  labs(title = "Average Price vs. Number of Customers", 
       subtitle = "per Utility company for the Residential Sector in 2022", 
       x = "Number of Customers", 
       y = "Average Price (cents/kWh)",
       caption = "Source : https://www.eia.gov/electricity/data.php#sales") +
  our_theme
```

<img src="workspace_files/figure-gfm/price-by-customers-1.png" alt="Scatterplot and trendline of Number of Customers in the Residential Sector per Utility Company versus average price in cents per kilowatthour. There is no correlation between the number of customers and the average price. The price that the utility companies offer depends on other factors. Most utility companies serve electricity to less than 50000 households, while some serve 250000 and more households."  />

## 5. Per household consumption of electricity (megawatthours sold/customers) vs price

- Q: Do consumers use less electricity if it is more expensive?

``` r
ggplot(table_Residential, aes(x = (`Sales (Megawatthours)`*1000)/`Customers (Count)`, y = `Average Price (cents/kWh)`)) +
  geom_hex() +
  geom_smooth() +
  labs(title = "Electricity Consumption per Customer vs. Average Price", 
       subtitle = "in the Residential Sector in 2022", 
       x = "Average Electricity Consumption per Customer (in kWh)", 
       y = "Average Price (cents/kWh)", 
       fill = "Number of\nUtility\nCompanies",
       caption = "Source : https://www.eia.gov/electricity/data.php#sales") +
  our_theme
```

<img src="workspace_files/figure-gfm/household-consumption-electricity-1.png" alt="Heatmap and trendline of average electricity consumption per customer in the Residential Sector in kilowatthours of each utility company versus average price in cents per kilowatthours offered by the respective utility company. Heatmap shows that most customers use around 14000 kilowatthours of electrcity annually paying between 10 to 14 cents per kilowatthour. There is a slight negative correlation. Household consumption is lower when the price is high."  />
