# Codecademy-DS-Independent-Project-
Music store analysis 

Raw data can be found [here](https://www.sqlitetutorial.net/sqlite-sample-database/)

# **Basic Requirements**

## 1. Which tracks appeared in the most playlists? how many playlist did they appear in?
```
select A.TrackId, B.name, A.PlaylistId, count(A.PlaylistId) as "Number_of_Appearance" 
from playlist_track A 
join tracks B
on A.TrackId =B.TrackId
group by A.TrackId
order by Number_of_Appearance desc;
```

## 2. Which track generated the most revenue? which album? which genre?
 ```
select I.TrackId, T.name as Track_Name, A.Title as Album_Name, G.name as Genre, sum(I.UnitPrice * I.Quantity) as Revenue 
from invoice_items I
join tracks T
on I.TrackId = T.TrackId
join albums A 
on T.AlbumId = A.AlbumId
join genres G
on T.GenreId = G.GenreId
group by I.TrackId
order by Revenue desc; 
```

## 3. Which countries have the highest sales revenue? What percent of total revenue does each country make up?
```
Select T.Country, T.Revenue, Round((T.Revenue/(select sum(invoice_items.Quantity * invoice_items.UnitPrice) from invoice_items))*100, 2) as Percentage
from 
(select invoices.BillingCountry as Country,Round(sum(invoice_items.Quantity * invoice_items.UnitPrice),2) as Revenue
from invoices
join invoice_items 
on invoices.InvoiceId =invoice_items.InvoiceId
group by invoices.BillingCountry) T, invoice_items
group by T.Country
order by Percentage desc; 
```

## 4. How many customers did each employee support, what is the average revenue for each sale, and what is their total sale?
```
select employees.EmployeeId, employees.FirstName, employees.LastName, ifnull(count(distinct customers.CustomerId),0) as 'Number_of_customer_supported', 
round((sum(invoices.Total)/ count(distinct customers.CustomerId)),2) as Average_sales_per_employee,
round(sum(invoices.Total),2) as Total_Revenue_per_employee
from employees
join customers on employees.EmployeeId =customers.SupportRepId 
join invoices on customers.CustomerId =invoices.CustomerId
group by employees.EmployeeId
order by Total_Revenue_per_employee desc; 

```

# **Intermediate Challenge**

## 5. Do longer or shorter length albums tend to generate more revenue? 
```
with albumn_length as (select sum(tracks.Milliseconds)/1000/60 as Length, tracks.AlbumId from tracks group by tracks.AlbumId), 
revenue as (select tracks.AlbumId, sum(invoice_items.Quantity * invoice_items.UnitPrice) as Revenue
from tracks 
join invoice_items on tracks.TrackId =invoice_items.TrackId
group by tracks.AlbumId) 

select albumn_length.AlbumId, albums.Title, albumn_length.Length as Length_in_minutes, revenue.Revenue 
from albumn_length
join revenue on albumn_length.AlbumId =revenue.AlbumId
join albums on albumn_length.AlbumId = albums.AlbumId
group by albumn_length.AlbumId
order by Revenue desc; 
```

## 6. Is the number of times a track appear in any playlist a good indicator of sales?

# **Advanced Challenge** 

## 7. How much revenue is generated each year, and what is its percent change 31 from the previous year?
