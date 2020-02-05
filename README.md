# DynamicRThemes
Make RStudio boot a light or dark theme depending on the time of day in your area! This repo contains instructions and code for easy set up.

For those like me that like adaptive themes, I've written some RProfile code that will load either a light or dark theme into R depending on the time of day and your location. 

Writing code that would automatically generate an RProfile file isn't a great idea as it might not play nice with people's existing Profiles. Instead, I've included the code here and instructions on how to install it. 

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
3.) Cut and paste the following code into your RProfile document and then save. Before you do, please read and consider the following.

```{r}
#Please input the location of the closest major city to you, your preferred light theme, #and your preferred dark theme. The defaults are Chicago, Dawn, and Dracula. If you move #or travel, you will need to change this file.

#### input the MAJOR city closest to you and your preferred light and dark themes##
yourcity <- "Chicago"
yourlighttheme <- "Dawn"
yourdarktheme <- "Dracula"
##############################################################################



# Get latitude and longitude from worldcitieslocationsdf
citydf <- worldcitieslocations::mydata
yourlatitude <- worldcitieslocations::mydata[citydf$city == yourcity,3]
yourlongitude <- worldcitieslocations::mydata[citydf$city == yourcity,4]

#When you boot RStudio, checks if it is day or night and boots your chosen themes
setHook("rstudio.sessionInit", function(newSession) {
  if (newSession)
  
    
    currtime <- (Sys.time())
    sunpath <- suncalc::getSunlightTimes(date = as.Date(Sys.time()),
                                         lat = yourlatitude, 
                                         lon = yourlongitude) 
    ifelse(currtime > sunpath$sunrise & currtime < sunpath$sunset,
           rstudioapi::applyTheme(yourlighttheme), 
           rstudioapi::applyTheme(yourdarktheme)) 

  
}, action = "append")
```
  



  