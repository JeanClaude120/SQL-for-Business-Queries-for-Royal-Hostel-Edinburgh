# Business-Queries-for-Royal-Hostel-Edinburgh

<p align="center">
  <img 
    width="368" 
    height="443" 
    alt="Screenshot 2025-07-10 at 13 50 52" 
    src="https://github.com/user-attachments/assets/b05bb94f-9ad9-4f47-9dfb-8e695c0e4e40" />
</p>


 - Count the total number of bookings
   
```SQL
Select count(*) as Total_bookings from bookings
;
```

- Show bookings made via Hostelworld

```SQL
Select * from bookings
where booking_channel = 'Hostelworld'
;
```

- List bookings with total prive over £200

```SQL
Select booking_id, total_price
From bookings
WHERE total_price > 200
;
```

- Sort the bookings by check-in date (Soonest the first)

```SQL
Select * from bookings
Order by checkin_date ASC
;
```

- Count number of cancellations
```SQL
Select count(*) As cancellations
from bookings
where was_cancelled = 1;
```

- Average price per night per room type

```SQL
SELECT room_type, Round(AVG(price_per_night),2) As avg_price
from bookings
Group by room_type
;
```

- Count bookings by guest nationality
```SQL
Select guest_nationality, COUNT(*) AS num_bookings
From bookings
Group By guest_nationality
Order By num_bookings DESC
;
```

- Find the month with the highest total revenue
```SQL
SELECT MONTH(checkin_date) As month, Round(SUM(total_price),2) as revenue
from bookings
Where was_cancelled = 0
Group by Month(checkin_date)
Order by revenue DESC
Limit 5
;
```

- Calculate the average review score by room type

```SQL
SELECT room_type, Round(AVG(review_score), 2) AS avg_review
from bookings
Where review_score IS NOT NULL
GROUP BY room_type
;
```

- Total bookings per booking channel
```SQL
Select booking_channel, Count(*) as bookings
from bookings
Group by booking_channel
;
```

- Average length of stay for private room guests

```SQL
Select Round(AVG(nights), 2) As avg_stay
from bookings
Where room_type = 'private Room'
;
```

- Bookings during the Fringe Festival(August only)
  
```SQL
Select * from bookings
Where Month(checkin_date) = 8
;
```

- Find bookings that were made less than 7 days before check-in

```SQL
Select *, DATEDIFF(Checkin_date, booking_date) As days_in_advance
from bookings
Where DATEDIFF(checkin_date, booking_date) < 7
;
```

- Top 5 nationalities by average total spend
  
```SQL
select guest_nationality, Round(AVG(total_price), 2) As avg_spend
from bookings
Where was_cancelled = 0
Group by guest_nationality
ORDER BY avg_spend DESC
LIMIT 5
;
```

- Cumulative monthly revenue over time
  
```SQL
Select
DATE_FORMAT(checkin_date, '%Y-%m') as month,
SUM(total_price) as montly_revenue,
SUM(SUM(total_price)) OVER (ORDER BY DATE_FORMAT(checkin_date,'%Y-%m')) As Cumulative_revenue
from bookings
Where was_cancelled = 0
Group by month
;
```

- Categorize bookings as "Short stay", "Medium stay", "Longstay"

```SQL
Select booking_id, nights,
CASE
WHEN nights <= 2 Then 'Short stay'
When nights BETWEEN 3 AND 5 THEN 'Medium Stay'
Else 'Long Stay'
END AS stay_category
From bookings
;
```

- Detect peak booking weeks(More than 10 bookings in a 7-day

```SQL
Select checkin_date, COUNT(*) AS bookings
from bookings
Group by checkin_date
Having bookings > 2
;
```

- Most profitable day based on the check-in date

```SQL
Select checkin_date, SUM(total_price) AS revenue
from bookings
where was_cancelled = 0
Group by checkin_date
Order by revenue DESC
LIMIT 5
;
```

- Guests who paid above average total_price

```SQL
Select * from bookings
where total_price > (
Select AVG(total_price) from bookings
Where was_cancelled = 0)
;
```
