---
title: "Analyzing a Superstore dataset"
author: "Laia Porcar, Luis Marcos López, Philippe Robert"
date: "7/17/2022"
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
paste0(file_path, "/share-electricity-renewables.csv")
```

[1] "C:/Users/Luis/Documents/Luis/Universidad/Master_DS/Github_repositories/Business_economic_and_financial_data/share-electricity-renewables.csv"

```r
df_renen <- read.csv(paste0(file_path, "/share-electricity-renewables.csv"),header=TRUE)
df_globwarm <- read.csv(paste0(file_path, "/climate-change.csv"),header=TRUE)

df_renen[1:150,] %>%
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
   <td style="text-align:right;"> 65.9574400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 84.7457660 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 81.1594240 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 67.0212800 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 62.9213500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 63.4408570 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 76.1904750 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 78.9473700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 73.9726000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 82.9787200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 79.7872300 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 76.9230900 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 84.0909100 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 80.1801760 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 86.2069000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 87.2881400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 87.6032940 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 85.8267750 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 82.9059800 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 83.0188750 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 84.6153900 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 19.1974540 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 19.1790120 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 17.7387240 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 18.3610740 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 18.5532740 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 18.2072850 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 18.9173150 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 17.9012010 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 16.9915700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 16.6308210 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 16.7589260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 16.6342960 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 16.2721350 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 16.8194480 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 17.2940860 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 17.6545260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 18.2166460 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 18.0019000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 16.8457780 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 16.8254180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 16.5328200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 16.4986040 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 16.2771700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 16.2968650 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 16.6334820 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 16.9411870 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 17.0357760 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 16.4390180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 16.9646400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 17.7263160 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 17.6801720 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 17.6496770 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 18.6536710 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 19.7232480 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 20.7351440 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 22.4307400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 22.5492200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 19.1974540 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 19.1790140 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 17.7387240 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 18.3610780 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 18.5532740 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 18.2072850 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 18.9173150 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 17.9012010 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 16.9915680 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 16.6308210 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 16.7589260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 16.6342960 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 16.2721350 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 16.8194470 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 17.2940860 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 17.6545260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 18.2166480 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 18.0019000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 16.8457760 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 16.8254180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 16.5328180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 16.4986040 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 16.2771700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 16.2968650 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 16.6334800 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 16.9411870 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 17.0357780 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 16.4390180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 16.9646380 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 17.7263160 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 17.6801720 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 17.6496800 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 18.6536730 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 19.7232480 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 20.7351460 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 22.4307400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Africa (BP) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2021 </td>
   <td style="text-align:right;"> 22.5492200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 86.3636400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 92.1466000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 95.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 93.7677100 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 95.6852800 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 93.9597300 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 95.6594400 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 96.1759100 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 96.2818000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 97.4169700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 96.2264200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 95.4423600 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 93.8502660 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 97.8000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 97.5044560 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 98.7132340 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 98.3695700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 97.5524500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 98.5680160 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2020 </td>
   <td style="text-align:right;"> 100.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1985 </td>
   <td style="text-align:right;"> 5.2631583 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1986 </td>
   <td style="text-align:right;"> 1.9258916 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 3.9223394 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1988 </td>
   <td style="text-align:right;"> 1.3103250 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1989 </td>
   <td style="text-align:right;"> 1.4748107 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.8383011 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 1.6892477 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 1.0882642 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.8182755 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.8348840 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.9789501 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.6536265 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.3488859 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.9242542 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.8199701 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.2124980 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.2591549 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.2061632 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.8974532 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.8032256 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 1.6373285 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.6177080 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.6057357 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.7033327 </td>
  </tr>
</tbody>
</table></div>

```r
df_globwarm[1:15,] %>%
  kable(format = "html", col.names = colnames(df)) %>%
  kable_styling() %>%
  kableExtra::scroll_box(width = "100%", height = "300px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:300px; overflow-x: scroll; width:100%; "><table class="table" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-01-01 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 418.3103 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-01-31 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 425.3770 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-03-01 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 433.5520 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-04-01 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 424.4968 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-05-01 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 423.3627 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-05-31 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 421.4619 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-07-01 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 421.2362 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-07-31 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 422.9604 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-08-30 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 425.3638 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-09-30 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 426.9630 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-10-30 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 426.0322 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1992-11-29 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 427.3198 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1993-01-01 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 430.2821 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1993-01-31 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 434.4374 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antarctica </td>
   <td style="text-align:left;"> 1993-03-02 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 437.6219 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
</tbody>
</table></div>
