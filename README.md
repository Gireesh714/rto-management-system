# RTO Management System

A PostgreSQL-based database management project simulating a Regional Transport Office (RTO) system. It handles vehicle registrations, license issuance, permit renewals, fee payments, and user management using normalized relational schema and complex SQL queries.

## 📌 Features
- BCNF-normalized schema with 15+ tables
- Vehicle registration and ownership tracking
- License test results and appointment scheduling
- Permit and renewal management with expiry tracking
- SQL queries for analytics (e.g., test scores, trends, expiry listings)
- Data population scripts and constraint validations

## 🛠️ Technologies Used
- PostgreSQL
- SQL

## 📁 Folder Structure
- `ER_Diagram/` – contains the ER model and relational schema
- `SQL_Scripts/` – includes schema creation, insertion, and query files
- `Reports/` – project documentation and presentation slides

## 🧪 How to Run
1. Open PostgreSQL or pgAdmin
2. Run `schema.sql` to create tables
3. Run `insert.sql` to populate data
4. Use `queries.sql` to test analytics and reports
