# Codecademy-DS-Independent-Project-
Music store analysis 

Raw data can be found [here](https://www.sqlitetutorial.net/sqlite-sample-database/)

**Basic Requirements**

1. Which tracks appeared in the most playlists? how many playlist did they appear in?
```
SELECT  A.TrackId, B.name, A.PlaylistId, count(A.PlaylistId) as "Number_of_Appearance" 
from playlist_track A 
join tracks B
on A.TrackId =B.TrackId
group by A.TrackId
order by Number_of_Appearance DESC;

2. Which track generated the most revenue? which album? which genre?

3. Which countries have the highest sales revenue? What percent of total revenue does each country make up?

4. How many customers did each employee support, what is the average revenue for each sale, and what is their total sale?
