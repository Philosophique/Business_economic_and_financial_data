---
title: "Evolution of solar energy production and use."
author: "Laia Porcar, Luis Marcos López, Philippe Robert"
date: "11/08/2022"
output:
  html_document:
    keep_md: yes
    toc: yes
    df_print: kable
  pdf_document:
    toc: yes
editor_options:
  chunk_output_type: console
---





```r
# Installs
#install.packages("caret")
#install.packages("car")
#install.packages("ggplot2")
#install.packages("devtools")


# Libraries
library(knitr)
library(tidyverse)
library(ggplot2)
library(dplyr)
library(ggcorrplot)
library(devtools) 
library(caret)
library(car)
library(kableExtra)
library(ggpubr)
```




```r
# Read the dataset
file_path <- getwd()
paste(file_path)
```

[1] "C:/Users/Luis/Documents/Luis/Universidad/Master_DS/Github_repositories/Business_economic_and_financial_data"

```r
df_capacity <- read.csv(paste0(file_path, "/installed-solar-PV-capacity.csv"),header=TRUE)
df_capita <- read.csv(paste0(file_path, "/per-capita-solar.csv"),header=TRUE)
df_primary <- read.csv(paste0(file_path, "/primary-energy-consumption-from-solar.csv"),header=TRUE)
df_share <- read.csv(paste0(file_path, "/share-electricity-solar.csv"),header=TRUE)
df_consump <- read.csv(paste0(file_path, "/solar-energy-consumption.csv"),header=TRUE)
df_cumcap <- read.csv(paste0(file_path, "/solar-pv-cumulative-capacity.csv"),header=TRUE)
df_prices <- read.csv(paste0(file_path, "/solar-pv-prices.csv"),header=TRUE)

df_capacity[1:150,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0011124 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0031373 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0051621 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0081994 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0109270 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0130290 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0152430 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0180190 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0219960 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0259650 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0342680 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0466560 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0633730 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.1068450 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.1938420 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.2663880 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.3459540 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.6669730 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.5731200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.9307610 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 2.9884170 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 4.7110630 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 7.2102681 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 8.4153193 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 9.7038594 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 10.3022607 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0010124 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0030373 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0050621 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0080994 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0109270 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0130290 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0152430 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0180190 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0219960 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0259650 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0342680 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0466560 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0633730 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.1068450 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.1938420 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.2663880 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.3459540 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.6669730 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.5731200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.9307610 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 2.9884170 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 4.7110630 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 7.2102681 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 8.4153193 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 9.7038594 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 10.3022598 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0011000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0491000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.2191000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.4000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.4230000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.4230000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 0.4230000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 0.4230000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000250 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0000260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0012520 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0062520 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0082570 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0082520 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0086180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0088060 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0088060 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.1912360 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.4421110 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 0.7641350 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 1.0713660 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0610000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0938000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.1393000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.2214000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.3711200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.5062300 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.7121510 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.9508200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 1.2410060 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 1.6106340 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 1.9724850 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.2989761 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.8780110 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 3.7645859 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 5.6292642 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 9.8840498 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 16.5456895 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 36.5771523 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 61.1705820 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 91.5886172 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 142.6535156 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 217.3108750 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 284.5324375 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 344.2201562 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 423.6035625 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 501.5762813 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0770000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.1125000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.1620000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.2460000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.3956550 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.5345600 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.7451810 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.9884500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 1.2851060 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 1.6591710 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 2.0291639 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.3630061 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.9514031 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 4.0524751 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 6.6251738 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 12.1367725 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 20.0238320 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 40.6440781 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 65.6182344 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 96.1571641 </td>
  </tr>
</tbody>
</table></div>

```r
df_capita[1:150,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0435445 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0529013 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0576104 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0666770 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0791980 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0918344 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1148430 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.1482134 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.2067269 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.2641523 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.5769836 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.5575211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2.4871609 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1.9111619 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 4.4762049 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 7.4213972 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 11.8820553 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 16.6267014 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 20.8468475 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 25.9841919 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 31.4598846 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 31.4637527 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1965 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1966 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1967 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1968 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1969 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1970 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.7018285 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.3633764 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 1.9600090 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 2.0436988 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 4.1760130 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 3.9315450 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 16.1738968 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 32.4067879 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 37.7796211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 37.6540756 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 41.0254326 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 38.9422913 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1965 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1966 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1967 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1968 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1969 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1970 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0013941 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0021100 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0028855 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0033907 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0036416 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0049520 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0052392 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0059637 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0065185 </td>
  </tr>
</tbody>
</table></div>

```r
df_primary[1:150,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0353139 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0439558 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0490436 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0581589 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0707906 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0841344 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1078617 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.1427328 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.2041683 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.2675983 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.5996614 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.6609555 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2.7218072 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 2.1463211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 5.1585293 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 8.7753477 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 14.4134150 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 20.6873131 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 26.5989304 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 33.9889908 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 42.1750641 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 43.2150383 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0353094 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0439494 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0490356 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0581744 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0707933 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0841369 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1078750 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.1427364 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.2041617 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.2676019 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.5996686 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.6609564 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2.7218072 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 2.1463213 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 5.1585288 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 8.7753372 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 14.4134150 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 20.6873131 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 26.5989304 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 33.9889870 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 42.1750641 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 43.2150383 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1965 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1966 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1967 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1968 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1969 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1970 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0252500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0499833 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0732728 </td>
  </tr>
</tbody>
</table></div>

```r
df_share[1:150,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 3.4090910 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 2.7027028 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 2.5862070 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 2.5423730 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 3.3057850 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 3.1496062 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 3.4188035 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 3.7735850 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 5.1282053 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0027127 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0032585 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0034455 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0039406 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0045427 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0052429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0064677 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0082656 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0116517 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0152072 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0321170 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0872215 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.1373856 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.1056404 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.2472638 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.4122445 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.6729101 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.9403634 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.1831368 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.4861330 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 1.8780427 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 1.8401653 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0027127 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0032585 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0034455 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0039406 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0045427 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0052429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0064677 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0082656 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0116517 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0152072 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0321170 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0872215 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.1373856 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.1056404 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.2472638 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.4122445 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.6729102 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.9403635 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.1831368 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.4861331 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 1.8780429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 1.8401653 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.3846154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 0.5649718 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
</tbody>
</table></div>

```r
df_consump[1:150,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0300000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0300000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0300000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0300000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0400000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0400000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0400000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0400000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 0.0400000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0119410 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0149600 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0167997 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0200593 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0245669 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0293835 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0379121 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0504794 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0726539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0958216 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.2160518 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.6020893 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.9926592 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.7875202 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.9041501 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 3.2586064 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 5.3840914 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 7.7734146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 10.0535430 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 12.8942890 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 16.0588050 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 16.5152360 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1971 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1973 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1974 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1975 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0119410 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0149600 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0167997 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0200593 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0245669 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0293835 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0379121 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0504794 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0726539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0958216 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.2160518 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.6020893 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.9926592 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.7875202 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.9041501 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 3.2586064 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 5.3840914 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 7.7734146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 10.0535430 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 12.8942890 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 16.0588050 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 16.5152360 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
</tbody>
</table></div>

```r
df_cumcap[1:150,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 1.112425e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 3.137276e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 5.162126e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 8.199402e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 1.092700e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 1.302900e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 1.524300e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 1.801900e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 2.199600e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 2.596500e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 3.426800e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 4.665600e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 6.337300e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 1.068450e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.938420e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 2.663880e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 3.459540e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 6.669730e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.573120e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.930761e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 2.988417e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 4.711063e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 7.210268e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 8.415319e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 9.703859e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 1.030226e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 1.012425e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 3.037276e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 5.062126e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 8.099402e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 1.092700e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 1.302900e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 1.524300e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 1.801900e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 2.199600e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 2.596500e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 3.426800e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 4.665600e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 6.337300e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 1.068450e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.938420e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 2.663880e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 3.459540e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 6.669730e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.573120e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.930761e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 2.988417e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 4.711063e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 7.210268e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 8.415319e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 9.703859e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 1.030226e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.100000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 4.910000e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 2.191000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 4.000000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 4.230000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 4.230000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 4.230000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 4.230000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.000000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 2.500000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 2.600000e-02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.252000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 6.252000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 8.257000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 8.252000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 8.618000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 8.806000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 8.806000e+00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.912360e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 4.421110e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 7.641350e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 1.071366e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 6.100000e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 9.380000e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.393000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 2.214000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 3.711200e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 5.062300e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 7.121510e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 9.508200e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 1.241006e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 1.610634e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 1.972485e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.298976e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.878011e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 3.764586e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 5.629264e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 9.884050e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 1.654569e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 3.657715e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 6.117058e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 9.158862e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 1.426535e+05 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 2.173109e+05 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 2.845324e+05 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 3.442202e+05 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 4.236036e+05 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 5.015763e+05 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 7.700000e+01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 1.125000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.620000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 2.460000e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 3.956550e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 5.345600e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 7.451810e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 9.884500e+02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 1.285106e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 1.659171e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 2.029164e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.363006e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.951403e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 4.052475e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 6.625174e+03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.213677e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2.002383e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 4.064408e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 6.561823e+04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asia Pacific (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 9.615716e+04 </td>
  </tr>
</tbody>
</table></div>

```r
df_prices[1:150,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1976 </td>
   <td style="text-align:right;"> 106.0862215 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 80.6255283 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1978 </td>
   <td style="text-align:right;"> 56.2256974 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1979 </td>
   <td style="text-align:right;"> 47.7387997 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1980 </td>
   <td style="text-align:right;"> 35.0084531 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1981 </td>
   <td style="text-align:right;"> 26.5215554 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 7 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 22.2781065 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 8 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1983 </td>
   <td style="text-align:right;"> 19.0955199 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 9 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1984 </td>
   <td style="text-align:right;"> 16.9737954 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 14.8520710 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 11 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 11.1390532 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 12 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 8.4868977 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 13 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 7.5321217 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 14 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 8.1686391 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 15 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 8.8051564 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 16 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 7.9564666 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 17 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 7.2138631 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 18 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 7.1077768 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 19 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 6.3651733 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 20 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 5.8347422 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 21 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 6.0469146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 22 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 6.3651733 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 23 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 5.7286560 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 24 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 5.0921386 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 25 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 4.8799662 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 26 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 4.7738800 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 27 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 4.0312764 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 28 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.9782333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 29 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 4.1373626 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 30 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 4.2434489 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 31 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 4.4556213 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 32 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 4.1373626 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 33 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 3.4478022 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 34 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 2.3869400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 35 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 2.0447500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 36 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.6772500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 37 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.8764167 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 38 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.6963333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 39 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.6415000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 40 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.5965000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 41 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.5505000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 42 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.4621667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 43 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.4115000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 44 </td>
   <td style="text-align:left;"> World </td>
   <td style="text-align:left;"> OWID_WRL </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.3772500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.1 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.2 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.3 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.4 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.5 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.6 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.7 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.8 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.9 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.10 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.11 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.12 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.13 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.14 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.15 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.16 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.17 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.18 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.19 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.20 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.21 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.22 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.23 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.24 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.25 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.26 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.27 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.28 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.29 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.30 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.31 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.32 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.33 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.34 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.35 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.36 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.37 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.38 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.39 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.40 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.41 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.42 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.43 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.44 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.45 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.46 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.47 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.48 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.49 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.50 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.51 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.52 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.53 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.54 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.55 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.56 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.57 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.58 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.59 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.60 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.61 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.62 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.63 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.64 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.65 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.66 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.67 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.68 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.69 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.70 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.71 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.72 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.73 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.74 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.75 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.76 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.77 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.78 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.79 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.80 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.81 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.82 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.83 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.84 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.85 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.86 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.87 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.88 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.89 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.90 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.91 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.92 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.93 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.94 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.95 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.96 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.97 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.98 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.99 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.100 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.101 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.102 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.103 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.104 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA.105 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
</tbody>
</table></div>
