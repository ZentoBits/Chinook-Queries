1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.
  ```
  SELECT Customer.FirstName || ' ' || Customer.LastName AS 'Name', Customer.CustomerId, Customer.Country
  FROM Customer
  WHERE Customer.Country != 'USA'
  ```

2. Provide a query only showing the Customers from Brazil.
  ```
  SELECT *
  FROM Customer
  WHERE Customer.Country = 'Brazil'
  ```

3. Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.
  ```
  SELECT Customer.FirstName ||" "|| Customer.LastName AS Name, Invoice.CustomerID, Invoice.InvoiceDate, Invoice.BillingCountry
  FROM Invoice
  JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
  WHERE Invoice.BillingCountry = "Brazil"
  ```

4. Provide a query showing only the Employees who are Sales Agents.
  ```
  SELECT FirstName ||" " || LastName AS Name, Title
  FROM Employee
  WHERE Employee.Title = "Sales Support Agent"
  ```

5. Provide a query showing a unique list of billing countries from the Invoice table.
  ```
  SELECT BillingCountry
  FROM Invoice
  GROUP BY Invoice.BillingCountry
  ```

6. Provide a query showing the invoices of customers who are from Brazil.
  ```
  SELECT *
  FROM Invoice
  JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
  WHERE Invoice.BillingCountry = "Brazil"
  ```

7. Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.
  ```
  SELECT Employee.FirstName ||" "||Employee.LastName AS "Name" , Employee.EmployeeId, Invoice.*
  FROM Employee
  JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
  JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
  ORDER BY Employee.EmployeeId
  ```

8. Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.
  ```
  SELECT Customer.FirstName || " " || Customer.LastName, Invoice.Total, Customer.Country, Employee.FirstName || " " || Employee.LastName AS "Sales Agent"
  FROM Invoice
  JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
  JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId;
  ```

9. How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?
  ```
  SELECT COUNT(*) AS 'Total Invoices'
  FROM Invoice
  WHERE Invoice.InvoiceDate
  LIKE '%2011%' OR Invoice.InvoiceDate LIKE '%2009%'

  SELECT COUNT(Invoice.Total) AS '2009 Total'
  FROM Invoice
  WHERE Invoice.InvoiceDate LIKE '%2009%'

  SELECT COUNT(Invoice.Total) AS '2011 Total'
  FROM Invoice
  WHERE Invoice.InvoiceDate LIKE '%2011%'
  ```

10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.
  ```
  SELECT COUNT(*) AS 'Line Items ID 37'
  FROM InvoiceLine
  WHERE InvoiceLine.InvoiceId = '37'
  ```

11. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY
  ```
  SELECT InvoiceLine.InvoiceId, COUNT(*) 'Line Items / Invoice'
  FROM InvoiceLine
  GROUP BY InvoiceLine.InvoiceId
  ```

12. Provide a query that includes the track name with each invoice line item.
  ```
  SELECT Track.Name, InvoiceLine.InvoiceLineId
  FROM InvoiceLine
  JOIN Track ON InvoiceLine.TrackId = Track.TrackId
  ```

13. Provide a query that includes the purchased track name AND artist name with each invoice line item.
  ```
  SELECT Track.Name, Artist.Name, InvoiceLine.InvoiceLineId
  FROM InvoiceLine
  JOIN Track ON InvoiceLine.TrackId = Track.TrackId
  JOIN Album ON Track.AlbumId = Album.AlbumId
  JOIN Artist ON Album.ArtistId = Artist.ArtistId
  ```

14. Provide a query that shows the # of invoices per country. HINT: GROUP BY
  ```
  SELECT Customer.Country, COUNT(Invoice.InvoiceId)
  FROM Customer
  JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
  GROUP BY Customer.Country
  ```

15. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resultant table.
  ```
  SELECT COUNT(*) AS 'Total Tracks', Playlist.Name
  FROM PlaylistTrack
  JOIN Playlist ON PlaylistTrack.PlaylistId = Playlist.PlaylistId
  GROUP BY PlaylistTrack.PlaylistId
  ```

16. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.
  ```
  SELECT Track.Name, Album.Title, MediaType.Name, Genre.Name
  FROM Track
  JOIN Album ON Track.AlbumId = Album.AlbumId
  JOIN MediaType ON Track.MediaTypeId = MediaType.MediaTypeId
  JOIN Genre ON Track.GenreId = Genre.GenreId
  ```

17. Provide a query that shows all Invoices but includes the # of invoice line items.
  ```
  SELECT *, COUNT(InvoiceLine.InvoiceLineId) AS 'Invoice Lines'
  FROM Invoice
  JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
  GROUP BY Invoice.InvoiceId
  ```

18. Provide a query that shows total sales made by each sales agent.
  ```
  SELECT Employee.FirstName, SUM(Invoice.Total) AS 'Total Sales'
  FROM Employee
  JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
  JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
  WHERE Employee.Title = 'Sales Support Agent'
  GROUP BY Employee.EmployeeId
  ```

19. Which sales agent made the most in sales in 2009?
  ```
  SELECT Employee.FirstName, SUM(Invoice.Total) AS 'Total'
  FROM Employee
  JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
  JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
  WHERE Invoice.InvoiceDate LIKE '%2009%'
  GROUP BY Employee.EmployeeId
  ORDER BY TOTAL DESC LIMIT 1
  ```

20. Which sales agent made the most in sales in 2010?
  ```
  SELECT Employee.FirstName, SUM(Invoice.Total) AS 'Total'
  FROM Employee
  JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
  JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
  WHERE Invoice.InvoiceDate LIKE '%2010%'
  GROUP BY Employee.EmployeeId
  ORDER BY TOTAL DESC LIMIT 1
  ```

21. Which sales agent made the most in sales over all?
  ```
  SELECT Employee.FirstName, SUM(Invoice.Total) AS 'Total'
  FROM Employee
  JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
  JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
  GROUP BY Employee.EmployeeId
  ORDER BY TOTAL DESC LIMIT 1
  ```

22. Provide a query that shows the # of customers assigned to each sales agent.
  ```
  SELECT COUNT(Customer.CustomerId) AS 'Customers Assigned', Employee.FirstName || ' ' || Employee.LastName AS 'Name'
  FROM Employee
  JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
  GROUP BY Employee.EmployeeId
  ```

23. Provide a query that shows the total sales per country. Which country's customers spent the most?
  ```
  SELECT Customer.Country, SUM(Invoice.Total) AS 'Total Sales'
  FROM Customer
  JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
  GROUP BY Customer.Country
  ```

24. Provide a query that shows the most purchased track of 2013.
  ```
  SELECT Track.Name, SUM(Invoice.Total) AS 'Total Sales'
  FROM Track
  JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId
  JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
  WHERE Invoice.InvoiceDate LIKE '%2013%'
  GROUP BY Track.TrackId
  ORDER BY TOTAL DESC LIMIT 1
  ```

25. Provide a query that shows the top 5 most purchased tracks over all.
  ```
  SELECT Track.Name, SUM(Invoice.Total) AS 'Total Sales'
  FROM Track
  JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId
  JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
  GROUP BY Track.TrackId
  ORDER BY TOTAL DESC LIMIT 5
  ```

26. Provide a query that shows the top 3 best selling artists.
  ```
  SELECT Artist.Name, SUM(Invoice.Total) AS 'Total'
  FROM Artist
  JOIN Album ON Artist.ArtistId = Album.ArtistId
  JOIN Track ON Album.AlbumId = Track.AlbumId
  JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId
  JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
  GROUP BY Artist.Name
  ORDER BY TOTAL DESC LIMIT 3
  ```

27. Provide a query that shows the most purchased Media Type.
  ```
  SELECT MediaType.Name, SUM(Invoice.Total) AS 'Total'
  FROM MediaType
  JOIN Track ON MediaType.MediaTypeId = Track.MediaTypeId
  JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId
  JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
  GROUP BY MediaType.Name
  ORDER BY TOTAL DESC LIMIT 1
  ```
