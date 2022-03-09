---
title: 'Weekly Exercises #4'
author: "Gabriela Diaz"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(openintro)     # for the abbr2state() function
```

```
## Loading required package: airports
```

```
## Loading required package: cherryblossom
```

```
## Loading required package: usdata
```

```r
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(ggmap)         # for mapping points on maps
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(gplots)        # for col2hex() function
```

```
## 
## Attaching package: 'gplots'
```

```
## The following object is masked from 'package:stats':
## 
##     lowess
```

```r
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
```

```
## Linking to GEOS 3.9.1, GDAL 3.4.0, PROJ 8.1.1; sf_use_s2() is TRUE
```

```r
library(leaflet)       # for highly customizable mapping
library(carData)       # for Minneapolis police stops data
library(ggthemes)      # for more themes (including theme_map())
theme_set(theme_minimal())
```


```r
# Starbucks locations
Starbucks <- read_csv("https://www.macalester.edu/~ajohns24/Data/Starbucks.csv")
```

```
## Rows: 25600 Columns: 13
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (11): Brand, Store Number, Store Name, Ownership Type, Street Address, C...
## dbl  (2): Longitude, Latitude
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
starbucks_us_by_state <- Starbucks %>% 
  filter(Country == "US") %>% 
  count(`State/Province`) %>% 
  mutate(state_name = str_to_lower(abbr2state(`State/Province`))) 

# Lisa's favorite St. Paul places - example for you to create your own data
favorite_stp_by_lisa <- tibble(
  place = c("Home", "Macalester College", "Adams Spanish Immersion", 
            "Spirit Gymnastics", "Bama & Bapa", "Now Bikes",
            "Dance Spectrum", "Pizza Luce", "Brunson's"),
  long = c(-93.1405743, -93.1712321, -93.1451796, 
           -93.1650563, -93.1542883, -93.1696608, 
           -93.1393172, -93.1524256, -93.0753863),
  lat = c(44.950576, 44.9378965, 44.9237914,
          44.9654609, 44.9295072, 44.9436813, 
          44.9399922, 44.9468848, 44.9700727)
  )

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

```
## Rows: 40606 Columns: 5
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): state, fips
## dbl  (2): cases, deaths
## date (1): date
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Put your homework on GitHub!

If you were not able to get set up on GitHub last week, go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) and get set up first. Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 4th weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab under Stage and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 


## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises from tutorial

These exercises will reiterate what you learned in the "Mapping data with R" tutorial. If you haven't gone through the tutorial yet, you should do that first.

### Starbucks locations (`ggmap`)

  1. Add the `Starbucks` locations to a world map. Add an aesthetic to the world map that sets the color of the points according to the ownership type. What, if anything, can you deduce from this visualization?  

```r
world <- get_stamenmap(
    bbox = c(left = -180, bottom = -57, right = 179, top = 82.1), 
    maptype = "terrain",
    zoom = 2)
```

```
## Source : http://tile.stamen.com/terrain/2/0/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/2.png
```

```r
ggmap(world) + 
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude, color= `Ownership Type`), 
             alpha = .3, 
             size = .1) +
  theme_map()
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

  2. Construct a new map of Starbucks locations in the Twin Cities metro area (approximately the 5 county metro area).  

```r
twin_cities_metro<-get_stamenmap(
    bbox = c(left = -93.4847, bottom = 44.7501, right = -92.7363, top = 45.1302), 
    maptype = "terrain",
    zoom = 10)
```

```
## Source : http://tile.stamen.com/terrain/10/246/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/247/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/248/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/246/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/247/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/248/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/246/369.png
```

```
## Source : http://tile.stamen.com/terrain/10/247/369.png
```

```
## Source : http://tile.stamen.com/terrain/10/248/369.png
```

```r
ggmap(twin_cities_metro) +
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude), 
             alpha = .8, 
             size = .5) +
  theme_map()
```

```
## Warning: Removed 25488 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-2-1.png)<!-- -->


  3. In the Twin Cities plot, play with the zoom number. What does it do?  (just describe what it does - don't actually include more than one map).  
  The bigger I made the zoom number the further zoomed out it is and the easier it is to read. The smaller the zoom number it becomes difficult to interpret the map and is blurry.

  4. Try a couple different map types (see `get_stamenmap()` in help and look at `maptype`). Include a map with one of the other map types.  

```r
twin_cities_metro<-get_stamenmap(
    bbox = c(left = -93.4847, bottom = 44.7501, right = -92.7363, top = 45.1302), 
    maptype = "watercolor",
    zoom = 10)
```

```
## Source : http://tile.stamen.com/watercolor/10/246/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/247/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/248/367.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/246/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/247/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/248/368.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/246/369.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/247/369.jpg
```

```
## Source : http://tile.stamen.com/watercolor/10/248/369.jpg
```

```r
ggmap(twin_cities_metro) +
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude), 
             alpha = .8, 
             size = .5) +
  theme_map()
```

```
## Warning: Removed 25488 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->


  5. Add a point to the map that indicates Macalester College and label it appropriately. There are many ways you can do think, but I think it's easiest with the `annotate()` function (see `ggplot2` cheatsheet).

```r
twin_cities_metro<-get_stamenmap(
    bbox = c(left = -93.4847, bottom = 44.7501, right = -92.7363, top = 45.1302), 
    maptype = "toner",
    zoom = 10)
```

```
## Source : http://tile.stamen.com/toner/10/246/367.png
```

```
## Source : http://tile.stamen.com/toner/10/247/367.png
```

```
## Source : http://tile.stamen.com/toner/10/248/367.png
```

```
## Source : http://tile.stamen.com/toner/10/246/368.png
```

```
## Source : http://tile.stamen.com/toner/10/247/368.png
```

```
## Source : http://tile.stamen.com/toner/10/248/368.png
```

```
## Source : http://tile.stamen.com/toner/10/246/369.png
```

```
## Source : http://tile.stamen.com/toner/10/247/369.png
```

```
## Source : http://tile.stamen.com/toner/10/248/369.png
```

```r
ggmap(twin_cities_metro) +
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude), 
             alpha = .8, 
             size = .5) +
  annotate(geom = "point",
           x=-93.1691,
           y=44.9379,
           color="orange",
           size=2)+
  theme_map()+
  annotate(geom = "text",
           x=-93.1691,
           y=44.9379,
           label="Macalester College",
           color="orange",
           size=3)
```

```
## Warning: Removed 25488 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

  

### Choropleth maps with Starbucks data (`geom_map()`)

The example I showed in the tutorial did not account for population of each state in the map. In the code below, a new variable is created, `starbucks_per_10000`, that gives the number of Starbucks per 10,000 people. It is in the `starbucks_with_2018_pop_est` dataset.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))
```

```
## Rows: 51 Columns: 2
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): state
## dbl (1): est_pop_2018
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
starbucks_with_2018_pop_est <-
  starbucks_us_by_state %>% 
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>% 
  mutate(starbucks_per_10000 = (n/est_pop_2018)*10000)
```

  6. **`dplyr` review**: Look through the code above and describe what each line of code does.
#Pulls up the data we are going to be using, which in this case is the estimated population in 2018 of each US State.
  - Separates the state variable in the data into two columns, dot, and state.
  - Removes the dot column from the data.
  - Creates a new data set.
  - Uses the starbucks_us_by_state data as a base and the pipe indicates we want whatever changes below to be added to that data set.
  - Taking the variable state name from the census data and changing it's name to state.
  - Creating a new variable named starbucks_per_10000 that gives us the number of starbucks per 10000 people given the est_pop_2018 data.

  7. Create a choropleth map that shows the number of Starbucks per 10,000 people on a map of the US. Use a new fill color, add points for all Starbucks in the US (except Hawaii and Alaska), add an informative title for the plot, and include a caption that says who created the plot (you!). Make a conclusion about what you observe.

```r
starbucks_us_by_state <- Starbucks %>% 
  filter(Country == "US") %>% 
  count(`State/Province`) %>% 
  mutate(state_name = str_to_lower(abbr2state(`State/Province`)))

states_map <- map_data("state")

starbucks_us_by_state %>% 
  left_join(census_pop_est_2018,
            by =c("state_name"="state")) %>% 
  mutate(starbucksper10=(n/est_pop_2018)*10000) %>% 
 ggplot() +
 geom_map(map = states_map,
           aes(map_id = state_name,
               fill = starbucksper10)) +
   geom_point(data = Starbucks %>% 
                filter(Country == "US" , 
                       !(`State/Province` %in% c("HI", "AK"))),
             aes(x = Longitude, y = Latitude),
             size = .05,
             alpha = .2, 
             color = "goldenrod")+
  expand_limits(x = states_map$long, y = states_map$lat) + 
  labs(title = "Starbucks per 10000 Americans")+
  theme_map()+
  theme(legend.background = element_blank())
```

![](04_exercises_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


### A few of your favorite things (`leaflet`)

  8. In this exercise, you are going to create a single map of some of your favorite places! The end result will be one map that satisfies the criteria below. 

  * Create a data set using the `tibble()` function that has 10-15 rows of your favorite places. The columns will be the name of the location, the latitude, the longitude, and a column that indicates if it is in your top 3 favorite locations or not. For an example of how to use `tibble()`, look at the `favorite_stp_by_lisa` I created in the data R code chunk at the beginning.  

```r
favorite_places<-tibble(
  name_of_location=c("Time Market", "Central Park", "Godparents house", "Home", "Park City", "Punta Cana", "Crisp & Green", "The Chop Shop", "Blue Bottle Coffee", "Jackson Hole"),
  long = c(-110.9643,-73.9665,-108.6829,-110.9187,-111.4980,
           -68.3725,-93.5100,-111.9293,-74.0018,-110.7624),
  lat=c(32.2315,40.7812,39.1038,32.2979,40.6461, 
        18.5601, 44.9690,33.5028, 40.7531,43.4799),
  top3yesorno=c("yes","yes","yes", "no", "no", "no", "no","no","no","no")
)
favorite_places <- favorite_places %>% mutate(top3yesorno = as.factor(top3yesorno))
```

  * Create a `leaflet` map that uses circles to indicate your favorite places. Label them with the name of the place. Choose the base map you like best. Color your 3 favorite places differently than the ones that are not in your top 3 (HINT: `colorFactor()`). Add a legend that explains what the colors mean.

```r
pal<-colorFactor("viridis",
                 domain=favorite_places$top3yesorno)
leaflet(data = favorite_places) %>% 
  addTiles() %>% 
  addCircles(lng = ~long, 
             lat = ~lat, 
             label = ~name_of_location,
             weight = 10, 
             opacity = 1, 
             color = ~pal(top3yesorno)) %>% 
  addPolylines(lng = ~long, 
               lat = ~lat, 
               color = ("crimson")) %>% 
  addLegend(position= "bottomright",pal=pal,values=~top3yesorno )
```

```{=html}
<div id="htmlwidget-fff66f87d5afa56c820f" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-fff66f87d5afa56c820f">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircles","args":[[32.2315,40.7812,39.1038,32.2979,40.6461,18.5601,44.969,33.5028,40.7531,43.4799],[-110.9643,-73.9665,-108.6829,-110.9187,-111.498,-68.3725,-93.51,-111.9293,-74.0018,-110.7624],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#FDE725","#FDE725","#FDE725","#440154","#440154","#440154","#440154","#440154","#440154","#440154"],"weight":10,"opacity":1,"fill":true,"fillColor":["#FDE725","#FDE725","#FDE725","#440154","#440154","#440154","#440154","#440154","#440154","#440154"],"fillOpacity":0.2},null,null,["Time Market","Central Park","Godparents house","Home","Park City","Punta Cana","Crisp &amp; Green","The Chop Shop","Blue Bottle Coffee","Jackson Hole"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addPolylines","args":[[[[{"lng":[-110.9643,-73.9665,-108.6829,-110.9187,-111.498,-68.3725,-93.51,-111.9293,-74.0018,-110.7624],"lat":[32.2315,40.7812,39.1038,32.2979,40.6461,18.5601,44.969,33.5028,40.7531,43.4799]}]]],null,null,{"interactive":true,"className":"","stroke":true,"color":"crimson","weight":5,"opacity":0.5,"fill":false,"fillColor":"crimson","fillOpacity":0.2,"smoothFactor":1,"noClip":false},null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addLegend","args":[{"colors":["#440154","#FDE725"],"labels":["no","yes"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"bottomright","type":"factor","title":"top3yesorno","extra":null,"layerId":null,"className":"info legend","group":null}]}],"limits":{"lat":[18.5601,44.969],"lng":[-111.9293,-68.3725]}},"evals":[],"jsHooks":[]}</script>
```
  
  
  * Connect all your locations together with a line in a meaningful way (you may need to order them differently in the original data).  
  
  * If there are other variables you want to add that could enhance your plot, do that now.  
  
## Revisiting old datasets

This section will revisit some datasets we have used previously and bring in a mapping component. 

### Bicycle-Use Patterns

The data come from Washington, DC and cover the last quarter of 2014.

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`. This code reads in the large dataset right away.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## Rows: 347 Columns: 5
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): name
## dbl (4): lat, long, nbBikes, nbEmptyDocks
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

  9. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. This time, plot the points on top of a map. Use any of the mapping tools you'd like.
  

```r
departures<-Trips %>% 
  group_by(sstation) %>% 
  summarise(sum_station=n()) %>% 
  left_join(Stations,
         by=c("sstation"="name"))
  msp_map<-get_stamenmap(
    bbox = c(left = -77.3, bottom = 38.60111, right = -76.8, top = 39.12351), 
    maptype = "toner",
    zoom = 10)
```

```
## Source : http://tile.stamen.com/toner/10/292/390.png
```

```
## Source : http://tile.stamen.com/toner/10/293/390.png
```

```
## Source : http://tile.stamen.com/toner/10/292/391.png
```

```
## Source : http://tile.stamen.com/toner/10/293/391.png
```

```
## Source : http://tile.stamen.com/toner/10/292/392.png
```

```
## Source : http://tile.stamen.com/toner/10/293/392.png
```

```r
  ggmap(msp_map) + 
  geom_point(data = departures, 
             aes(x = long, y = lat, color= sum_station), 
             alpha = 1, 
             size = 1)+
    scale_color_viridis_c()
```

```
## Warning: Removed 13 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
  10. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? Also plot this on top of a map. I think it will be more clear what the patterns are.
  

```r
proportion_casual<-Trips %>% 
  group_by(sstation, client) %>% 
  summarise(sum_station=n()) %>%
  group_by(sstation) %>% 
  mutate(total=sum(sum_station),
         prop=sum_station/total) %>% 
  filter(client=="Casual") %>% 
  left_join(Stations,
         by=c("sstation"="name")) 
```

```
## `summarise()` has grouped output by 'sstation'. You can override using the `.groups` argument.
```

```r
  ggmap(msp_map) + 
  geom_point(data=proportion_casual,aes(x=long, y=lat,color=prop))+
   labs(title="Which areas have stations with a higher percentage of \ndepartures by casual users", x="longitude", y="latitude")
```

```
## Warning: Removed 13 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
  
### COVID-19 data

The following exercises will use the COVID-19 data from the NYT.

  11. Create a map that colors the states by the most recent cumulative number of COVID-19 cases (remember, these data report cumulative numbers so you don't need to compute that). Describe what you see. What is the problem with this map?

```r
  cum_covid <- covid19 %>% 
  filter(date==max(date)) %>% 
  mutate(state=str_to_lower(state))

states_map <- map_data("state")

cum_covid %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = cases)) +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()+
  scale_fill_viridis_c()
```

![](04_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

  
  12. Now add the population of each state to the dataset and color the states by most recent cumulative cases/10,000 people. See the code for doing this with the Starbucks data. You will need to make some modifications.

```r
covid_over_10 <- covid19 %>% 
  filter(date==max(date)) %>% 
  mutate(state=str_to_lower(state)) %>% 
   left_join(census_pop_est_2018,
            by = "state") %>% 
  mutate(cases_per_10000 = (cases/est_pop_2018)*10000)

states_map <- map_data("state")

covid_over_10 %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = cases_per_10000)) +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()+
  scale_fill_viridis_c()
```

![](04_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  


  
  13. **CHALLENGE** Choose 4 dates spread over the time period of the data and create the same map as in exercise 12 for each of the dates. Display the four graphs together using faceting. What do you notice?
  
## Minneapolis police stops

These exercises use the datasets `MplsStops` and `MplsDemo` from the `carData` library. Search for them in Help to find out more information.

  14. Use the `MplsStops` dataset to find out how many stops there were for each neighborhood and the proportion of stops that were for a suspicious vehicle or person. Sort the results from most to least number of stops. Save this as a dataset called `mpls_suspicious` and display the table.  

```r
mpls_suspicious<-MplsStops %>% 
  group_by(neighborhood) %>% 
  summarise(n=n(),poportion_of_suspicious=mean(problem=="suspicious")) %>% 
  arrange(desc(n))

mpls_suspicious
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["neighborhood"],"name":[1],"type":["fct"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]},{"label":["poportion_of_suspicious"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"Downtown West","2":"4409","3":"0.7570878"},{"1":"Whittier","2":"3328","3":"0.4059495"},{"1":"Near - North","2":"2256","3":"0.4153369"},{"1":"Lyndale","2":"2154","3":"0.4048282"},{"1":"Jordan","2":"2075","3":"0.3918072"},{"1":"Hawthorne","2":"2031","3":"0.3865091"},{"1":"Marcy Holmes","2":"1798","3":"0.2875417"},{"1":"Lowry Hill East","2":"1491","3":"0.4044266"},{"1":"East Phillips","2":"1387","3":"0.6914203"},{"1":"Folwell","2":"1230","3":"0.3943089"},{"1":"Willard - Hay","2":"1207","3":"0.4407622"},{"1":"Holland","2":"1169","3":"0.2728828"},{"1":"Ventura Village","2":"1096","3":"0.6989051"},{"1":"Powderhorn Park","2":"1055","3":"0.7071090"},{"1":"Midtown Phillips","2":"1019","3":"0.7526987"},{"1":"Steven's Square - Loring Heights","2":"1006","3":"0.4443340"},{"1":"Nicollet Island - East Bank","2":"945","3":"0.1724868"},{"1":"King Field","2":"846","3":"0.2943262"},{"1":"Central","2":"832","3":"0.6346154"},{"1":"Cedar Riverside","2":"825","3":"0.6824242"},{"1":"North Loop","2":"799","3":"0.4167710"},{"1":"McKinley","2":"772","3":"0.4080311"},{"1":"Loring Park","2":"741","3":"0.7395412"},{"1":"Phillips West","2":"726","3":"0.6418733"},{"1":"Webber - Camden","2":"656","3":"0.5868902"},{"1":"Longfellow","2":"603","3":"0.7313433"},{"1":"Prospect Park - East River Road","2":"594","3":"0.3535354"},{"1":"CARAG","2":"559","3":"0.4186047"},{"1":"Audubon Park","2":"554","3":"0.3718412"},{"1":"Tangletown","2":"547","3":"0.1791590"},{"1":"Elliot Park","2":"544","3":"0.7794118"},{"1":"East Isles","2":"530","3":"0.2113208"},{"1":"Seward","2":"510","3":"0.7450980"},{"1":"Victory","2":"498","3":"0.5321285"},{"1":"St. Anthony West","2":"475","3":"0.2694737"},{"1":"Windom Park","2":"461","3":"0.3427332"},{"1":"Como","2":"452","3":"0.3053097"},{"1":"Windom","2":"404","3":"0.4529703"},{"1":"Harrison","2":"401","3":"0.6109726"},{"1":"Bottineau","2":"377","3":"0.2546419"},{"1":"Corcoran","2":"360","3":"0.6111111"},{"1":"Cleveland","2":"356","3":"0.5786517"},{"1":"Logan Park","2":"355","3":"0.2225352"},{"1":"Marshall Terrace","2":"355","3":"0.2619718"},{"1":"Lind - Bohanon","2":"344","3":"0.7906977"},{"1":"Northeast Park","2":"326","3":"0.3987730"},{"1":"Sheridan","2":"318","3":"0.3176101"},{"1":"ECCO","2":"308","3":"0.6103896"},{"1":"Mid - City Industrial","2":"278","3":"0.2769784"},{"1":"Downtown East","2":"262","3":"0.5458015"},{"1":"Lynnhurst","2":"245","3":"0.3510204"},{"1":"Waite Park","2":"244","3":"0.5901639"},{"1":"Lowry Hill","2":"243","3":"0.4814815"},{"1":"Hiawatha","2":"235","3":"0.6851064"},{"1":"Linden Hills","2":"218","3":"0.5688073"},{"1":"St. Anthony East","2":"218","3":"0.3027523"},{"1":"University of Minnesota","2":"218","3":"0.3853211"},{"1":"Standish","2":"212","3":"0.8490566"},{"1":"Beltrami","2":"211","3":"0.2511848"},{"1":"Howe","2":"196","3":"0.7448980"},{"1":"Kenwood","2":"193","3":"0.2487047"},{"1":"Northrop","2":"189","3":"0.9100529"},{"1":"East Harriet","2":"169","3":"0.4733728"},{"1":"Cedar - Isles - Dean","2":"153","3":"0.3529412"},{"1":"Columbia Park","2":"151","3":"0.4238411"},{"1":"Diamond Lake","2":"149","3":"0.7718121"},{"1":"Regina","2":"142","3":"0.7605634"},{"1":"Ericsson","2":"136","3":"0.7720588"},{"1":"Bancroft","2":"134","3":"0.8432836"},{"1":"Shingle Creek","2":"132","3":"0.8030303"},{"1":"Fulton","2":"130","3":"0.6923077"},{"1":"Bryn - Mawr","2":"125","3":"0.6240000"},{"1":"Sumner - Glenwood","2":"123","3":"0.5691057"},{"1":"Kenny","2":"118","3":"0.5508475"},{"1":"Keewaydin","2":"115","3":"0.9130435"},{"1":"Minnehaha","2":"113","3":"0.8407080"},{"1":"Cooper","2":"112","3":"0.5000000"},{"1":"Wenonah","2":"112","3":"0.9017857"},{"1":"Bryant","2":"96","3":"0.8020833"},{"1":"Field","2":"87","3":"0.7816092"},{"1":"West Calhoun","2":"80","3":"0.5625000"},{"1":"Armatage","2":"77","3":"0.8441558"},{"1":"Morris Park","2":"74","3":"0.9594595"},{"1":"Hale","2":"61","3":"0.7868852"},{"1":"Page","2":"41","3":"0.7804878"},{"1":"Camden Industrial","2":"34","3":"0.3529412"},{"1":"Humboldt Industrial Area","2":"10","3":"0.4000000"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  
  15. Use a `leaflet` map and the `MplsStops` dataset to display each of the stops on a map as a small point. Color the points differently depending on whether they were for suspicious vehicle/person or a traffic stop (the `problem` variable). HINTS: use `addCircleMarkers`, set `stroke = FAlSE`, use `colorFactor()` to create a palette.  
  
  16. Save the folder from moodle called Minneapolis_Neighborhoods into your project/repository folder for this assignment. Make sure the folder is called Minneapolis_Neighborhoods. Use the code below to read in the data and make sure to **delete the `eval=FALSE`**. Although it looks like it only links to the .sph file, you need the entire folder of files to create the `mpls_nbhd` data set. These data contain information about the geometries of the Minneapolis neighborhoods. Using the `mpls_nbhd` dataset as the base file, join the `mpls_suspicious` and `MplsDemo` datasets to it by neighborhood (careful, they are named different things in the different files). Call this new dataset `mpls_all`.


```r
mpls_nbhd <- st_read("Minneapolis_Neighborhoods/Minneapolis_Neighborhoods.shp", quiet = TRUE)
```

  17. Use `leaflet` to create a map from the `mpls_all` data  that colors the neighborhoods by `prop_suspicious`. Display the neighborhood name as you scroll over it. Describe what you observe in the map.
  
  18. Use `leaflet` to create a map of your own choosing. Come up with a question you want to try to answer and use the map to help answer that question. Describe what your map shows. 
  
  
## GitHub link

  19. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 04_exercises.Rmd, provide a link to the 04_exercises.md file, which is the one that will be most readable on GitHub.
  https://github.com/gdiaz987/exercise4.git


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
