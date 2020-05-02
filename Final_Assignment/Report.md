# Apartment house for seniors in Prague
This is a project for Coursera Applied Data Science Capstone course. It show's my personal ideas about problem that I selected to try to solve. The file contains several parts:
  - [Introduction/Business Problem](#intro)
  - [Data](#Data)

<h2 id="intro">Introduction/Business Problem </h2>
Problem that I am trying to solve, is to determine which districts of given city (I selected Prague, in CZ) are most suitable for real estate development copmany, that wants to plan and build apartment house for seniors.

The answer to question how this can be interesting for such company is obvious: Average age in local population is increasing, and also wealth of individuals. Especially elderly people have more money to spend and they are very reliable target group. If real estate development company sells or rent new apartments to seniors, it is clear that they will be very good renters, especially in compare with students or other young people - these did not have time to cumulate money during their lifes, and their lifes are more unstable in general.

However, the problem also is, that Prague is capital city of Czech Republic and it has many districts. Some of them are more noisy than others, some of them are purely industrial, some of them does not have any facilites such as foodstores, and so on.

For figuring out solution for this problem, we will do multiple clustering of districts according to different criteria. This will show us how districts can be grouped according each of criteria. Each of this criteria can be quantified and it can be told, whether higher values of that criteria are better or worse for purpose of the project. Then we will try to find intersection of districts, that were grouped into best, or at least not worst (for criteria that are less important) clusters according to each criteria.

__Foursquare__ data are especially important in this case, because they can help us to cluster city districts, according to numbers or density of different types of venues. If we want to cluster districts according to multiple different parameters, we must know which parameters are important for us. In our case, Foursquare can provide us data for parameters such as number of stores, pharmacies, or accessibility of district by public transport.

## Data
Recall that purpose of the project is to determine which districts in the city (Prague) are most suitable for real estate development company, that wants to build apartment house for seniors, and then sell or rent individual apartments. Because of this fact, we will be interested in several parameters, that may be interesting for target clients/buyers/renters (elderly people/seniors).
