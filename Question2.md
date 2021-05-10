# Fall 2021 Data Science Intern Challenge

### Question 2

Q2(a)

Answer: 54
- SELECT COUNT(DISTINCT(OrderID)) FROM Orders WHERE ShipperID = (SELECT ShipperID FROM Shippers WHERE ShipperName = 'Speedy Express');

Q2(b)

Answer: Peacock
- SELECT e.LastName FROM Orders o, Employees e WHERE o.EmployeeID == e.EmployeeID GROUP BY o.EmployeeID ORDER BY COUNT(DISTINCT(OrderID)) DESC LIMIT 1;

Q2(c)

Answer: Boston Crab Meat
- SELECT p.ProductName FROM OrderDetails o, Products p WHERE o.OrderID IN (SELECT DISTINCT(OrderID) FROM Orders WHERE CustomerID IN (SELECT CustomerID FROM Customers WHERE Country = 'Germany')) AND p.ProductID = o.ProductID GROUP BY o.ProductID ORDER BY SUM(o.Quantity) DESC LIMIT 1;