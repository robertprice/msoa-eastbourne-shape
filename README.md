# msoa-eastbourne-shape
Shape files for Eastbourne [MSOA](http://webarchive.nationalarchives.gov.uk/20160105160709/http://www.ons.gov.uk/ons/guide-method/geography/beginner-s-guide/census/super-output-areas--soas-/index.html) (Middle Super Output Areas)

## Plotting in R

The MSOA map can be drawn in R using the following.

```r
setwd('/path/to/msoa-eastbourne-shape')
library(rgdal)
msoa <- readOGR(".", "eastbourne_msoa", verbose = FALSE)
plot(msoa, border = "black")
```

The MSOA map can be drawn in R and overlaid on a Google Map using the following code.

```r
setwd('/path/to/msoa-eastbourne-shape')

library(rgdal)
library(ggmap)
library(ggplot2)
library(maptools)

msoa <- readOGR(".", "eastbourne_msoa", verbose = FALSE)
msoa <- spTransform(msoa, CRS("+init=epsg:4326"))
msoa.f <- fortify(msoa, region="MSOA11CD")
bb <- as.vector(msoa@bbox)
map <- get_map(location = bb, maptype = "roadmap", color = "bw", zoom = 12)
ggmap(map, extent = "device") + geom_polygon(data = msoa.f, aes(long,lat, group=group), colour = "black", size = 0.2, fill = NA)
```
