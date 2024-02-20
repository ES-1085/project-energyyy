Project Workspace
================
Energyyy

``` r
library(tidyverse)
library(broom)
#install.packages("lubridate")
library(lubridate)
library(tidyr)
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

1.  Ownership vs price of electricity /according to sector

- Q: Does ownership e.g. cooperatives influence the average price of
  electricity, does it benefit individuals/residential sector or is it
  more geared towards commercial sector…?\`

to-do: - facet by sector (for readability) - aes(color = ownership) to
be able to compare the same ownership better in the different facets -
justify looking at this question, why it makes sense even though prices
are mainly affected by fuel prices (we are looking at one year - 2022 -
and different utility companies, we assume that they pay about the same
for fuel) - arrange x-axis in spectrum from cooperative to big
corporation /catered to people or to profit (fct_relevel) Order:
Cooperative, municipal, political subdivision, state, federal, Behind
the Meter (clarify what it is), retail power marketer, investor owned –
need to justify our levelling into this spectrum – - geom_smooth - does
that work on a boxplot? see if there is a trend on this
cooperative-corporate spectrum for different sectors - add labs()

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
  geom_smooth(method = "glm", color = "black") +
  ylim(0,25)+
  facet_wrap(~Sector) +
  scale_color_manual(values = c("Cooperative" = "#ffffd9", "Municipal" = "#edf8b1", "Political Subdivision" = "#c7e9b4", "State" = "#7fcdbb", "Federal" = "#41b6c4", "Behind the Meter" = "#1d91c0", "Retail Power Marketer" = "#225ea8", "Investor Owned" = "#0c2c84")) +
  theme(axis.text.x = element_text(
    angle = 45, vjust = 1, hjust  = 1), plot.margin = margin(1, 0.1, 1, 1, unit = "mm")) +
  labs(
    title = "Does ownership type of the utility company influence the average price of electricity?",
    subtitle = "Per sector based on 2022 data",
    caption = "Source : https://www.eia.gov/electricity/data.php#sales",
    x = ""
  )
```

    ## Warning: Removed 73 rows containing non-finite values (`stat_boxplot()`).

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 73 rows containing non-finite values (`stat_smooth()`).

![](workspace_files/figure-gfm/ownership-average-price-state-1.png)<!-- -->

``` r
#ggsave(filename = "plot1.png", device = "png")
```

2a. Revenue and actual blackouts and ownership - Q: does less profit and
community owned mean less blackouts? Or does ownership tell us that big
companies have more means to prevent blackouts?

- aes(x = Revenue, y = n of blackouts, color = ownership) +

- geom_point()

- finish cleaning up company names in Excel

- join tables by utility company name

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

2b. CAIDI reliability in electricity networks throughout the US based on
states Q: How does CAIDI vary throughout the US states? Animate leaflet
from 2013-2022.

- use sequential color scheme for Index
- import/upload data and tidy it

3.  Amount of electricity used, cheaper prices?

- Q: Is it cheaper to buy electricity depending on the sector? Is buying
  electricity in bulk cheaper?

- ggplot(aes(x = Sales(mWh)/n of customers, y = average price, color =
  sectors))

- compare this to experience in history: companies that consume lots of
  electricity usually get cheaper electricity prices because utilities
  had reliable demand from the industries

- electricity producers want reliable consumers –\> does this bias still
  exist? Book: “The Grid”

\[4. Number of customers vs price by sector – if we are bored, we can do
this! - Q: Do bigger utility companies (meaning more customers) offer
lower avg. prices due to scale? - ggplot(aes(x = avg price, y =
customers)), facet( ~ sector)\]

5.  Per household consumption of electricity (megawatthours
    sold/customers) vs price

- Q: Do consumers use less electricity if it is more expensive?
- ggplot(aes(x = Sales/customers, y = avg. price))

Stretch goals: 6. Heatmap of the US’ electricity use - Note: we might
need long lat data to map the US states - Average per capita Sales per
state (sum up all sales, group by state, calculate average), each box of
the heatmap is one state - goes together with 5.
