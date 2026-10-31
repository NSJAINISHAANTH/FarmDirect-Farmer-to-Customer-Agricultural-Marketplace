# FarmDirect – Direct Farmer-to-Customer Marketplace

## 1. Project Title

**FarmDirect** – A simple web application for selling agricultural products directly from farmers to customers.

## 2. Introduction

Farmers often sell their produce through middlemen and receive a lower price. Customers also pay more because of extra charges in the supply chain. FarmDirect is a small college project that connects farmers and customers on one website. Farmers can list products. Customers can view products, search by name, and place orders. There is no online payment. The customer simply clicks **Place Order / BUY NOW**.

## 3. Problem Statement

Agricultural products usually pass through several intermediaries before they reach the customer. This reduces the farmer’s profit and increases the customer’s cost. Existing large e-commerce systems are complex and not suitable as a beginner Java project. There is a need for a **small, easy-to-explain** web application that demonstrates farmer-to-customer selling using Java, JDBC and MySQL.

## 4. Proposed Solution

FarmDirect provides:

- Farmer registration and login
- Product add / view / edit / delete
- Customer registration and login
- Product browsing and search
- Simple order placement with stock update
- Farmer view of customer orders

The application uses HTML, CSS and JavaScript for the user interface, Java Servlets for request handling, JDBC for database access, and MySQL for storage.

## 5. Objectives

- Remove the middleman between farmer and customer.
- Allow farmers to manage their own product listings.
- Allow customers to search products and place orders.
- Keep product stock updated after each successful order.
- Use JDBC with `PreparedStatement` for safe database operations.
- Keep the project small enough for a final-year viva.

## 6. Features

### Farmer

- Register and login
- Add agricultural products
- View, edit and delete own products
- View customer orders
- Accept, mark delivered, or cancel an order

### Customer

- Register and login
- View all available products
- Search products by name (`LIKE`)
- View product details (name, category, quantity, price, farmer, location)
- Place an order
- View own orders

### Order

- Quantity check against available stock
- Total price = quantity × price per unit
- Order saved in MySQL
- Product quantity reduced
- If insert or update fails, the JDBC transaction is rolled back

## 7. Technologies Used

| Layer | Technology |
| --- | --- |
| Frontend | HTML5, CSS3, JavaScript |
| Backend | Java Servlets |
| Database access | JDBC (`Connection`, `PreparedStatement`, `ResultSet`) |
| Database | MySQL |
| Build | Maven |
| Server | Apache Tomcat 9 |

Not used: React, Angular, Node.js, Spring Boot, payment gateways, maps, chat, AI/ML.

## 8. System Requirements

- JDK 11 or later
- Apache Maven 3.8+
- Apache Tomcat 9 (javax.servlet)
- MySQL 8
- A modern web browser (Chrome, Edge, Firefox)

## 9. Database Design

Database name: `farmdirect`

### users

| Column | Description |
| --- | --- |
| user_id | Primary key |
| name | Full name |
| email | Unique email |
| password | Login password |
| phone | Phone number |
| role | `FARMER` or `CUSTOMER` |

### products

| Column | Description |
| --- | --- |
| product_id | Primary key |
| farmer_id | Foreign key to `users.user_id` |
| product_name | Name of the product |
| category | Vegetable, Fruit, Grain, etc. |
| quantity | Available quantity in kg |
| price | Price per kg |
| location | Farmer location |

### orders

| Column | Description |
| --- | --- |
| order_id | Primary key |
| customer_id | Foreign key to `users.user_id` |
| farmer_id | Foreign key to `users.user_id` |
| product_id | Foreign key to `products.product_id` |
| quantity | Ordered quantity |
| total_price | quantity × price |
| order_date | Date and time of order |
| status | `PLACED`, `ACCEPTED`, `DELIVERED`, `CANCELLED` |

SQL script: `database/farmdirect.sql`

## 10. Module Description

### Farmer module

Farmer registers, logs in, and opens the farmer dashboard. From there the farmer adds products (name, category, quantity, price, location), edits or deletes them, and sees orders from customers.

### Customer module

Customer registers, logs in, searches or browses products, opens product details, enters quantity, and clicks **BUY NOW**. The system checks stock and creates an order.

### Order module

`OrderDAO.placeOrder()` runs in a transaction:

1. Lock and read product row
2. Reject order if requested quantity is more than available stock
3. Insert order with status `PLACED`
4. Reduce product quantity
5. Commit, or rollback on error

## 11. How to Run

1. Install JDK, Maven, MySQL and Tomcat 9.
2. Open MySQL and run `database/farmdirect.sql`.
3. Open `src/main/resources/db.properties` and set your MySQL username and password.
4. From the `FarmDirect` folder run:

```bash
mvn clean package
```

5. Copy `target/FarmDirect.war` to Tomcat’s `webapps` folder and start Tomcat.
6. Open:

```text
http://localhost:8080/FarmDirect/
```

### Demo login (password for all: `password123`)

| Role | Email |
| --- | --- |
| Farmer | ravi@farmdirect.com |
| Farmer | arun@farmdirect.com |
| Customer | priya@gmail.com |
| Customer | karthik@gmail.com |

If the app cannot connect to MySQL, check that the `farmdirect` database exists and that `db.properties` matches your MySQL user.

### Project structure

```text
FarmDirect
 ├── database/farmdirect.sql
 ├── src/main/java
 │    ├── model        User, Product, Order
 │    ├── dao          UserDAO, ProductDAO, OrderDAO
 │    ├── util         DBConnection
 │    └── controller   LoginController, ProductController, OrderController
 └── src/main/webapp   HTML, CSS, JS
```

## 12. Future Enhancements

These are **not implemented** in this project. They can be added later:

- Online payment
- Mobile application
- Multilingual support
- Delivery tracking
- AI-based crop price prediction
- Location-based recommendations
