# Music-Store-Data-Analysis
SQL PROJECT- MUSIC STORE DATA ANALYSIS 

                                QUESTION SET 1 
                          
1. Who is the senior most employee based on job title? 
2. Which countries have the most Invoices? 
3. What are top 3 values of total invoice? 
4. Which city has the best customers? We would like to throw a promotional Music 
Festival in the city we made the most money. Write a query that returns one city that 
has the highest sum of invoice totals. Return both the city name & sum of all invoice 
totals 
5. Who is the best customer? The customer who has spent the most money will be 
declared the best customer. Write a query that returns the person who has spent the 
most money

                                 QUESTION SET 2 

1. Write query to return the email, first name, last name, & Genre of all Rock Music 
listeners. Return your list ordered alphabetically by email starting with A 
2. Let's invite the artists who have written the most rock music in our dataset. Write a 
query that returns the Artist name and total track count of the top 10 rock bands 
3. Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the 
longest songs listed first

                                  QUESTION SET 3
   
1. Find how much amount spent by each customer on artists? Write a query to return 
customer name, artist name and total spent 
2. We want to find out the most popular music Genre for each country. We determine the 
most popular genre as the genre with the highest amount of purchases. Write a query 
that returns each country along with the top Genre. For countries where the maximum 
number of purchases is shared return all Genres 
3. Write a query that determines the customer that has spent the most on music for each 
country. Write a query that returns the country along with the top customer and how 
much they spent. For countries where the top amount spent is shared, provide all 
customers who spent this amount



# 🎵 *Music Store Data Analysis* - SQL Insights  

## 🏅 *Senior Most Employee*  
🔹 The most senior employee based on job title is retrieved using:  
```sql
SELECT * FROM employee  
ORDER BY levels DESC  
LIMIT 1;
```
🌍 Countries with the Most Invoices
🔹 The top countries with the highest invoice counts:

```sql
SELECT COUNT(*) AS c, billing_country  
FROM invoice  
GROUP BY billing_country  
ORDER BY c DESC;
```
💰 Top 3 Invoice Values
🔹 The highest invoice amounts in the dataset:

```sql
SELECT total  
FROM invoice  
ORDER BY total DESC  
LIMIT 3;
```
🎉 Best City for a Music Festival
🔹 The city with the highest total invoice amount is:

```sql
SELECT billing_city, SUM(total) AS InvoiceTotal  
FROM invoice  
GROUP BY billing_city  
ORDER BY InvoiceTotal DESC  
LIMIT 1;
```
🏆 Best Customer
🔹 The customer who has spent the most money:

```sql
SELECT customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) AS total  
FROM customer  
JOIN invoice ON customer.customer_id = invoice.customer_id  
GROUP BY customer.customer_id  
ORDER BY total DESC  
LIMIT 1;
```
🎸 Rock Music Fans & Artists
🎵 Rock Music Listeners
🔹 List of emails and names of customers who listen to Rock music:

```sql
SELECT email, first_name, last_name, genre.name  
FROM customer  
JOIN invoice ON invoice.customer_id = customer.customer_id  
JOIN invoice_line ON invoice_line.invoice_id = invoice.invoice_id  
JOIN track ON track.track_id = invoice_line.track_id  
JOIN genre ON genre.genre_id = track.genre_id  
WHERE genre.name LIKE 'Rock'  
ORDER BY email;
```
🎤 Top 10 Rock Artists
🔹 The top 10 artists with the most Rock tracks:

```sql
SELECT artist.artist_id, artist.name, COUNT(artist.artist_id) AS number_of_songs  
FROM track  
JOIN album ON album.album_id = track.album_id  
JOIN artist ON artist.artist_id = album.artist_id  
JOIN genre ON genre.genre_id = track.genre_id  
WHERE genre.name LIKE 'Rock'  
GROUP BY artist.artist_id  
ORDER BY number_of_songs DESC  
LIMIT 10;
```
⏳ Longest Songs
🔹 Tracks that are longer than the average song length:

```sql
SELECT name, milliseconds  
FROM track  
WHERE milliseconds > (  
    SELECT AVG(milliseconds) FROM track  
)  
ORDER BY milliseconds DESC;
```
🎧 Advanced Music Store Insights
💵 Customer Spending on Artists
🔹 How much each customer spent on the top-selling artist:

```sql
WITH best_selling_artist AS (  
    SELECT artist.artist_id, artist.name, SUM(invoice_line.unit_price * invoice_line.quantity) AS total_sales  
    FROM invoice_line  
    JOIN track ON track.track_id = invoice_line.track_id  
    JOIN album ON album.album_id = track.album_id  
    JOIN artist ON artist.artist_id = album.artist_id  
    GROUP BY artist.artist_id  
    ORDER BY total_sales DESC  
    LIMIT 1  
)  
SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name,  
       SUM(il.unit_price * il.quantity) AS amount_spent  
FROM invoice i  
JOIN customer c ON c.customer_id = i.customer_id  
JOIN invoice_line il ON il.invoice_id = i.invoice_id  
JOIN track t ON t.track_id = il.track_id  
JOIN album alb ON alb.album_id = t.album_id  
JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id  
GROUP BY c.customer_id, c.first_name, c.last_name, bsa.artist_name  
ORDER BY amount_spent DESC;
```
🌎 Most Popular Genre by Country
🔹 The most purchased music genre in each country:

```sql
WITH popular_genre AS (  
    SELECT COUNT(invoice_line.quantity) AS purchases, customer.country, genre.name, genre.genre_id,  
           ROW_NUMBER() OVER (PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity) DESC) AS RowNo  
    FROM invoice_line  
    JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id  
    JOIN customer ON customer.customer_id = invoice.customer_id  
    JOIN track ON track.track_id = invoice_line.track_id  
    JOIN genre ON genre.genre_id = track.genre_id  
    GROUP BY customer.country, genre.name, genre.genre_id  
)  
SELECT * FROM popular_genre WHERE RowNo = 1;
```
🏅 Top Customer in Each Country
🔹 The customer who has spent the most money in each country:

```sql
WITH customer_with_country AS (  
    SELECT customer.customer_id, first_name, last_name, billing_country,  
           SUM(total) AS total_spending,  
           ROW_NUMBER() OVER (PARTITION BY billing_country ORDER BY SUM(total) DESC) AS RowNo  
    FROM invoice  
    JOIN customer ON customer.customer_id = invoice.customer_id  
    GROUP BY customer.customer_id, first_name, last_name, billing_country  
)  
SELECT * FROM customer_with_country WHERE RowNo = 1;
```
🚀 This SQL analysis provides valuable insights into customer behavior, sales trends, and artist popularity in the music store dataset!










