# Online Bookstore – SQL Project

## Project Overview
This project simulates an **online bookstore database** using three datasets:  
- **Books.csv** – contains book details (title, author, genre, price, stock).  
- **Customers.csv** – stores customer information (name, email, city, country).  
- **Orders.csv** – records customer purchases (order date, quantity, total amount).  

The project demonstrates how SQL can be applied to manage relational data, perform joins, and answer real-world business questions.

---

## Project Objectives
- Create and manage relational tables for books, customers, and orders.  
- Establish **foreign key references** between tables to ensure data integrity.  
- Analyze book sales, customer activity, and inventory using SQL queries.  
- Practice **aggregate functions, GROUP BY, HAVING, and JOINs**.  
- Extract meaningful insights from structured data.

---

## Queries Solved
### Basic Queries
1. Retrieve all books in the "Fiction" genre  
2. Find books published after the year 1950  
3. List all customers from Canada  
4. Show orders placed in November 2023  
5. Retrieve the total stock of books available  
6. Find the details of the most expensive book  
7. Show all customers who ordered more than 1 quantity of a book  
8. Retrieve all orders where the total amount exceeds $20  
9. List all genres available in the Books table  
10. Find the book with the lowest stock  
11. Calculate the total revenue generated from all orders  

### Advanced Queries
1. Retrieve the total number of books sold for each genre  
2. Find the average price of books in the "Fantasy" genre  
3. List customers who have placed at least 2 orders  
4. Find the most frequently ordered book  
5. Show the top 3 most expensive books of the "Fantasy" genre  
6. Retrieve the total quantity of books sold by each author  
7. List the cities where customers who spent over $30 are located  
8. Find the customer who spent the most on orders  
9. Calculate the stock remaining after fulfilling all orders  

---

## How to Run
1. Import the CSV files (`Books.csv`, `Customers.csv`, `Orders.csv`) into PostgreSQL/MySQL.  
2. Create tables with appropriate primary and foreign keys.  
3. Run the queries from `Queries.sql` to reproduce the results.  

---

## Skills Practiced
- SQL basics (SELECT, WHERE, ORDER BY)  
- Joins (INNER JOIN, LEFT JOIN)  
- Aggregate functions (SUM, COUNT, AVG, MAX, MIN)  
- GROUP BY and HAVING clauses  
- Data integrity with foreign keys  

## Dataset Overview
This project uses three datasets (`Books.csv`, `Customers.csv`, `Orders.csv`) to simulate an online bookstore.  
Together, they represent book inventory, customer details, and purchase transactions.

---

## Books Table
| Column Name      | Description                          | Data Type   |
|------------------|--------------------------------------|-------------|
| Book_ID          | Unique identifier for each book      | Integer     |
| Title            | Title of the book                    | Text        |
| Author           | Author of the book                   | Text        |
| Genre            | Genre/category of the book           | Text        |
| Published_Year   | Year the book was published          | Integer     |
| Price            | Price of the book (USD)              | Decimal     |
| Stock            | Number of copies available in stock  | Integer     |

---

## Customers Table
| Column Name      | Description                          | Data Type   |
|------------------|--------------------------------------|-------------|
| Customer_ID      | Unique identifier for each customer  | Integer     |
| Name             | Full name of the customer            | Text        |
| Email            | Customer’s email address             | Text        |
| Phone            | Customer’s phone number              | Integer     |
| City             | City where the customer lives        | Text        |
| Country          | Country where the customer lives     | Text        |

---

## Orders Table
| Column Name      | Description                          | Data Type   |
|------------------|--------------------------------------|-------------|
| Order_ID         | Unique identifier for each order     | Integer     |
| Customer_ID      | References `Customers.Customer_ID`   | Integer (FK)|
| Book_ID          | References `Books.Book_ID`           | Integer (FK)|
| Order_Date       | Date when the order was placed       | Date        |
| Quantity         | Number of copies ordered             | Integer     |
| Total_Amount     | Total price of the order             | Decimal     |

---

## Tools Used
- **SQL (PostgreSQL/MySQL)** for queries and analysis  
- **pgAdmin / SSMS** for database management

## SQL queries
### --Table1 BOOKS
```sql
create table books(
	Book_ID int primary key,
	Title varchar(100),
	Author varchar(100),
	Genre varchar(100),
	Published_Year int,
	Price numeric(5,2),
	Stock int);
```
	
### --TABLE2 CUSTOMERS
```sql
create table customers(
	Customer_ID int primary key,
	Name varchar(100),
	Email varchar(100),
	Phone int,
	City varchar(100),
	Country varchar(100));
```

### --TABLE3 ORDERS
```sql
create table orders(
	Order_ID int primary key,
	Customer_ID int references customers(customer_id),
	Book_ID int references books(book_id),
	Order_Date date,
	Quantity int,
	Total_Amount numeric(10,2));
```
### --IMPORT DATA INTO ORDERS TABLE
```sql
copy 
orders (Order_ID,Customer_ID,Book_ID,Order_Date,Quantity,Total_Amount)
from 'C:\Users\unnati jaiswal\OneDrive\Desktop\Skill Course Advance  Excel\Excel Basic to Advance Full Course\ST - SQL ALL PRACTICE FILES-2\All Excel Practice Files\Orders.csv'
delimiter','
csv header;
```
```sql
SELECT * FROM BOOKS;
SELECT * FROM CUSTOMERS;
SELECT * FROM ORDERS;
```


## Basic queries

### -- 1) Retrieve all books in the "fiction" genre.
```sql
Select GENRE from books
WHERE GENRE = 'Fiction';
```

### -- 2) Find books published after the year 1950.
```sql
SELECT  * FROM books
WHERE published_year > 1950;
```

### -- 3) list all the customers from Canada.
```sql
SELECT customer_ID, name, email, phone, city FROM customers
WHERE country = 'Canada'; 
```
### --4) Show orders placed in November 2023
```sql
select order_ID, customer_ID, book_ID,  quantity, total_amount from orders 
where order_date between '2023-11-01' and '2023-11-30';
```
### --5) Retrieve the total stock of books available.
```sql
Select sum(stock) as total_stock 
from books; 
```
### --6) Find the details of the most expensive book.
```sql
Select * from books
order by  price desc; 
```
### --7) Show all customers who ordered more than one quantity of a book.
```sql
Select * from customers c join orders o
on c.customer_Id = o.customer_id 
where o.quantity > 2;
```

### --8) Retreive all orders where the total amount exceeds $20.
```sql
SELECT * FROM orders 
WHERE total_amount > 20;
```

### --9) List all genres available in the books table.
```sql
Select distinct genre 
from books; 
```
#### --10) Find the book with the lowest stock. 
```sql
SELECT * FROM books 
ORDER BY stock ASC;
``` 

### --11) Calculate the total revenue generated from all orders. 
```sql
Select sum(total_amount) as total_revenue 
from orders;
```

## Advance queries

### --1) Retrieve the total number of books sold for each genre.
```sql
Select b.genre,sum(o.quantity) as Quantity_sold
from books b join orders o on b.book_Id = o.book_Id
Group by genre;
``` 

### --2) Find the average price of books in the "Fantasy" genre.
```sql
Select avg(price) as AVG_price from books
where genre = 'Fantasy';
```

### --3) List the customers who have placed at least two orders.
```sql
Select c.customer_id, c.name, c.email, c.phone, c.city, c.country, count(o.order_id) 
as order_placed FROM customers c JOIN orders o ON c.customer_id = o.customer_id 
GROUP BY c.customer_id;
``` 

### --4) Find the most frequently ordered book.
```sql
SELECT b.book_id, b.title, b.author, b.genre, b.published_year, b.price, b.stock, count(o.order_id)
AS frequently_ordered
FROM books b JOIN orders o ON b.book_id = o.book_id
GROUP BY b.book_id
ORDER BY frequently_ordered desc;
``` 

### --5) The top three most expensive books of the fantasy genre.
```sql
select * from books where genre = 'Fantasy'
order by price desc
limit 3;
```

### --6) List the cities where customers who spend over $30 are located.
```sql
select c.city, sum(o.total_amount) as spending
from customers c join orders o on c.customer_id = o.customer_id
group by c.customer_id 
having sum(o.total_amount)>= 30
order by spending desc;
```

### --7) Find the customer who spent the most on orders.
```sql
SELECT c.customer_id, c.name, c.email, c.phone, c.city, c.country, sum(o.total_amount) as spending 
from customers c join orders o on c.customer_id = o.customer_id
group by c.customer_id 
order by spending desc;
```

### --8) Retrieve the total quantity of books sold by each author. 
```sql
SELECT b.author, sum(O.quantity) AS quantity_sold 
FROM books b JOIN orders o ON b.book_id = o.book_id
GROUP BY b.book_id
order by quantity_sold desc;
```

### --9) Calculate the stock remaining after fulfilling all orders. 
```sql
SELECT b.book_id, b.title, b.author, b.genre, b.published_year, b.price,
b.stock-coalesce(sum(o.quantity),0) as inventory
FROM books b left JOIN orders o ON b.book_id = o.book_id
GROUP BY b.book_id
order by inventory asc ;
```

```sql
SELECT  b.title,b.stock,coalesce(sum(o.quantity),0) as quantity_sold,
b.stock-coalesce(sum(o.quantity),0) as inventory
FROM books b left JOIN orders o ON b.book_id = o.book_id
GROUP BY b.book_id
order by stock,quantity_sold, inventory Asc;
```

## Conclusion
This mini SQL project demonstrates how relational data from an online bookstore can be analyzed using SQL queries to answer real-world business questions.  

By working with datasets for books, customers, and orders, the project highlights the importance of SQL in managing structured data, performing joins, and applying aggregate functions.  

It serves as a strong foundation for more advanced database projects, showcasing how SQL can be used not only for data retrieval but also for generating meaningful insights into sales, customer behavior, and inventory management.






   


