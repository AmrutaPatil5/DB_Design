## Comic-Con Parking System

## 📌 Overview

This project presents the **ER Diagram design** for a multi-zone event parking system used in large-scale conventions like Comic-Con. The goal is to design a clean, scalable database that supports:

- Vehicle entry and exit tracking
- Parking spot allocation across zones and levels
- Reserved parking categories (VIP, Staff, Exhibitors, EV, etc.)
- Parking sessions and ticket management
- Payment tracking and pricing logic

This system models a **real-world parking facility**, not just a simple entry-exit tracker, ensuring flexibility and scalability.

---

## 🎯 Understanding of Workflow

The system follows a structured real-world flow:

1. A **vehicle enters** the parking facility
2. A **parking ticket is generated**
3. A suitable **parking spot is assigned** based on vehicle type and category
4. A **parking session begins** (entry time recorded)
5. The vehicle occupies the spot during the session
6. On exit, **exit time is recorded**
7. Parking charges are calculated using **pricing rules**
8. A **payment is recorded**

This design supports:

- multiple visits by the same vehicle
- reuse of parking spots over time
- tracking currently parked vehicles
- handling reserved parking categories

---

## 🧱 Entities Used in the Design

### 🚙 Vehicle Categories

Defines types of vehicles:

- Bike, Car, SUV, EV, etc.

- One category can have many vehicles

---

### 🚗 Vehicles

Stores vehicle information.

- Each vehicle belongs to one category
- A vehicle can have multiple parking sessions

---

### 🗺️ Parking Zones

Represents parking areas divided by:

- zones

- levels

- One zone contains multiple parking spots

---

### 🏷️ Spot Categories

Defines reservation types:

- VIP

- Staff

- Exhibitor

- EV Charging

- General

- One category can be assigned to multiple parking spots

---

### 🅿️ Parking Spots

Represents individual parking spaces.

- Each spot belongs to a zone
- Each spot has a category
- One spot can be reused across multiple sessions

---

### 🎫 Parking Tickets

Generated at the time of entry.

- Linked to a vehicle
- Associated one-to-one with a parking session

---

### 🔴 Parking Sessions

Core entity of the system.

- Tracks entry and exit timestamps
- Links vehicle, parking spot, and ticket
- Determines whether a vehicle is currently parked

---

### 💳 Payments

Stores payment details.

- Linked to parking sessions
- Includes amount, status, and payment time

---

### 💰 Pricing Rules

Defines pricing logic.

- Based on:
  - vehicle category
  - spot category

- Enables flexible fee calculation

---

### 👥 Access Categories

Represents special access roles:

- VIP
- Staff
- Exhibitors
- Cosplayers

---

### 🔗 Vehicle Access (Bridge Table)

Handles many-to-many relationship:

- One vehicle can have multiple access categories
- One category can apply to multiple vehicles

---

## 🔗 Relationships & Design Logic

### Key Relationships

- **Vehicle Category → Vehicles (1:N)**
- **Vehicle → Parking Sessions (1:N)**
- **Parking Zone → Parking Spots (1:N)**
- **Spot Category → Parking Spots (1:N)**
- **Parking Spot → Parking Sessions (1:N)**
- **Parking Session → Payment (1:N)**

---

### Ticket & Session Relationship

- **Parking Session ↔ Parking Ticket (1:1)**

Each session has exactly one ticket, ensuring proper tracking of entry.

---

### Many-to-Many Relationship

- **Vehicle ↔ Access Categories (M:N)** via `vehicle_access`

This allows flexible assignment of special privileges.

---

## 🔑 Key Design Decisions

### 1. Parking Session as Core Entity

The entire system revolves around:

- vehicle
- parking spot
- time

This allows:

- multiple visits per vehicle
- reuse of parking spots
- tracking active vehicles

---

### 2. Separation of Ticket and Session

- Ticket = generated at entry
- Session = full lifecycle (entry → exit)

This ensures better tracking and scalability.

---

### 3. Spot Reusability

Parking spots are **not permanently assigned**.

- One spot can serve many vehicles over time
- Managed through parking sessions

---

### 4. Pricing Logic Separation

Pricing is handled through a separate entity:

- `pricing_rules`

This allows:

- dynamic pricing
- category-based billing

---

### 5. Availability Handling

Parking availability is **not stored directly**.

Instead:

- A spot is considered occupied if:
  - there is an active session (no exit time)

This avoids data inconsistency.

---

### 6. Reserved Parking Support

Handled using:

- `spot_categories` → defines reserved spots
- `access_categories` → defines who can access them

---

## 🔑 PK/FK Design

- Every entity has a **Primary Key (PK)**
- Relationships are maintained using **Foreign Keys (FK)**

Examples:

- `vehicles.category_id → vehicle_categories.category_id`
- `parking_sessions.vehicle_id → vehicles.vehicle_id`
- `parking_spots.zone_id → parking_zones.zone_id`
- `payments.session_id → parking_sessions.session_id`

This ensures:

- referential integrity
- consistent data linking

---

## 🧩 Normalization & Data Structure

- Follows **Third Normal Form (3NF)**
- Avoids redundancy
- Uses bridge table for many-to-many relationships
- Keeps entities modular and scalable

---

## 📊 Practicality of the Model

This design supports:

✔ Multiple entries per vehicle
✔ Parking spot reuse across sessions
✔ Multi-zone and multi-level parking
✔ Reserved parking categories
✔ Entry and exit tracking
✔ Payment and pricing logic
✔ Tracking currently parked vehicles

---

## 🖼️ Diagram Quality

- Clean and structured layout
- Logical grouping of entities
- Clearly defined relationships
- PK and FK properly marked
- Easy to read and review

---

## 🚀 Possible Improvements (Future Scope)

- Real-time occupancy dashboard
- EV charging session tracking
- Surge pricing during peak hours
- Reservation/booking system
- Audit logs for entry/exit tracking

---

## 🧠 Conclusion

This ER diagram represents a **real-world, scalable parking management system** suitable for large events. It effectively models vehicle flow, parking allocation, session tracking, and payment handling while maintaining clarity, flexibility, and normalization.

---
