Project proposal
================
Energyyy

``` r
library(tidyverse)
library(broom)
#install.packages("lubridate")
library(lubridate)
library(tidyr)
```

## 1. Introduction

The United States of America has on of the most diverse electric grids
in the world. With this many stakeholders it is interesting to know
which factors affect electricity distribution and pricing to what extent
in the US. We are looking into the data from the the U.S. Energy
Information Administration (EIA). EIA collects, analyzes, and
disseminates independent and impartial energy information. The utility
companies are required to self-report this data due to regulations. The
variables that we will be looking at are Ownership type, Customers
(Count), Sales (Megawatt hours) Revenues (Thousands Dollars), and
Average Price (cents/kWh).

Research Question: Which factors affect electricity distribution and
pricing to what extent in the US? - black outs - urban rural - sectors
-seasons - regional differences - utility companies ownership

## 2. Data

``` r
library(readxl)
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
```

``` r
glimpse(table_Residential)
```

    ## Rows: 1,526
    ## Columns: 7
    ## $ Entity                         <chr> "Alaska Electric Light & Power Co.", "A…
    ## $ State                          <chr> "AK", "AK", "AK", "AK", "AK", "AK", "AK…
    ## $ Ownership                      <chr> "Investor Owned", "Investor Owned", "Co…
    ## $ `Customers (Count)`            <dbl> 15159, 5627, 7770, 96750, 40277, 28994,…
    ## $ `Sales (Megawatthours)`        <dbl> 162982, 26750, 44102, 591397, 282672, 1…
    ## $ `Revenues (Thousands Dollars)` <dbl> 20081.7, 9950.0, 23512.0, 117980.8, 809…
    ## $ `Average Price (cents/kWh)`    <dbl> 12.32142, 37.19626, 53.31278, 19.94951,…

``` r
glimpse(table_Commercial)
```

    ## Rows: 1,535
    ## Columns: 7
    ## $ Entity                         <chr> "Alaska Electric Light & Power Co.", "A…
    ## $ State                          <chr> "AK", "AK", "AK", "AK", "AK", "AK", "AK…
    ## $ Ownership                      <chr> "Investor Owned", "Investor Owned", "Co…
    ## $ `Customers (Count)`            <dbl> 2423, 2662, 3742, 16339, 6835, 4408, 13…
    ## $ `Sales (Megawatthours)`        <dbl> 125095, 46941, 80587, 1247883, 119020, …
    ## $ `Revenues (Thousands Dollars)` <dbl> 13045.9, 13800.0, 36659.8, 195913.5, 32…
    ## $ `Average Price (cents/kWh)`    <dbl> 10.428794, 29.398607, 45.490960, 15.699…

``` r
glimpse(table_Industrial)
```

    ## Rows: 1,245
    ## Columns: 7
    ## $ Entity                         <chr> "Alaska Electric Light & Power Co.", "C…
    ## $ State                          <chr> "AK", "AK", "AK", "AK", "AK", "AK", "AL…
    ## $ Ownership                      <chr> "Investor Owned", "Cooperative", "Coope…
    ## $ `Customers (Count)`            <dbl> 119, 7, 557, 23, 14, 116, 6125, 15, 2, …
    ## $ `Sales (Megawatthours)`        <dbl> 116995, 63345, 842722, 119983, 22801, 8…
    ## $ `Revenues (Thousands Dollars)` <dbl> 13369.2, 8204.0, 169391.8, 14685.5, 226…
    ## $ `Average Price (cents/kWh)`    <dbl> 11.427155, 12.951298, 20.100555, 12.239…

``` r
glimpse(table_Transportation)
```

    ## Rows: 43
    ## Columns: 7
    ## $ Entity                         <chr> "City of North Little Rock - (AR)", "En…
    ## $ State                          <chr> "AR", "AR", "AZ", "AZ", "CA", "CA", "CA…
    ## $ Ownership                      <chr> "Municipal", "Investor Owned", "Politic…
    ## $ `Customers (Count)`            <dbl> 1, 1, 1, 1, 1, 1, 1, 2, 1, 4, 1, 1, 1, …
    ## $ `Sales (Megawatthours)`        <dbl> 213, 6, 9375, 829, 93808, 6968, 909, 10…
    ## $ `Revenues (Thousands Dollars)` <dbl> 33.0, 0.9, 866.0, 115.5, 12338.1, 1322.…
    ## $ `Average Price (cents/kWh)`    <dbl> 15.492958, 15.000000, 9.237333, 13.9324…

``` r
glimpse(table_Disturbance)
```

    ## Rows: 167
    ## Columns: 11
    ## $ Year                           <chr> "2022", "2022", "2022", "2022", "2022",…
    ## $ Month                          <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    ## $ `Event Date and Time`          <chr> "01/02/2022  1:21 PM", "01/03/2022  1:0…
    ## $ `Restoration Date and Time`    <chr> "01/02/2022  5:38 PM", "01/03/2022  2:0…
    ## $ Duration                       <chr> "4 Hours, 17 Minutes", "13 Hours,  0 Mi…
    ## $ `Utility/Power Pool`           <chr> "Pacific Gas & Electric Co", "Southern …
    ## $ `NERC Region`                  <chr> "WECC", "SERC", "SERC", "SERC", "NPCC",…
    ## $ `Area Affected`                <chr> "California: Tuolumne County;", "Georgi…
    ## $ `Type of Disturbance`          <chr> "Electrical System Separation (Islandin…
    ## $ `Loss (megawatts)`             <chr> "3", "283", "Unknown", "Unknown", "0", …
    ## $ `Number of Customers Affected` <chr> "1706", "40885", "60424", "142000", "0"…

This mutate function adds a column which contains the sector of the
data.

``` r
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
glimpse(energy_sector)
```

    ## Rows: 4,349
    ## Columns: 8
    ## $ Entity                         <chr> "Alaska Electric Light & Power Co.", "A…
    ## $ State                          <chr> "AK", "AK", "AK", "AK", "AK", "AK", "AK…
    ## $ Ownership                      <chr> "Investor Owned", "Investor Owned", "Co…
    ## $ `Customers (Count)`            <dbl> 15159, 5627, 7770, 96750, 40277, 28994,…
    ## $ `Sales (Megawatthours)`        <dbl> 162982, 26750, 44102, 591397, 282672, 1…
    ## $ `Revenues (Thousands Dollars)` <dbl> 20081.7, 9950.0, 23512.0, 117980.8, 809…
    ## $ `Average Price (cents/kWh)`    <dbl> 12.32142, 37.19626, 53.31278, 19.94951,…
    ## $ Sector                         <chr> "Residential", "Residential", "Resident…

``` r
disturbance_unjoined <- table_Disturbance |>
  separate(col = Duration, into = c("Hours (Duration)", "Minutes (Duration)"), sep = ',') 

disturbance <- 
  rename(disturbance_unjoined, Entity = `Utility/Power Pool`) |>
  right_join(energy_sector, by = "Entity")
```

    ## Warning in right_join(rename(disturbance_unjoined, Entity = `Utility/Power Pool`), : Detected an unexpected many-to-many relationship between `x` and `y`.
    ## ℹ Row 19 of `x` matches multiple rows in `y`.
    ## ℹ Row 333 of `y` matches multiple rows in `x`.
    ## ℹ If a many-to-many relationship is expected, set `relationship =
    ##   "many-to-many"` to silence this warning.

``` r
glimpse(disturbance)
```

    ## Rows: 4,397
    ## Columns: 19
    ## $ Year                           <chr> "2022", "2022", "2022", "2022", "2022",…
    ## $ Month                          <dbl> 2, 2, 2, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, …
    ## $ `Event Date and Time`          <chr> "02/04/2022  1:10 PM", "02/04/2022  1:1…
    ## $ `Restoration Date and Time`    <chr> NA, NA, NA, "04/13/2022  8:00 AM", "04/…
    ## $ `Hours (Duration)`             <chr> NA, NA, NA, "46 Hours", "46 Hours", "46…
    ## $ `Minutes (Duration)`           <chr> NA, NA, NA, " 45 Minutes", " 45 Minutes…
    ## $ Entity                         <chr> "Central Hudson Gas & Elec Corp", "Cent…
    ## $ `NERC Region`                  <chr> "NPCC", "NPCC", "NPCC", "WECC", "WECC",…
    ## $ `Area Affected`                <chr> "New York: Ulster County;", "New York: …
    ## $ `Type of Disturbance`          <chr> "Loss of electric service to more than …
    ## $ `Loss (megawatts)`             <chr> "Unknown", "Unknown", "Unknown", "3140"…
    ## $ `Number of Customers Affected` <chr> "67404", "67404", "67404", "73717", "73…
    ## $ State                          <chr> "NY", "NY", "NY", "OR", "OR", "OR", "OR…
    ## $ Ownership                      <chr> "Investor Owned", "Investor Owned", "In…
    ## $ `Customers (Count)`            <dbl> 233720, 37720, 749, 809603, 108016, 430…
    ## $ `Sales (Megawatthours)`        <dbl> 2122918, 874245, 65913, 8088473, 652205…
    ## $ `Revenues (Thousands Dollars)` <dbl> 472571.0, 171489.0, 10853.0, 1103143.0,…
    ## $ `Average Price (cents/kWh)`    <dbl> 22.260445, 19.615668, 16.465644, 13.638…
    ## $ Sector                         <chr> "Residential", "Commercial", "Industria…

## 3. Ethics review

### Limitations in data sources:

- table_CAIDI: Most utility companies use IEE standards to report CAIDI,
  but some do not. We have decided to leave out companies of our
  analysis that do not use IEE standards, so we will not have a complete
  account of ALL utility companies, but of most of them. We decided not
  to use the CAIDI index that includes Major Event Days because those
  represent hurricanes, storms etc. which utility companies do not have
  control over. Thus, they do not attest to the utility’s ability to
  provide reliable services.

- The data collection is standardized and done by governmental
  institutions, so the data should be reliable, of high quality and
  equal across utility companies and sectors. However, the EIA relies on
  the honesty of the utility company to share accurate data with the
  EIA.

- Ilham and Anna do not have an extensive background in the energy
  industry, so we rely on Rudy for most of the expertise in our project.

### Positive effects on people:

- Education of lay people: might help citizens to be better informed of
  the US energy industry which is highly complex.
- Decision-makers: gain a better understanding to improve policy-making
  and can therefore evaluate the efficiency and effectiveness of utility
  companies. This enables them to encourage the companies to become more
  efficient and effective.
- Education of Rudy: improves his personal understanding of the US
  energy sector
- We could enhance these positive effects by dispersing our research
  more and communicating it to citizens and decision-makers.

### Negative effects on people:

- The reputation of specific utility companies might suffer from our
  research if they are connected to higher prices for lower-quality
  offering of energy services.

### Minimising negative impact:

- We will talk about the limitations/disclaimers of our research, so
  that people are aware of what we cannot show with our data, so that
  people do not draw wrong conclusions from our research.
  - This is a beneficial action to take because people will be more
    aware of the limitations and can understand our data and our
    visualizations better.
- We are presenting to our class at COA only and we do not intend to
  share our outcomes with the wider community where it would lead to the
  implementation of policies etc.

## 4. Data analysis plan

We will use all the mentioned variables in our analyses. There will be
other data that we include in our analysis to visualize the data we
already have, such as coordinates to locate the US states on a potential
heatmap. In case we would like to expand our data analysis, we would go
further into price developments per sector and US state potentially
using data between 1960-2023.

Priority list of data analyses:

1.  Ownership vs price of electricity /according to sector

- Q: Does ownership e.g. cooperatives influence the average price of
  electricity, does it benefit individuals/residential sector or is it
  more geared towards commercial sector…?\`

``` r
p1 <- energy_sector |>
  filter(!is.na(Ownership)) |>

 ggplot(aes(x = Ownership, y = `Average Price (cents/kWh)`)) +
  #geom_point(aes(color = Sector)) +
  geom_boxplot(outlier.shape = NA, aes(color = Sector)) +
  ylim(0,25)+
  theme(axis.text.x = element_text(angle = 45, vjust = 1, hjust  = 1), plot.margin = margin(1, 0.1, 1, 1, unit = "mm")) 
#ggsave(filename = "plot1.png", device = "png")
```

2.  Revenue vs price and actual blackouts and ownership

- Q: does less profit and community owned mean less blackouts? Or does
  ownership tell us that big companies have more means to prevent
  blackouts?

``` r
disturbance |> 
  group_by(Entity) |>
  count() |>
  arrange(n)
```

    ## # A tibble: 1,308 × 2
    ## # Groups:   Entity [1,308]
    ##    Entity                        n
    ##    <chr>                     <int>
    ##  1 AGC Division of APGI Inc      1
    ##  2 American PowerNet             1
    ##  3 Ammper Power LLC              1
    ##  4 Arrow Energy TX LLC           1
    ##  5 BKV-BPP Retail, LLC           1
    ##  6 BP Energy Company             1
    ##  7 BP Energy Retail LLC          1
    ##  8 Basin Electric Power Coop     1
    ##  9 Branch Energy Texas LLC       1
    ## 10 Calpine Power America LLC     1
    ## # ℹ 1,298 more rows

For the next plot we want to count the disturbances per utility company.
However, we need to clean up the data first because some companies come
up with different names. Hence, the mutate function.

``` r
disturbance |>
  mutate(Company = 
           case_when(
    `Entity` %in% 
      c("American Electric Power (Regulated Generation)", 
      "American Electric Power - (RFC Reliability Region) (8400 Smiths Mill Road, New Albany Ohio 43054)")
        ~ "American Electric Power", 
     `Entity` %in% 
      c("Baltimore Gas & Electric Co",  
"Baltimore Gas and Electric") ~ "Baltimore Gas & Electric",
     TRUE ~ as.character(as.character(`Entity`))
    )) |>
    group_by(Company) |>
  count()
```

    ## # A tibble: 1,308 × 2
    ## # Groups:   Company [1,308]
    ##    Company                            n
    ##    <chr>                          <int>
    ##  1 3000 Energy Corp                   2
    ##  2 4-County Electric Power Assn       3
    ##  3 A & N Electric Coop                5
    ##  4 AGC Division of APGI Inc           1
    ##  5 ALLETE, Inc.                       3
    ##  6 AP Holdings LLC                    2
    ##  7 Accent Energy Holdings, LLC        2
    ##  8 Access Energy Coop                 3
    ##  9 Adams Electric Cooperative Inc     3
    ## 10 Adams-Columbia Electric Coop       3
    ## # ℹ 1,298 more rows

``` r
#ggplot(aes(x = `Average Price (cents/kWh)`, y = `Revenues (Thousands Dollars)`, size = n of blackouts/in categories)), facet(~ ownership)
```

3.  Amount of electricity used, cheaper prices?

- Q: Is it cheaper to buy electricity depending on the sector and is
  buying electricity in bulk cheaper?
- ggplot(aes(x = Sales(mWh)/n of customers, y = average price, color =
  sectors))

4.  Number of customers vs price by sector

- Q: Do bigger utility companies (meaning more customers) offer lower
  avg. prices due to scale?
- ggplot(aes(x = avg price, y = customers)), facet( ~ sector)

5.  Per household consumption of electricity (megawatthours
    sold/customers) vs price

- Q: Do consumers use less electricity if it is more expensive?
- ggplot(aes(x = Sales/customers, y = avg. price))

Stretch goals: 6. Heatmap of the US’ electricity use - Note: we might
need long lat data to map the US states - Average per capita Sales per
state (sum up all sales, group by state, calculate average), each box of
the heatmap is one state

7.  Seasonal price variation vs average temperature, per month

- We would need: avg temperature per month per state, electricity price
  per month for multiple years per state
