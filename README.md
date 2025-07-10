# Business-Queries-for-Royal-Hostel-Edinburgh

(1) Count the total number of bookings
Select count(*) as Total_bookings from bookings
;
(2) Show bookings made via Hostelworld
Select * from bookings
where booking_channel = 'Hostelworld'
;
(3) List bookings with total prive over £200
Select booking_id, total_price
From bookings
WHERE total_price > 200
;
(4) Sort the bookings by check-in date (Soonest the first)
Select * from bookings
Order by checkin_date ASC
;
(5) Count number of cancellations
Select count(*) As cancellations
from bookings
where was_cancelled = 1;

(6) Average price per night per room type
SELECT room_type, Round(AVG(price_per_night),2) As avg_price
from bookings
Group by room_type
;
(7) Count bookings by guest nationality
Select guest_nationality, COUNT(*) AS num_bookings
From bookings
Group By guest_nationality
Order By num_bookings DESC
;
(8) Find the month with the highest total revenue
SELECT MONTH(checkin_date) As month, Round(SUM(total_price),2) as revenue
from bookings
Where was_cancelled = 0
Group by Month(checkin_date)
Order by revenue DESC
Limit 5
;
(9) Calculate average review score by room type
SELECT room_type, Round(AVG(review_score), 2) AS avg_review
from bookings
Where review_score IS NOT NULL
GROUP BY room_type
;
(10) Total bookings per booking channel
Select booking_channel, Count(*) as bookings
from bookings
Group by booking_channel
;
(11) Average lenght of stay for private room guests
Select Round(AVG(nights), 2) As avg_stay
from bookings
Where room_type = 'private Room'
;
(12) Bookings during the Fringe Festival(August only)
Select * from bookings
Where Month(checkin_date) = 8
;
(12) Find bookings that were made less than 7 days before check-in
Select *, DATEDIFF(Checkin_date, booking_date) As days_in_advance
from bookings
Where DATEDIFF(checkin_date, booking_date) < 7
;
(12) Top 5 nationalities by average total spend
select guest_nationality, Round(AVG(total_price), 2) As avg_spend
from bookings
Where was_cancelled = 0
Group by guest_nationality
ORDER BY avg_spend DESC
LIMIT 5
;
(13) Cumulative monthly revenue over time
Select
DATE_FORMAT(checkin_date, '%Y-%m') as month,
SUM(total_price) as montly_revenue,
SUM(SUM(total_price)) OVER (ORDER BY DATE_FORMAT(checkin_date,'%Y-%m')) As Cumulative_revenue
from bookings
Where was_cancelled = 0
Group by month
;
(14) Categorize bookings as "Short stay", "Medium stay", "Longstay"
Select booking_id, nights,
CASE
WHEN nights <= 2 Then 'Short stay'
When nights BETWEEN 3 AND 5 THEN 'Medium Stay'
Else 'Long Stay'
END AS stay_category
From bookings
;
(15) Detect peak booking weeks(More than 10 bookings in a 7-day
Select checkin_date, COUNT(*) AS bookings
from bookings
Group by checkin_date
Having bookings > 2
;
(16) Most profitable day based on the check-in date
Select checkin_date, SUM(total_price) AS revenue
from bookings
where was_cancelled = 0
Group by checkin_date
Order by revenue DESC
LIMIT 5
;
(17) Guests who paid above average total_price
Select * from bookings
where total_price > (
Select AVG(total_price) from bookings
Where was_cancelled = 0)
;
