# Project-Data-Visualization-of-Interactive-Map
> It's a project of data visualization on interactive map, written in R. <br>
> The topic is about the damages (financial damages and deaths included) caused by severe wheather in USA in recent years, which are presented by graduated colored maps and interactive maps. <br>
## Key Steps
### Graduated Colored Map
*1. generate the base map*
```{r}
us.states <- map_data("state") %>%
  as_tibble(.) %>%
  dplyr::rename(state = region) %>%
  select(-subregion)
```
*2. modify data* <br>
*3. generate the map*
```{r}
map_damage <- ggplot(d_damage, aes(x = long, y = lat, group = group, fill = total_damage)) +
  geom_polygon(color = "white") +
  scale_fill_gradient(n.breaks = 5,low = "slategray2", high = "slategray4", name = "Total Damage") +
  theme_map() +
  labs(title = "Total Monetary Damage by State")+
  geom_text(data=statenames, inherit.aes = FALSE, 
            aes(label=state.abb, x=state.center.x, 
                y=state.center.y),size = 3, colour="gray25")
```
### Interactive Map
*1. a simple example*
```{r}
map_leaflet <- leaflet(d_us) %>%
  addProviderTiles("OpenStreetMap.Mapnik") %>%  
  addMarkers(~BEGIN_LON, ~BEGIN_LAT, popup = paste("Event: ", d_us$EVENT_TYPE, "<br>",
                           "Number of deaths: ", d_us$total_deaths,"<br>",
                           "Year: ", d_us$YEAR,"<br>",
                           "End location: ",d_us$END_LOCATION))
```
*2. a more complex one*
map_leaflet_new<- leaflet(d_us_new) %>%
  addProviderTiles("OpenStreetMap.Mapnik") %>%  
  addCircles(~longitude, ~latitude,color = color_type, radius = 50000,
             popup = paste("Event: ", d_us_new$EVENT_TYPE, "<br>",
                           "Number of deaths: ", d_us_new$total_deaths,"<br>",
                           "Year: ", d_us_new$YEAR,"<br>",
                           "End location: ",d_us_new$END_LOCATION)) %>%
  addLegend(pal = pal, values = ~d_us_new$EVENT_TYPE, title = "Event type")
```
