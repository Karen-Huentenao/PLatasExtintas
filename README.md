Trabajo con plantas extintas
================

# Introduccon

en este documento trabajremos para explorar la identidad de plantas que
se encuentran extintas en silvestria o totalmente extintas segun
[**IUCN**](https://www.iucnredlist.org/)

## Trabajando con los datos

vamos a partir por cargar los paquetes necesarios para trabajar

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.3     v purrr   0.3.4
    ## v tibble  3.1.1     v dplyr   1.0.5
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   1.4.0     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rmarkdown)
```

Ahora voy a leer los datos con los que voy a trabajar:

``` r
plants <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-08-18/plants.csv")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   .default = col_double(),
    ##   binomial_name = col_character(),
    ##   country = col_character(),
    ##   continent = col_character(),
    ##   group = col_character(),
    ##   year_last_seen = col_character(),
    ##   red_list_category = col_character()
    ## )
    ## i Use `spec()` for the full column specifications.

## Filtrando los datos para resolver el ejemplo 1

el codigo que voy a ejecutar ahora es para resolver el problema en el
siguiente \[slide\] deberi air el link d ela diapo pero no lo tengo para
poner en la base de datos solo los datos de chile y solo usar las
columnas para pais (*country*), la especie (*binomial\_name*) y la
categoria de IUCN (*red\_list\_category*)

``` r
Chile <- plants %>% 
  dplyr::filter(country == "Chile")

Chile
```

    ## # A tibble: 2 x 24
    ##   binomial_name    country continent group   year_last_seen threat_AA threat_BRU
    ##   <chr>            <chr>   <chr>     <chr>   <chr>              <dbl>      <dbl>
    ## 1 Santalum fernan~ Chile   South Am~ Flower~ 1900-1919              0          1
    ## 2 Sophora toromiro Chile   South Am~ Flower~ 1920-1939              0          0
    ## # ... with 17 more variables: threat_RCD <dbl>, threat_ISGD <dbl>,
    ## #   threat_EPM <dbl>, threat_CC <dbl>, threat_HID <dbl>, threat_P <dbl>,
    ## #   threat_TS <dbl>, threat_NSM <dbl>, threat_GE <dbl>, threat_NA <dbl>,
    ## #   action_LWP <dbl>, action_SM <dbl>, action_LP <dbl>, action_RM <dbl>,
    ## #   action_EA <dbl>, action_NA <dbl>, red_list_category <chr>

seleccionamos un par de columnas

``` r
Chile <- plants %>% 
  dplyr::filter(country == "Chile") %>% 
  dplyr::select(binomial_name, country)

Chile
```

    ## # A tibble: 2 x 2
    ##   binomial_name           country
    ##   <chr>                   <chr>  
    ## 1 Santalum fernandezianum Chile  
    ## 2 Sophora toromiro        Chile

agregamos el ref list category para saber cual de estas especies esta
extinta en silvestria

``` r
Chile <- plants %>% 
  dplyr::filter(country == "Chile") %>% 
  dplyr::select(binomial_name, country, red_list_category)

Chile
```

    ## # A tibble: 2 x 3
    ##   binomial_name           country red_list_category  
    ##   <chr>                   <chr>   <chr>              
    ## 1 Santalum fernandezianum Chile   Extinct            
    ## 2 Sophora toromiro        Chile   Extinct in the Wild

## Resumen de especies por pais

``` r
Resumen <- plants %>%
  dplyr::filter(continent == "South America") %>% 
  group_by(country) %>% 
  summarise(n_species = n())
Resumen
```

    ## # A tibble: 9 x 2
    ##   country             n_species
    ##   <chr>                   <int>
    ## 1 Argentina                   1
    ## 2 Bolivia                     1
    ## 3 Brazil                     10
    ## 4 Chile                       2
    ## 5 Colombia                    6
    ## 6 Ecuador                    52
    ## 7 Peru                        4
    ## 8 Trinidad and Tobago         6
    ## 9 Venezuela                   1
