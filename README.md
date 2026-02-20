# 🎬 CAP Theorem Implementation using Sakila Database (MySQL)

## 📌 Overview

This project demonstrates the **CAP Theorem (Consistency, Availability, Partition Tolerance)** using the **Sakila Movie Rental Database** in MySQL.

The Sakila database simulates a DVD rental system with tables such as:

* `film` — movie information
* `inventory` — movie copies
* `customer` — customer details
* `rental` — rental transactions
* `payment` — payment records
* `staff` — store staff

This experiment shows how distributed database concepts apply to real-world database operations.

---

## 🎯 Aim

To implement and demonstrate the three CAP theorem properties:

* **Consistency** — valid and correct data updates using transactions
* **Availability** — system always responds to queries
* **Partition Tolerance** — system continues operation despite data differences

---

## ⚙️ Requirements

* MySQL Server 8+
* MySQL Workbench
* Sakila Sample Database

---

## 📥 Sakila Installation

### 1. Download Sakila Database

Download from MySQL official site:

```
https://dev.mysql.com/doc/index-other.html
```

Download **Sakila Sample Database**.

---

### 2. Load Database

Run in MySQL:

```sql
SOURCE sakila-schema.sql;
SOURCE sakila-data.sql;
```

---

### 3. Use Database

```sql
USE sakila;
```

---

## 🧪 Implementation

---

## ✅ Program 1 — Consistency Demonstration

Uses transactions and foreign key constraints to ensure valid rental records.

### Check inventory

```sql
SELECT inventory_id, film_id, store_id
FROM inventory
LIMIT 5;
```

### Insert rental transaction

```sql
START TRANSACTION;

INSERT INTO rental
(rental_date, inventory_id, customer_id, staff_id)
VALUES (
NOW(),
(SELECT inventory_id FROM inventory LIMIT 1),
(SELECT customer_id FROM customer LIMIT 1),
(SELECT staff_id FROM staff LIMIT 1)
);

COMMIT;
```

### Verify record

```sql
SELECT * FROM rental
ORDER BY rental_id DESC
LIMIT 5;
```

✔ Demonstrates **Consistency**

---

## ✅ Program 2 — Availability Demonstration

System responds to read queries at all times.

### Retrieve films

```sql
SELECT title, rental_rate
FROM film
LIMIT 10;
```

### Retrieve customer rentals

```sql
SELECT c.first_name, c.last_name, f.title
FROM customer c
JOIN rental r ON c.customer_id = r.customer_id
JOIN inventory i ON r.inventory_id = i.inventory_id
JOIN film f ON i.film_id = f.film_id
LIMIT 10;
```

✔ Demonstrates **Availability**

---

## ✅ Program 3 — Partition Tolerance Simulation

Simulates distributed database nodes.

### Create replica table

```sql
CREATE TABLE inventory_backup AS
SELECT * FROM inventory;
```

### Update original table

```sql
UPDATE inventory
SET store_id = 2
WHERE inventory_id = (
SELECT inventory_id FROM inventory LIMIT 1
);
```

### Compare tables

```sql
SELECT inventory_id, store_id FROM inventory LIMIT 5;
SELECT inventory_id, store_id FROM inventory_backup LIMIT 5;
```

✔ Demonstrates **Partition Tolerance**

---

## ✅ Program 4 — CAP Trade-off (Consistency vs Availability)

### Consistency priority (locking)

```sql
SELECT *
FROM inventory
WHERE inventory_id = (
SELECT inventory_id FROM inventory LIMIT 1
)
FOR UPDATE;
```

### Availability priority

```sql
SELECT * FROM inventory LIMIT 5;
```

---

## 📊 Results

* Transactions ensured consistent rental records.
* Query operations provided continuous system availability.
* Data replication simulation demonstrated partition tolerance.

---

## 🧠 Conclusion

The experiment successfully demonstrated CAP theorem properties using the Sakila movie rental database. The system maintained data correctness through transactions, provided continuous access to data, and simulated distributed system behavior.

---

## 📚 Concepts Covered

* CAP Theorem
* MySQL Transactions
* Foreign Keys
* Database Consistency
* Data Availability
* Distributed Systems
