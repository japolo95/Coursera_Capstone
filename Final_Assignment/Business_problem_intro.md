# Apartment house for seniors in Prague
Prague is city in Czech republic (country in central Europe). Like all other countries in Europe, average age in population is slowly increasing over decades. This continuous change creates a requirement for more services for elderly people, such as pharmacies, hospitals, parks, and so on. This also includes a problem with housing and living. Many apartments are in buildings without elevators, sometimes there are no close stores, or even no public transport stations.

Fortunately, there are projects, companies, and people, who want to offer decent living posibilities for seniors. There are many retirement homes and apartment houses for elderly people in the country, and they make their living much easier, because they are built with respect to needs of these people. This means there is elevator, enough nature around, it's not located in busy parts of cities, and so on.

Let's say there is fictional real estate development company which wants to build a new apartment house in Prague, which will be designed especially for elderly people. __In this project, I will analyze different parts of Prague, using Foursquare API, information from Wikipedia, and data from OpenStreetMaps, to find out potentionally good places for building new apartment house for seniors__. I will use different criteria, which I consider important.

_Please take into account, that this project is simplified and inlcudes only some basic criteria, thus it does not reflect whole reality._

## Usage of data in planning
For our project, I decided to set several criteria, which may be important for analysis of different district in Prague. But before that, we need to decide how to determine what is _part of Prague_. 

### Dividing city into different neighborhoods / districts

On Wikipedia (https://en.wikipedia.org/wiki/Districts_of_Prague) we can find list of districts in Prague. From what we can see, Prague can be split into different districts or neighborhoods in three different ways:
 - Administrative districts: Prague is divided into 22 parts administrative parts which are named very simply: Prague 1, Prague 2, Prague 3,... ,Prague 22. This type of division is sometimes only formal, because it includes huge areas, so it's not very precise.
 - Municipal districts: Prague can be divided also to 57 parts, which is more precise.
 - Cadastral areas: There are 111 cadastral areas in Prague, which is even more precise, and best fits the purpose of my analysis.
 
For our purpose, it is best to use Cadastral areas, as they are smaller (thus more precise) than Administrative or Municipal districts.

### Selecting our criteria
As our criteria I decided to ask following questions, about each district:

 - How much is the district good for relaxation? Usually elderly people prefers calm and clean environment
     - What is the size of nature areas (forests, parks,...) available to public in the district? (available on OpenStreetMaps)
     - How many clubs, bars and hotels are in the district? These places usually attract people who wants to enjoy their free time, so there could be problems with noise and some conflicts in streets, parks, and so on. This data is also available on Foursquare.
     - Also what is the population density - more people means usually more noise. This is available on Wikipedia
     - Sometimes there can be lot of industrial areas that may be also noisy. This is available in OpenStreetMaps
 - How much is the district suitable in terms of public services? For elderly people it's especially important to have everything they need in their district.
     - How many hospitals, pharmacies, and clinics are in the district? (available on Foursquare)
     - How many stores and supermarkets are in the district? (available on Foursquare)
     - Is there any public transport (tram or subway) station? (available on Foursquare)
     
There is more criteria which could be considered, but for purpose of our project, these should be enough.

## How the data will be used and processed
For figuring out solution for this problem, we will do multiple clustering of districts according to different criteria. This will show us how districts can be grouped according each of criteria. Each of this criteria can be quantified and it can be told, whether higher values of that criteria are better or worse for purpose of the project. Then we will try to find intersection of districts, that were grouped into best, or at least not worst (for criteria that are less important) clusters according to each criteria.
