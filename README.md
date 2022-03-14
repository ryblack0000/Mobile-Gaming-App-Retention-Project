# Mobile Gaming App Retention

In order to explore 30-day rolling retention for players of a mobile gaming app, this project involved writing complex SQL queries and the creation of additional columns in the data. Findings were presented visually to answer questions on subjects such as in-app spending, and customer segmentation by demographic (region and age). Google BigQuery was used in combination with Google Sheets.

*To view SQL queries used for this project, see '[SQL Queries.md](https://github.com/ryblack0000/Mobile-Gaming-App-Retention-Project/blob/main/SQL_queries.md)' file within this repository.

### Created by Ryan Blackadar and Brendan Anderson: Juno College of Technology, October 2021

## Project Objective
Investigate retention for player's of a mobile app by using a relational database and writing queries to aggregate data for the purposes of reporting and visualization.

## Points of investigation
* Retention rate (30-day rolling)
* Age of players
* Location of players
* Individual spend within app

## Tables Created
* Retention Rate by Day of the Year *(Appendix 1 in queries)*
* Retention Status by Member *(Appendix 2 in queries)*

## Business Questions & Exploratory Analysis
### 1. What percentage of the game's players showed retention by continuing to play the game more than 30 days after they day they joined?
Of the 40,452 players who joined the game over the span of the year, 27,687 of those players were considered retained (68.44%). *(Appendix 3 in queries)*

### 2. What is the fractional retention throughout the year and does this vary at specific times of the year?
For this analysis, we omitted the last 3 days of the data set because the retention rate could not be accurately determined and thus skewed the analysis. By investigating the fractional retention for each of the days of the year, we can see an *very slight* downward trend toward the end of the year. In general, there is no seasonality to be observed by retention rates. They range from a low of 56% to a high of 80% and they average 68% overall. *(Appendix 1 in queries)*
>![Retention Rates Over the Year](https://user-images.githubusercontent.com/90063554/139563305-e1b9a5bd-0e7e-4c7a-8129-4005d51535e9.png)

### 3. How does retention vary by region?
*(Appendix 4 in queries)*
>![Retention Rate by Region](https://user-images.githubusercontent.com/90063554/139563596-98834d6e-a810-4447-a7aa-9d03ca4113a6.png)

### 4. What are the most significant age groupings of the players of this game?
By dividing the age groupings of the players into 4-year (inclusive) segments, we found that the players between the ages of 16-23 represent the largest of players at 81% of the entire player base. *(Appendix 5 in queries)*
>![Age Groupings by Count of Players](https://user-images.githubusercontent.com/90063554/139592147-a8dc70f4-4366-4bc3-8295-e25392f4ff6e.png)

### 5. What regions are the players based in and how many players are in each of these regions?
By segmenting the player base by geographical region, we can see that the largest portion of the user base is in North America, while Asia has the lesser of the 6 regions. *(Appendix 6 in queries)*
>![Count of Players by Region](https://user-images.githubusercontent.com/90063554/139592466-052e169c-ba04-4ec0-8f99-013336cd500d.png)

### 6. What is the total spend within the app by the players in each of the regions?
By segmenting the player base by geographical region and investigating the spending within the app in those regions, we can see that North America has the highest spending levels with revenues approximately $400,000 higher than in Asia. *(Appendix 7 in queries)*
>![Users Total Spend by Region](https://user-images.githubusercontent.com/90063554/139592734-ec5a2ba4-e798-4a3f-a992-d228fc532872.png)
 
### 7. What is the age distribution, and what is the most profitable age segmentation playing this game? What about also taking into consideration the region the player is from?
We found that 20 is the age group with the most players (5911), with age following a normal distribution as shown in the graph.
>![Count of Players](https://user-images.githubusercontent.com/90063554/139684940-55a836b2-b558-4f70-9045-af05bbf804a1.png)

Predictably, the total amount spent per age category follows a similar distribution, with 20 again being the age with the most amount of money spent ($8,382,685.06).
>![In-App Spending by Age](https://user-images.githubusercontent.com/90063554/139685078-d92545f1-fa7c-4b55-bbe3-a11710e44a28.png)

We wanted to then determine which age category is the most profitable by determining the average amount of money that each players spends for each age category. Interestingly, we found that players in the lowest and highest age categories spent more on average than those in the middle.
>![In-App Spending _ Count of Players Each Age](https://user-images.githubusercontent.com/90063554/139684495-f917e888-36bf-436b-a631-4bf47191e295.png)

We also wanted to determine the average profitably of all players by each region. To do this, we took the total revenue from each region and divided it by the number of players in that region. We discovered that players in Europe were the most profitable ($1,411 avg spend/person), while players in Africa were the least profitable ($1,391 avg spend/person).

>![Player Count by Region](https://user-images.githubusercontent.com/90063554/139684540-45d69243-3b52-4525-81e0-096144d7e763.png)

>![Regional Spending _ Count of Players by Region](https://user-images.githubusercontent.com/90063554/139684584-a6e61808-1864-4795-8ef0-3281f520720b.png)

*(Appendix 8 in queries)*
