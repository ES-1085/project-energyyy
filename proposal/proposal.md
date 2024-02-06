Project proposal
================
Energyyy

``` r
library(tidyverse)
library(broom)
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

RQ: Which factors affect electricity distribution and pricing to what
extent in the US? - black outs - urban rural - sectors -seasons -
regional differences - utility companies ownership

## 2. Data

``` r
library(readxl)
```

``` r
library(readxl)
table_Residential <- read_excel("/cloud/project/data/table_Residential.xlsx")
```

    ## Warning: Expecting numeric in G1487 / R1487C7: got '.'

    ## Warning: Expecting numeric in G1488 / R1488C7: got '.'

    ## Warning: Expecting numeric in G1489 / R1489C7: got '.'

    ## Warning: Expecting numeric in G1490 / R1490C7: got '.'

    ## Warning: Expecting numeric in G1491 / R1491C7: got '.'

    ## Warning: Expecting numeric in G1492 / R1492C7: got '.'

    ## Warning: Expecting numeric in G1493 / R1493C7: got '.'

    ## Warning: Expecting numeric in G1494 / R1494C7: got '.'

    ## Warning: Expecting numeric in G1495 / R1495C7: got '.'

    ## Warning: Expecting numeric in G1496 / R1496C7: got '.'

    ## Warning: Expecting numeric in G1497 / R1497C7: got '.'

    ## Warning: Expecting numeric in G1498 / R1498C7: got '.'

    ## Warning: Expecting numeric in G1499 / R1499C7: got '.'

    ## Warning: Expecting numeric in G1500 / R1500C7: got '.'

    ## Warning: Expecting numeric in G1501 / R1501C7: got '.'

    ## Warning: Expecting numeric in G1502 / R1502C7: got '.'

    ## Warning: Expecting numeric in G1503 / R1503C7: got '.'

    ## Warning: Expecting numeric in G1504 / R1504C7: got '.'

    ## Warning: Expecting numeric in G1505 / R1505C7: got '.'

    ## Warning: Expecting numeric in G1506 / R1506C7: got '.'

    ## Warning: Expecting numeric in G1507 / R1507C7: got '.'

    ## Warning: Expecting numeric in G1508 / R1508C7: got '.'

    ## Warning: Expecting numeric in G1509 / R1509C7: got '.'

    ## Warning: Expecting numeric in G1510 / R1510C7: got '.'

    ## Warning: Expecting numeric in G1511 / R1511C7: got '.'

    ## Warning: Expecting numeric in G1512 / R1512C7: got '.'

    ## Warning: Expecting numeric in G1513 / R1513C7: got '.'

    ## Warning: Expecting numeric in G1514 / R1514C7: got '.'

    ## Warning: Expecting numeric in G1515 / R1515C7: got '.'

    ## Warning: Expecting numeric in G1516 / R1516C7: got '.'

    ## Warning: Expecting numeric in G1517 / R1517C7: got '.'

    ## Warning: Expecting numeric in G1518 / R1518C7: got '.'

    ## Warning: Expecting numeric in G1519 / R1519C7: got '.'

    ## Warning: Expecting numeric in G1520 / R1520C7: got '.'

    ## Warning: Expecting numeric in G1521 / R1521C7: got '.'

    ## Warning: Expecting numeric in G1522 / R1522C7: got '.'

    ## Warning: Expecting numeric in G1523 / R1523C7: got '.'

    ## Warning: Expecting numeric in G1524 / R1524C7: got '.'

    ## Warning: Expecting numeric in G1525 / R1525C7: got '.'

    ## Warning: Expecting numeric in G1526 / R1526C7: got '.'

    ## Warning: Expecting numeric in G1527 / R1527C7: got '.'

``` r
table_Commercial <- read_excel("/cloud/project/data/table_Commercial.xlsx")
```

    ## Warning: Expecting numeric in G1265 / R1265C7: got '.'

    ## Warning: Expecting numeric in G1490 / R1490C7: got '.'

    ## Warning: Expecting numeric in G1491 / R1491C7: got '.'

    ## Warning: Expecting numeric in G1492 / R1492C7: got '.'

    ## Warning: Expecting numeric in G1493 / R1493C7: got '.'

    ## Warning: Expecting numeric in G1494 / R1494C7: got '.'

    ## Warning: Expecting numeric in G1495 / R1495C7: got '.'

    ## Warning: Expecting numeric in G1496 / R1496C7: got '.'

    ## Warning: Expecting numeric in G1497 / R1497C7: got '.'

    ## Warning: Expecting numeric in G1498 / R1498C7: got '.'

    ## Warning: Expecting numeric in G1499 / R1499C7: got '.'

    ## Warning: Expecting numeric in G1500 / R1500C7: got '.'

    ## Warning: Expecting numeric in G1501 / R1501C7: got '.'

    ## Warning: Expecting numeric in G1502 / R1502C7: got '.'

    ## Warning: Expecting numeric in G1503 / R1503C7: got '.'

    ## Warning: Expecting numeric in G1504 / R1504C7: got '.'

    ## Warning: Expecting numeric in G1505 / R1505C7: got '.'

    ## Warning: Expecting numeric in G1506 / R1506C7: got '.'

    ## Warning: Expecting numeric in G1507 / R1507C7: got '.'

    ## Warning: Expecting numeric in G1508 / R1508C7: got '.'

    ## Warning: Expecting numeric in G1509 / R1509C7: got '.'

    ## Warning: Expecting numeric in G1510 / R1510C7: got '.'

    ## Warning: Expecting numeric in G1511 / R1511C7: got '.'

    ## Warning: Expecting numeric in G1512 / R1512C7: got '.'

    ## Warning: Expecting numeric in G1513 / R1513C7: got '.'

    ## Warning: Expecting numeric in G1514 / R1514C7: got '.'

    ## Warning: Expecting numeric in G1515 / R1515C7: got '.'

    ## Warning: Expecting numeric in G1516 / R1516C7: got '.'

    ## Warning: Expecting numeric in G1517 / R1517C7: got '.'

    ## Warning: Expecting numeric in G1518 / R1518C7: got '.'

    ## Warning: Expecting numeric in G1519 / R1519C7: got '.'

    ## Warning: Expecting numeric in G1520 / R1520C7: got '.'

    ## Warning: Expecting numeric in G1521 / R1521C7: got '.'

    ## Warning: Expecting numeric in G1522 / R1522C7: got '.'

    ## Warning: Expecting numeric in G1523 / R1523C7: got '.'

    ## Warning: Expecting numeric in G1524 / R1524C7: got '.'

    ## Warning: Expecting numeric in G1525 / R1525C7: got '.'

    ## Warning: Expecting numeric in G1526 / R1526C7: got '.'

    ## Warning: Expecting numeric in G1527 / R1527C7: got '.'

    ## Warning: Expecting numeric in G1528 / R1528C7: got '.'

    ## Warning: Expecting numeric in G1529 / R1529C7: got '.'

    ## Warning: Expecting numeric in G1530 / R1530C7: got '.'

    ## Warning: Expecting numeric in G1531 / R1531C7: got '.'

    ## Warning: Expecting numeric in G1532 / R1532C7: got '.'

    ## Warning: Expecting numeric in G1533 / R1533C7: got '.'

    ## Warning: Expecting numeric in G1534 / R1534C7: got '.'

    ## Warning: Expecting numeric in G1535 / R1535C7: got '.'

    ## Warning: Expecting numeric in G1536 / R1536C7: got '.'

``` r
table_Industrial <- read_excel("/cloud/project/data/table_Industrial.xlsx")
table_Transportation <- read_excel("/cloud/project/data/table_Transportation.xlsx") %>%
  mutate(`Average Price (cents/kWh)` == case_when(`Average Price (cents/kWh)` == "." ~ "NA"))
```

``` r
table_Residential <- read_excel("/cloud/project/data/table_Residential.xlsx") %>%
  mutate(`Average Price (cents/kWh)` = case_when(
    `Average Price (cents/kWh)` == "." ~ NA,
    TRUE                   ~ as.double(`Average Price (cents/kWh)`)
    ))
```

    ## Warning: Expecting numeric in G1487 / R1487C7: got '.'

    ## Warning: Expecting numeric in G1488 / R1488C7: got '.'

    ## Warning: Expecting numeric in G1489 / R1489C7: got '.'

    ## Warning: Expecting numeric in G1490 / R1490C7: got '.'

    ## Warning: Expecting numeric in G1491 / R1491C7: got '.'

    ## Warning: Expecting numeric in G1492 / R1492C7: got '.'

    ## Warning: Expecting numeric in G1493 / R1493C7: got '.'

    ## Warning: Expecting numeric in G1494 / R1494C7: got '.'

    ## Warning: Expecting numeric in G1495 / R1495C7: got '.'

    ## Warning: Expecting numeric in G1496 / R1496C7: got '.'

    ## Warning: Expecting numeric in G1497 / R1497C7: got '.'

    ## Warning: Expecting numeric in G1498 / R1498C7: got '.'

    ## Warning: Expecting numeric in G1499 / R1499C7: got '.'

    ## Warning: Expecting numeric in G1500 / R1500C7: got '.'

    ## Warning: Expecting numeric in G1501 / R1501C7: got '.'

    ## Warning: Expecting numeric in G1502 / R1502C7: got '.'

    ## Warning: Expecting numeric in G1503 / R1503C7: got '.'

    ## Warning: Expecting numeric in G1504 / R1504C7: got '.'

    ## Warning: Expecting numeric in G1505 / R1505C7: got '.'

    ## Warning: Expecting numeric in G1506 / R1506C7: got '.'

    ## Warning: Expecting numeric in G1507 / R1507C7: got '.'

    ## Warning: Expecting numeric in G1508 / R1508C7: got '.'

    ## Warning: Expecting numeric in G1509 / R1509C7: got '.'

    ## Warning: Expecting numeric in G1510 / R1510C7: got '.'

    ## Warning: Expecting numeric in G1511 / R1511C7: got '.'

    ## Warning: Expecting numeric in G1512 / R1512C7: got '.'

    ## Warning: Expecting numeric in G1513 / R1513C7: got '.'

    ## Warning: Expecting numeric in G1514 / R1514C7: got '.'

    ## Warning: Expecting numeric in G1515 / R1515C7: got '.'

    ## Warning: Expecting numeric in G1516 / R1516C7: got '.'

    ## Warning: Expecting numeric in G1517 / R1517C7: got '.'

    ## Warning: Expecting numeric in G1518 / R1518C7: got '.'

    ## Warning: Expecting numeric in G1519 / R1519C7: got '.'

    ## Warning: Expecting numeric in G1520 / R1520C7: got '.'

    ## Warning: Expecting numeric in G1521 / R1521C7: got '.'

    ## Warning: Expecting numeric in G1522 / R1522C7: got '.'

    ## Warning: Expecting numeric in G1523 / R1523C7: got '.'

    ## Warning: Expecting numeric in G1524 / R1524C7: got '.'

    ## Warning: Expecting numeric in G1525 / R1525C7: got '.'

    ## Warning: Expecting numeric in G1526 / R1526C7: got '.'

    ## Warning: Expecting numeric in G1527 / R1527C7: got '.'

``` r
#this is just a preview of trying to get rid of the "." and change it to NA
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
    ## $ `Average Price (cents/kWh)`    <chr> "11.427155000000001", "12.951298", "20.…

``` r
glimpse(table_Transportation)
```

    ## Rows: 43
    ## Columns: 8
    ## $ Entity                         <chr> "City of North Little Rock - (AR)", "En…
    ## $ State                          <chr> "AR", "AR", "AZ", "AZ", "CA", "CA", "CA…
    ## $ Ownership                      <chr> "Municipal", "Investor Owned", "Politic…
    ## $ `Customers (Count)`            <dbl> 1, 1, 1, 1, 1, 1, 1, 2, 1, 4, 1, 1, 1, …
    ## $ `Sales (Megawatthours)`        <dbl> 213, 6, 9375, 829, 93808, 6968, 909, 10…
    ## $ `Revenues (Thousands Dollars)` <dbl> 33.0, 0.9, 866.0, 115.5, 12338.1, 1322.…
    ## $ `Average Price (cents/kWh)`    <dbl> 15.492958, 15.000000, 9.237333, 13.9324…
    ## $ `==...`                        <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…

``` r
view(table_Transportation)
```

## 3. Ethics review

## 4. Data analysis plan
