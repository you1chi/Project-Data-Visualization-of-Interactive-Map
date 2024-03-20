# Project-Data-Visualization-of-Interactive-Map
> It's a project of data visualization on interactive map, written in R. <br>
> The topic is about the damages (financial damages and deaths included) caused by severe wheather in USA in recent years, which are presented by graduated colored maps and interactive maps. <br>
## Key Steps
*1. generate the base map*
```{r}
us.states <- map_data("state") %>%
  as_tibble(.) %>%
  dplyr::rename(state = region) %>%
  select(-subregion)
```

