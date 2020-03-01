# DataVisualization_A1.2-Minard
Recreation of Minard’s visualization of Napolean’s Russian Campaign in R

### Required dependencies are installed first:
```
install.packages("ggplot")
library(ggplot)
install.packages("ggmap")
library(ggmap)
install.packages("ggrepel")
library(ggrepel)
install.packages("gridExtra")
library(gridExtra)
install.packages("pander")
library(pander)
install.packages("tidyverse")
library(tidyverse)
install.packages("lubridate")
library(lubridate)
```
### Read dataset split into troops, temperature and cities travelled

```

minard_cities <- read.table("minard_cities.txt", header = TRUE, stringsAsFactors = FALSE)

minard_troops <- read.table("minard_troops.txt", header = TRUE, stringsAsFactors = FALSE)

minard_temps <- read.table("minard_temps.txt", header = TRUE, stringsAsFactors = FALSE) %>%
  mutate(date = dmy(date)) 
```
### Create the basic visualization of the path taken throughout the advance and retreat

```
ggplot() +
  geom_path(data = minard_troops, aes(x = long, y = lat, group = group, color = direction, size = survivors), lineend = "round") +
  geom_point(data = minard_cities, aes(x = long, y = lat), color = "#FFFFFF") +
  geom_text_repel(data = minard_cities, aes(x = long, y = lat, label = city), color = "#FFFFFF  ", family = "Arial") +
  scale_size(range = c(0.5, 15)) + 
  scale_colour_manual(values = c("#000000", "#DF242B")) +
  labs(x = NULL, y = NULL) + 
  guides(color = FALSE, size = FALSE)
  ```
  ### Add background map for aesthetics using R's get_stamenmap() method from the Stamen Project
  
  ```
  minard_march.1812.northeast.europe <- c(left = 23.5, bottom = 53.4, right = 38.1, top = 56.3)
  minard_march.1812.northeast.europe  <- get_stamenmap(bbox = minard_march.1812.northeast.europe, zoom = 7, maptype = "watercolor", where = "cache")
  minard_march.1812.total.plot <- ggmap(minard_march.1812.northeast.europe) +
  geom_path(data = minard_troops, aes(x = long, y = lat, group = group, color = direction, size = survivors), lineend = "square") +
  geom_point(data = minard_cities, aes(x = long, y = lat), color = "#FFFFFF") +
  geom_text_repel(data = minard_cities, aes(x = long, y = lat, label = city), color = "#FFFFFF", family = "Arial") +
  scale_size(range = c(0.5, 10)) + 
  scale_colour_manual(values = c("#000000", "#DF242B")) +
  guides(color = FALSE, size = FALSE)
  ```
  
  ### Plot the temperature in accordance with time
  
  ```
  minard_temps.1812.plot <- ggplot(data = minard_temps, aes(x = long, y = temp)) + geom_line() + geom_text(aes(label = temp), vjust = 1.2)
  ```
  
  ### Combine the march and temperature plots and adjust the panel height
  ```
  combined_minard.1812.plot <- rbind(ggplotGrob(minard_march.1812.total.plot), ggplotGrob(minard_temps.1812.plot))
  
  minard_panels <- combined_minard.1812.plot$layout$t[grep("panel", combined_minard.1812.plot$layout$name)]
  
  minard_march.panel.height <- combined_minard.1812.plot$heights[minard_panels][1]
  
  combined_minard.1812.plot$heights[minard_panels] <- unit(c(minard_march.panel.height, 0.1), "null")
  
  grid::grid.newpage()
  
  grid::grid.draw(combined_minard.1812.plot)
  ```
  
 
  
