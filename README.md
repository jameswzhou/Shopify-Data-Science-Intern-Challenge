# Shopify-Data-Science-Intern-Challenge
### Challenge submission for Shopify's Data Science Intern - Summer 2022 Position

---

**Question 1:**  
a) By quickly scanning through the "order_amount" column in the dataset, it can quickly be seen that the data contains extreme values or outliers (such as 704000 on row 17). These extremely high values causes the average (or mean) to be skewed upward. When evaluating the data, a metric should be chosen that is not heavily affected by these outliers.

b) A better metric for this dataset would be the median, which is less affected by the extreme values.

c) The median for "order_amount" is $284, which is a more reasonable value for the average order value (AOV) of sneakers.

**Question 2:**  
a) 192

> SELECT Count(OrderID) FROM Orders  
JOIN Shippers  
WHERE ShipperName = "Speedy Express" 

'Orders' contains the full list of orders, thus we can count the primary key 'OrderID' to determine the number of orders. Shippers can be joined (both contain ShipperID) to filter the ones where the 'ShipperName' is "Speedy Express". 

b) Peacock

> SELECT LastName, OrderCount FROM Employees  
JOIN (SELECT EmployeeID, Count(OrderID) as OrderCount FROM Orders  
GROUP BY EmployeeID  
ORDER BY Count(OrderID) DESC) AS o  
ON Employees.EmployeeID = o.EmployeeID  

The inner query takes 'Orders' and returns a table containing the number of orders that each Employee has made (hence the 'GROUP BY EmployeeID') in descending order. This table is then joined with the 'Employees' table on the 'EmployeeID' property to obtain the 'LastName' of the employees. Since is it sorted in descending order, the top row of the result contains the last name of the employee with the most orders.  

c) Boston Crab Meat

> SELECT ProductName, Ordered FROM Products   
JOIN (SELECT ProductID, SUM(Quantity) as Ordered FROM OrderDetails  
WHERE OrderID IN (SELECT OrderID FROM Orders  
WHERE CustomerID IN (SELECT CustomerID FROM Customers  
WHERE Country = "Germany"))  
GROUP BY ProductID  
ORDER BY Ordered DESC) AS p  
ON Products.ProductID = p.ProductID  

The innermost query obtains every 'CustomerID' belonging to 'Customers' in "Germany". The query outside that takes the 'Orders' and finds the ones with 'CustomerID' inside the result of the previous query, so we obtain every 'OrderID' where the customer is in Germany. We can then take the 'OrderDetails' table find the entries with matching 'OrderID' to determine the total number of each product ordered by customers in Germany (hence the 'GROUP BY ProductID') in descending order. Finally, we can join this with the 'Products' table to determine the name (or any other information we would like) of the product and the total number ordered by customers in Germany. Looking at the top entry (since the previous result was ordered in descending order), we can find the product that was ordered the most by customers in Germany. 
