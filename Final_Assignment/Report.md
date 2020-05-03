# Apartment house for seniors in Prague
This is a project for Coursera Applied Data Science Capstone course. It show's my personal ideas about problem that I selected to try to solve. The file contains several parts:
  - [Introduction/Business Problem](#intro)
  - [Data](#Data)
  - [Methodology](#Methodology)

<h2 id="intro">Introduction/Business Problem </h2>
Problem that I am trying to solve, is to determine which districts of given city (I selected Prague, in CZ) are most suitable for real estate development copmany, that wants to plan and build apartment house for seniors.

The answer to question how this can be interesting for such company is obvious: Average age in local population is increasing, and also wealth of individuals. Especially elderly people have more money to spend and they are very reliable target group. If real estate development company sells or rent new apartments to seniors, it is clear that they will be very good renters, especially in compare with students or other young people - these did not have time to cumulate money during their lifes, and their lifes are more unstable in general.

However, the problem also is, that Prague is capital city of Czech Republic and it has many districts. Some of them are more noisy than others, some of them are purely industrial, some of them does not have any facilites such as foodstores, and so on.

For figuring out solution for this problem, we will do multiple clustering of districts according to different criteria. This will show us how districts can be grouped according each of criteria. Each of this criteria can be quantified and it can be told, whether higher values of that criteria are better or worse for purpose of the project. Then we will try to find intersection of districts, that were grouped into best, or at least not worst (for criteria that are less important) clusters according to each criteria.

__Foursquare__ data are especially important in this case, because they can help us to cluster city districts, according to numbers or density of different types of venues. If we want to cluster districts according to multiple different parameters, we must know which parameters are important for us. In our case, Foursquare can provide us data for parameters such as number of stores, pharmacies, or accessibility of district by public transport.

## Data
Firstly, we need list of districts in Prague. This list will be obtained from following link (it's written in czech, because on english Wikipedia there is not enough information, but in final notebook, header will be translated to EN): https://cs.wikipedia.org/wiki/Seznam_katastr%C3%A1ln%C3%ADch_%C3%BAzem%C3%AD_v_Praze

Secondly, we will need information about each district location and shape, for that we will use OpenStreetMaps, that will give us exact polygon on map, that defines given district.

Finally, recall that purpose of the project is to determine which districts in the city (Prague) are most suitable for real estate development company, that wants to build apartment house for seniors, and then sell or rent individual apartments. Because of this fact, we will be interested in several parameters, that may be interesting for target clients/buyers/renters (elderly people/seniors).

Selected parameters, and their datasources are following:

Description | Ideal value | data source | details
--- | --- | --- | ---
Number of foodstores in district per square kilometer | Higher is better | Foursquare | Foursquare can return venues in given area.*
Number of health facilites (such as pharmacies, hospitals,...) in district per square kilometer | Higher is better | Foursquare | Foursquare can return venues in given area.*
Number of social venues that may produce noise (such as hotels or bars,...) in district per square kilometer | Lower is better | Foursquare | Foursquare can return venues in given area.*
Number of public transport venues (such as bus stops, subway stations,...) in district per square kilometer | Higher is better | Foursquare | Foursquare can return venues in given area.*
Percentage of land that is covered by nature (parks, forests,...) in each district | Higher is better | OpenStreetMaps | OpenStreetMaps can provide data about land usage for given area.**
Percentage of land that is covered by industrials zones in each district | Lower is better | OpenStreetMaps | OpenStreetMaps can provide data about land usage for given area.**
Percentage of land that is covered by commercial zones in each district | Lower is better | OpenStreetMaps | OpenStreetMaps can provide data about land usage for given area.**
Population density per square kilometer | Lower is better | Wikipedia | [link](https://cs.wikipedia.org/wiki/Seznam_katastr%C3%A1ln%C3%ADch_%C3%BAzem%C3%AD_v_Praze) (in czech, but in notebook I will translate header to english)

_*Foursquare can return venues in given bounding box, however, shape of district may be polygon. Data that will be outside of this polygon have to be removed. Also, absolute number of venues has to be recalculated to average value per kilometer._
_**OpenStreetMaps (OSM) can return small nodes, that defines small polygons of different land usages in given bounding box, however, shape of district may be also polygon. Data that will be outside of this polygon have to be removed. Also, size of polygons of all landusage will be calculated, and then converted to percentage of whole district area._

## Methodology
Methodology was splitted into two parts: Processing data to format that we want, and clustering districts according to each given parameter.

### Preprocessing data
__For the first part__, there was three data sources: Wikipedia, OpenStreetMap, and Foursquare.

From Wikipedia, we could obtain a list of districts in Prague, from [this link](https://cs.wikipedia.org/wiki/Seznam_katastr%C3%A1ln%C3%ADch_%C3%BAzem%C3%AD_v_Praze). We parsed it using pandas and we also used information about population density in each district, instead of just using district's name.

From OpenStreetMaps, we got list of points that defines each district's shape on map. We also used OSM for getting information about different land usages in each district. Because it is not possible to query OSM directly for land usage in given polygon, only in given rectangle, we used bounding box of each district, and when we recieved data from OSM, we used [Shapely](https://shapely.readthedocs.io/en/latest/manual.html) library, which has function for determining whether point is inside given polygon. Using this function, we analyzed land usage only in district polygon, not in whole bounding box. Then, we used [Shoelace algorithm](https://en.wikipedia.org/wiki/Shoelace_formula) to get size of each land usage type in each district and also size of whole district. Finally, we converted these values to percentages, so that we had information _how many % of land in each district was covered by different types of zones, like parks, industry, or commercial zones_.

![alt text](https://github.com/japolo95/Coursera_Capstone/blob/master/Final_Assignment/maps1_sbs.jpg "Districts polygons downloaded using OpenStreetMaps (left image) and Types of land use in Vysehrad district (right image)")
_Visualization of districts borders downloaded using OpenStreetMaps (left image), Types of land use in Vysehrad district (right image) - orange: borders of Vysehrad district, green: nature areas, gray: residential and commercial areas, black: industrial areas)_

From Foursquare, we were able to get number of venues in given rectanlge on map, however, for our purposes, we needed to limit results only to venues inside district polygon, because a district is usually not just rectangle on map, but more complicated polygon. We used  [Shapely](https://shapely.readthedocs.io/en/latest/manual.html), just the same way as in OpenStreetMaps case, to keep only venues that are inside each district's polygon. Then we converted numbers of venues to their density per square kilometer, in each district.

![alt text](https://github.com/japolo95/Coursera_Capstone/blob/master/Final_Assignment/foodstores_vinohrady.jpg "Vinohrady district polygon downloaded using OpenStreetMaps with foodstores shown in it, downloaded using Foursquare")
_Vinohrady district polygon downloaded using OpenStreetMaps with foodstores shown in it, downloaded using Foursquare_

### Clustering data and finding results
__For the second part__, I decided to cluster districts multiple times, each time by different criteria. The criteria were:
  - average number of foodstores in each district per square kilometer
  - average number of health venues (such as hospitals, pharmacies) in each district per square kilometer
  - average number of social venues that produce noise, such as bars or hotels, in each district per square kilometer.
  - average number of public transport venues, such as bus or tram stops in each district, per square kilometer.
  - percentage of each district area covered by nature like forests, parks, or public available gardens.
  - percentage of each district area covered by industrial zones
  - percentage of each district area covered by commercial zones
  - population density in each district per square kilometer

![alt text](https://github.com/japolo95/Coursera_Capstone/blob/master/Final_Assignment/final_dataset_example.jpg "Screen of final, preprocessed dataset, loaded in Pandas DataFrame. Note that nightlife, foodstores, health, and transport, shows number of corresponding venues per square kilometer in given district")
_Screen of final, preprocessed dataset, loaded in Pandas DataFrame. Note that nightlife, foodstores, health, and transport, shows number of corresponding venues per square kilometer in given district_

#### Clustering
Note that districts were not clustered by all these criteria together, instead I tried to group them by each criteria separatedly. I also tried to do clustering for multiple or all criteria together, but that brought much worse results, as there are only 112 districts in Prague.

Instead I firstly clustered by each criteria separatedly. For own clustering, I used [Agglomerative Clustering](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html), and for determining ideal number of clusters for each criteria, I used [Silhouette Score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html), where several possible numbers of clusters were tried, and the one with best Silhouette score was used (this could be different for each criteria). Results of clustering is visualized by boxplots in image below.

![alt text](https://github.com/japolo95/Coursera_Capstone/blob/master/Final_Assignment/boxplots_all.jpg "Boxplots of values distributions in each cluster for each feature")
_Boxplots of values distributions in each cluster for each feature_

Then I took best clusters of each criteria (what _best_ means is defined in [Data section](#Data) of this report and tried to find the intersection of it, in other words, tried to find districts, that are in the best cluster for all the criteria. Unfortunately (but as expected) there was no such district.

#### Findings best possible intersection between clusters
In that situation, we could simply say that also districts in _worse_ clusters are accepted, however, as some criteria are more important than other, I decided to create a function, which objective will be to find intersection of districts, that are in best possible clusters where this intersection exists, taking into account that some features are more important than others. 

For example tell the function, that criteria A is more important than criteria B. The function took that into account and accepted some worse clusters for all the criteria, while prioritizing the A criteria over B (that means, trying to first find intersection using worse clusters from B, than from A).

The function uses following formula: `f(x,t) = (min(x/10,1))*t` where _x_ is priority, _t_ is tolerance level, and function output is ratio of allowed clusters. Example: we have feature C with priority 3, and we set tolerance to 0.75. The function result is `min(3/10,1)*0.75 = 0.225 = 22.5%`. This means that for feature C we accept roughly 22.5% of clusters (sorted from best to worst). If there would be 10 clusters we would accept 2 best clusters. The `min` function in formula is used just to ensure that we don't allow more than 100% of clusters, which indeed is not possible.

I decided to set priorities (_x_ value in the formula) for each feature as following:
  1. nature area, nightlife, industry area - these affect quality of life all the time and most directly (general look of the district,  air and sound pollution)
  2. foodstores, health, transport, citiziens/km2 - these also affect quality of life, but not all the time, except the citiziens/km2, but for that it is still only required not to be extreme (like city center)
  3. commercial area - this is only nice to have, but really not critical
  
Using this function, I was able to get some reasonable result. Accepted clusters for each criteria are shown in following image.

![alt text](https://github.com/japolo95/Coursera_Capstone/blob/master/Final_Assignment/final_allowed_clusters.jpg "Percentage and numbers of clusters that we can search in for the districts. Clusters are sorted from best, to worst. 100% means that we give up using that criteria completely, as districts from all clusters are allowed")
_Percentage and numbers of clusters that we can search in for the districts. Clusters are sorted from best, to worst. 100% means that we give up using that criteria completely, as districts from all clusters are allowed_

Note that for some for criteria commercial_a (commercial area), we accepted districts from all clusters. This parameter has low priority as mentioned above. On the other hand, for criteria with high priority, such as nightlife (number of nightlife venues per square kilometer), nature_a (nature area), we searched for districts only in much fewer clusters (in other words, worse clusters were not accepted).


## Results
During whole process, several sub-results and results were generated. In this section, I will show and describe all of them.

### Data preparation

