# Vegetable Management System (VMS)

## Overview
The **Vegetable Management System (VMS)** is a Java-based desktop application developed to simplify and organize the process of vegetable trading.
It provides a common platform where different stakeholders such as farmers, wholesalers, and government authorities can interact and manage operations more efficiently.
This project focuses on improving transparency, reducing manual work, and enabling better coordination in the vegetable supply chain.

---

## Key Features
* Secure **role-based login system**
* Separate modules for **Farmers and Wholesalers**
* Monitoring access for **Panchayat, Mandi, and State authorities**
* **Price management** and tracking
* **Bidding system** for vegetable trading
* Integration with **MySQL database**

---

## Tech Stack
* **Java (Core + Swing)** – for application development
* **MySQL** – for database management
* **JDBC** – for database connectivity
* **Maven** – for project build and dependency management

---

## Project Structure
```
src/        → Application source code  
lib/        → External libraries  
Database/   → SQL scripts and database files  
pom.xml     → Maven configuration file  
```

---

## Database Design (MySQL)
The database is a core component of VMS — it's not just a backend detail, it drives the role-based access, bidding logic, and price tracking that make the entire system functional. Roughly half the engineering effort in this project goes into schema design and query logic, so it's documented here in the same depth as the Java modules.

### Core Tables
| Table | Purpose |
|---|---|
| `users` | Stores login credentials and role (Farmer, Wholesaler, Panchayat, Mandi, State) |
| `farmers` | Farmer profile details and linked produce listings |
| `wholesalers` | Wholesaler profile details and purchase history |
| `vegetables` | Vegetable catalog with category, unit, and quality info |
| `price_list` | Current and historical prices per vegetable, updated by authorities |
| `bids` | Bidding records — bidder, vegetable, amount, timestamp, status |
| `transactions` | Finalized trades between farmers and wholesalers |
| `authority_logs` | Audit/monitoring trail for Panchayat, Mandi, and State actions |

### Relationships
* A **farmer** can list multiple vegetables → `farmers` (1) → `vegetables` (many)
* A **vegetable** can receive multiple **bids** from different wholesalers → `vegetables` (1) → `bids` (many)
* A **bid**, once accepted, becomes a **transaction**
* **Authorities** (Panchayat/Mandi/State) have monitoring access across `price_list`, `bids`, and `transactions` for full transparency
* Foreign keys enforce referential integrity — e.g., a bid cannot exist without a valid `vegetable_id` and `wholesaler_id`


### JDBC Connectivity
* All reads/writes between the Swing UI and MySQL go through JDBC.
* The database layer is exercised on nearly every user action: login validation, price updates, bid placement, and transaction finalization.
* Connection credentials (URL, username, password) are configured in the Java DB connection class and must be updated to match your local MySQL instance before running the app.

### Why MySQL Matters Here
* Enforces **data integrity** through primary/foreign keys across farmers, vegetables, bids, and transactions.
* Enables **role-based data visibility** — authorities query monitoring views without being able to alter farmer/wholesaler data directly.
* Powers **price history tracking** so government bodies can audit rate changes over time.
* Supports the **bidding workflow** end-to-end: bid creation → status update → transaction record.

---

##  How to Run
1. Clone the repository:
```
git clone https://github.com/Gourav1525/VMS.git
```
2. Open the project in any Java IDE (NetBeans / IntelliJ / Eclipse)
3. **Set up MySQL:**
   - Create a new database (e.g., `vms_db`)
   - Import the schema and seed data using the SQL files provided in the `Database` folder
   - Verify all tables (`users`, `vegetables`, `price_list`, `bids`, `transactions`, etc.) were created successfully
4. **Configure database credentials:**
   - Open the JDBC connection class in `src/`
   - Update the DB URL, username, and password to match your local MySQL setup
   - Ensure MySQL service is running before launching the app
5. Build the project using Maven (`pom.xml`)
6. Run the main application file

> Most setup issues come from Steps 3–4 — a skipped import or mismatched credentials is the most common source of runtime/connection errors.

---

## Screenshots
Screenshots of the application interface can be added here to demonstrate functionality.

---

## Author
**Gourav Chouhan**

---

## Note
This project was developed as part of academic work and is intended to demonstrate the practical implementation of Java-based application development along with database integration.
