# Use Case Diagram – Airbnb Clone System

This directory contains the **Use Case Diagram** for the **Airbnb Clone System**.  
The diagram visualizes how different actors interact with the system and the key functionalities provided.

## 🎯 Objective
The goal of this use case diagram is to illustrate:
- The main **actors** (Guest, Host, Admin, Payment Gateway, Email Service)
- The **use cases** (Register, Login, Manage Properties, Book Properties, Make Payments, Leave Reviews, etc.)
- The relationships between actors and use cases (**include** and **extend** where applicable)

## 🛠️ System Name
**Airbnb Clone System**  
The name appears inside the system boundary in the diagram, containing all use cases.

## 👥 Actors
- **Guest:** Registers, logs in, searches properties, books, pays, and leaves reviews.  
- **Host:** Manages property listings, views bookings, and responds to reviews.  
- **Admin:** Manages users, monitors bookings/payments, and handles escalations.  
- **Payment Gateway:** Handles secure payment processing.  

## 🔑 Relationships
- **Include:**  
  - `Book Property` **includes** `Check Availability` and `Make Payment`  
  - `Register` **includes** `Verify Email`

- **Extend:**  
  - `Cancel Booking` **extends** `View Booking` (optional action)  
  - `Refund Payment` **extends** `Cancel Booking` (only if payment exists)

## 📄 File
The diagram is exported as **PNG** from Draw.io and stored in this directory.  

- **Diagram file:** `airbnb-use-case-diagram.png`

## 📷 Preview
![Use Case Diagram](./use-case-diagram.png)

---

## 📌 How to Reproduce
1. Open [Draw.io](https://app.diagrams.net/)
2. Recreate actors, use cases, and relationships as shown.
3. Export as `PNG` and place it in this folder.

---

## 📝 Notes
- This diagram focuses on **high-level functionality** and user interactions.
- Technical details (like APIs, database schema) are handled in other documentation sections.

