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
