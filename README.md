# DynamicRThemes
Make RStudio boot a light or dark theme depending on the time of day in your area! This repo contains instructions and code for easy set up. Requires latest version of R (3.6). You will also need RStudio > 1.2.5033 or RMarkdown will crash Rstudio.

1.) Install required packages:

Run the following code below to download the required packages.

```{r}
install.packages("rstudioapi","suncalc","devtools")
devtools::install_github("hamsamilton/worldcitieslocations")
```
2.) Open your RProfile file:

Run the following code to open your RProfile
```{r}
file.edit(file.path("~", ".Rprofile")) # edit .Rprofile in HOME
```
3.) Cut and paste the following code into your RProfile document and then save. 

```{r}


#When you boot RStudio, checks if it is day or night and boots your chosen themes
setHook("rstudio.sessionInit", function(newSession) {
  if (newSession)
    
  #### input the MAJOR city closest to you and your preferred light and dark themes##
  yourcity <- "Chicago"
  yourlighttheme <- "Dawn"
  yourdarktheme <- "Dracula"
  ##############################################################################
  
  
  
  # Get latitude and longitude from worldcitieslocationsdf
  citydf <- worldcitieslocations::mydata
  yourlatitude <- worldcitieslocations::mydata[citydf$city == yourcity,3]
  yourlongitude <- worldcitieslocations::mydata[citydf$city == yourcity,4]
  
    
    
    currtime <- (Sys.time())
  sunpath <- suncalc::getSunlightTimes(date = as.Date(Sys.time()),
                                       lat = yourlatitude, 
                                       lon = yourlongitude) 
  ifelse(currtime > sunpath$sunrise & currtime < sunpath$sunset,
         rstudioapi::applyTheme(yourlighttheme), 
         rstudioapi::applyTheme(yourdarktheme)) 
  # Clean the environment
  remove("yourcity","yourdarktheme","yourlatitude","yourlighttheme","yourlongitude","citydf")
  
}, action = "append")

```
  



  
