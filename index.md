---
title       : Belgian Energy Market App
subtitle    : Exploring energy client and supplier data
author      : Mathieu Wauters
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

<style type="text/css">
code.r{ /* Code block */
  font-size: 12px;
}
pre { /* Code block */
  font-size: 12px
}
</style>

## App functionality
The Belgian Energy Market app is a Shiny dashboard that allows energy suppliers and customers to browse relevant open-source data.

* Clients: browse electricity or gas usage in one or multiple municipalities. The average usage is calculated, as well as a drill-down per selected municipality. The data can be explored in a table at the bottom of the page.
* Suppliers: can view and compare their market share with industry peers. Irrelevant peers can be deselected using the menu in the sidebar or in the graph itself (implementation of plotly).

<img class=center src="https://raw.githubusercontent.com/thieumbaland/developingdataproducts_pres/gh-pages/Capture2.PNG" height=250>

--- .class #id

## Getting & Cleaning Data

* The data was downloaded from the website of the Belgian government (http://data.gov.be/nl/search/datasets?f[0]=im_field_category%3A35)
* A separate script `preparation.R` was written for data cleaning. 
* Geolocation information of the municipalities was added using Google's Geocoding API (https://developers.google.com/maps/documentation/geocoding/intro). The locations are visualized using the `leaflet` package. 
* The outcome is a <b>tidy dataset</b> for electricity and gas and for clients and suppliers. This is shown below.

```
## Source: local data frame [6 x 10]
## 
##   Hoofdgemeente Verbruiksjaar       Energie Injectie.afname
##           (chr)         (int)         (chr)           (chr)
## 1         AALST          2013 Elektriciteit        Injectie
## 2         AALST          2014 Elektriciteit          Afname
## 3         AALST          2012       Aardgas          Afname
## 4         AALST          2014 Elektriciteit          Afname
## 5         AALST          2013       Aardgas          Afname
## 6         AALST          2014 Elektriciteit          Afname
## Variables not shown: Sector (chr), Subsector (chr), Toegangspunt (chr),
##   Benaderend.Verbruik (dbl), lat (dbl), lng (dbl).
```

--- .class #id

## Reactivity
The user interface and server are connected by means of reactivity (`reactive()` method in R). An explanation for the client and supplier data is given below:

* Client: as a municipality is changed (added, removed) or the sector is changed, `ui.R` calls `server.R` which then adjust the dataframe in which the data is stored. The visualization (leaflet map) is modified but the data and tables change as well. The overall consumption needs to be recalculated and the plots (made in `plotly`) are modified.
* Supplier: here, reactivity is a bit more straightforward. As the selected supplier changes, the filtering in the underlying dataframe needs to be changed as well and the corresponding visuals will be adapted.

--- .class #id

## Future development

Finally, we can think about possible expansions and improvements for this dashboard. 

* The ultimate goal could be to turn this dashboard into the one-stop shop for energy usage, allowing individual customers to log in, check their usage and compare this to people in the same region and with the same family situation.
* A prediction of future energy use based on data from the previous months or years could be added as well. The course on `Practical Machine Learning` is a great starting point for this.

