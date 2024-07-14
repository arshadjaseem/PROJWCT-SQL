# PROJWCT-SQL
-- Create the database
CREATE DATABASE library;

-- Use the library database
USE library;

-- Create Branch table
CREATE TABLE Branch (Branch_no INT PRIMARY KEY,
    Manager_Id INT,
    Branch_address VARCHAR(300),
    Contact_no VARCHAR(15));
    SELECT * FROM Branch;
    
    
-- Create Employee table
CREATE TABLE Employee (Emp_Id INT PRIMARY KEY,
    Emp_name VARCHAR(100),
    Position VARCHAR(100),
    Salary DECIMAL(10, 2),
    Branch_no INT,
    FOREIGN KEY (Branch_no) REFERENCES Branch(Branch_no));

SELECT * FROM Employee;


-- Create Books table
CREATE TABLE Books (ISBN INT PRIMARY KEY,
    Book_title VARCHAR(255),
    Category VARCHAR(100),
    Rental_Price DECIMAL(10, 2),
    Status ENUM('yes', 'no'),
    Author VARCHAR(100),
    Publisher VARCHAR(100));

SELECT * FROM Books;


-- Create Customer table
CREATE TABLE Customer (Customer_Id INT PRIMARY KEY,
    Customer_name VARCHAR(100),
    Customer_address VARCHAR(255),
    Reg_date DATE);

SELECT * FROM Customer;



-- Create IssueStatus table
CREATE TABLE IssueStatus (Issue_Id INT PRIMARY KEY,
    Issued_cust INT,
    Issued_book_name VARCHAR(255),
    Issue_date DATE,
    Isbn_book INT,
    FOREIGN KEY (Issued_cust) REFERENCES Customer(Customer_Id),
    FOREIGN KEY (Isbn_book) REFERENCES Books(ISBN));
SELECT * FROM IssueStatus;

-- Create ReturnStatus table
CREATE TABLE ReturnStatus (Return_Id INT PRIMARY KEY,
    Return_cust INT,
    Return_book_name VARCHAR(255),
    Return_date DATE,
    Isbn_book2 INT,
    FOREIGN KEY (Return_cust) REFERENCES Customer(Customer_Id),
    FOREIGN KEY (Isbn_book2) REFERENCES Books(ISBN));

SELECT * FROM ReturnStatus;


-- Insert sample data into Branch table
INSERT INTO Branch VALUES
(1, 101, '355 head office', '585-1234'),
(2, 102, '451 ekm', '585-5678'),
(3, 103, '585 clcut St', '585-9012');

select * from Branch;

-- Insert sample data into Employee table
INSERT INTO Employee VALUES
(1, 'sara fernadas', 'Manager', 70000.00, 1),
(2, 'Jane Smith', 'Assistant Manager', 50500.00, 2),
(3, 'abbass ahmede', 'it', 355000.00, 3),
(4, 'safa ch', 'Cleaner', 35000.00, 1),
(5, 'David Lee', 'Clerk', 58000.00, 2),
(6, 'juliya femin', 'marketin head', 62000.00, 3);

select * from Employee;


-- Insert sample data into Books table
INSERT INTO Books VALUES
(1001, 'business management', 'Computer Science', 20.00, 'yes', 'John Smith', 'Tech Books Inc.'),
(1002, 'Data Structures and Algorithms', 'Computer Science', 25.00, 'yes', 'Jane Johnson', 'Tech Books Inc.'),
(1003, 'History of Ancient Rome', 'History', 15.00, 'yes', 'Mark Davis', 'History Press'),
(1004, 'Python Programming', 'Computer Science', 30.00, 'no', 'Sarah Adams', 'Programming Books Ltd.'),
(1005, 'Art of Painting', 'Art', 18.00, 'yes', 'Emily White', 'Art Publishing House');

select * from Books;


-- Insert sample data into Customer table
INSERT INTO Customer VALUES
(1, 'rahul', '123 bail St', '2022-02-15'),
(2, 'Boby', '878 fsr St', '2023-02-18'),
(3, 'Eves', '165 Main St', '2020-01-10'),
(4, 'Tommy', '589 my St', '2022-05-05'),
(5, 'Emmaas', '789 local St', '2013-12-10');

select * from Customer;


-- Insert sample data into IssueStatus table
INSERT INTO IssueStatus VALUES
(1, 1, 'Introduction to SQL', '2023-01-10', 1001),
(2, 2, 'Data Structures and Algorithms', '2023-03-15', 1002),
(3, 4, 'History of Ancient Rome', '2023-02-05', 1003),
(4, 5, 'Art of Painting', '2023-06-20', 1005),
(5, 3, 'Python Programming', '2023-04-01', 1004),
(6, 2, 'History of Ancient Rome', '2023-06-25', 1003);

select * from IssueStatus;

-- Insert sample data into ReturnStatus table
INSERT INTO ReturnStatus VALUES
(1, 1, 'Introduction to SQL', '2023-01-31', 1001),
(2, 2, 'Data Structures and Algorithms', '2023-03-30', 1002),
(3, 4, 'History of Ancient Rome', '2023-03-01', 1003),
(4, 3, 'Python Programming', '2023-04-20', 1004),
(5, 5, 'Art of Painting', '2023-07-05', 1005);

Select * from ReturnStatus;


-- Queries

-- 1. Retrieve the book title, category, and rental price of all available books.
SELECT Book_title, Category, Rental_Price FROM Books WHERE Status = 'yes';


-- 2. List the employee names and their respective salaries in descending order of salary.
SELECT Emp_name, Salary FROM Employee ORDER BY Salary DESC;


-- 3. Retrieve the book titles and the corresponding customers who have issued those books.
SELECT b.Book_title, c.Customer_name FROM IssueStatus i JOIN Books b ON i.Isbn_book = b.ISBN JOIN Customer c ON i.Issued_cust = c.Customer_Id;


-- 4. Display the total count of books in each category.
SELECT Category, COUNT(*) AS Total_Count FROM Books GROUP BY Category;


-- 5. Retrieve the employee names and their positions for employees whose salaries are above Rs. 50,000.
SELECT Emp_name, Position FROM Employee WHERE Salary > 50000;


-- 6. List the customer names who registered before 2022-01-01 and have not issued any books yet.
SELECT Customer_name FROM Customer WHERE Reg_date < '2022-01-01' AND Customer_Id NOT IN (SELECT Issued_cust FROM IssueStatus);



-- 7. Display the branch numbers and the total count of employees in each branch.
SELECT Branch_no, COUNT(*) AS Total_Employees FROM Employee GROUP BY Branch_no;


-- 8. Display the names of customers who have issued books in the month of June 2023.
SELECT DISTINCT c.Customer_name FROM IssueStatus i JOIN Customer c ON i.Issued_cust = c.Customer_Id WHERE MONTH(i.Issue_date) = 6 AND YEAR(i.Issue_date) = 2023;


-- 9. Retrieve book titles from the Books table containing 'history'.
SELECT Book_title FROM Books WHERE Category LIKE '%history%';


-- 10. Retrieve the branch numbers along with the count of employees for branches having more than 5 employees.
SELECT Branch_no, COUNT(*) AS Total_Employees FROM Employee GROUP BY Branch_no HAVING COUNT(*) > 5;


-- 11. Retrieve the names of employees who manage branches and their respective branch addresses.
SELECT e.Emp_name, b.Branch_address FROM Employee e JOIN Branch b ON e.Branch_no = b.Branch_no WHERE e.Position LIKE '%manager%';


-- 12. Display the names of customers who have issued books with a rental price higher than Rs. 25.
SELECT DISTINCT Customer_name FROM IssueStatus ib JOIN Books b ON i.Isbn_book = b.book_id JOIN Customer c on i.lss=c.customer_id where b.rental_price >25;
