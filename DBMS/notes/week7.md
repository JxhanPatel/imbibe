# 7.1: Application Design and Development/1: Architecture


## **Recap**
*   **Normal Forms and Relational Design**: Studied how progressive increase of constraints can minimize redundancy in a schema.
*   **3NF Decomposition**: Learnt how to decompose a schema into 3NF while preserving dependency and lossless join.
*   **BCNF Decomposition**: Learnt how to decompose a schema into BCNF with lossless join.
*   **Library Information System (LIS)**: Illustrated schema design and refinement for finalization using LIS specifications.
*   **Multivalued Dependencies (MVD) & 4NF**: Understood MVDs to handle attributes with multiple values and learnt 4NF decomposition.
*   **Design Summary & Temporal Data**: Discussed the overall database design process and issues related to modeling temporal data.

---

## **Objectives**
*   To identify **Application Programs** across various sectors.
*   To understand the **commonality of architecture** across applications.
*   To understand the **classification and evolution** of architectures.
*   To look at the architecture for a few **sample applications**.

---

## **1. Application Programs and Architecture**

### **Diversity and Unity**
*   **Diversity**: Applications differ widely in domain (financial, travel, education, etc.), functionality, user base, response time, and scale.
*   **Unity**: Despite diversity, these applications have much in common:
    *   Most use an **RDBMS** (e.g., Oracle, DB2, MySQL, PostgreSQL) for managing data.
    *   Applications are functionally split into three layers: **Frontend**, **Middle**, and **Backend**.

### **The Three Functional Layers (Tiers)**
1.  **Frontend or Presentation Layer / Tier**:
    *   Interacts with the user (Display/View, Input/Output).
    *   Examples: Choosing items, adding to cart, paying, tracking orders.
    *   Interfaces: Browser-based, Mobile App, or Custom.
2.  **Middle or Application / Business Logic Layer / Tier**:
    *   Implements the functionality and links the front and back ends.
    *   Tasks: Authentication, search logic, pricing, cart management, payment handling, and delivery management.
3.  **Backend or Data Access Layer / Tier**:
    *   Manages persistent, large-volume data with efficient access and security.
    *   Databases: User, inventory, orders, vendor databases.

<img width="720" height="640" alt="image" src="https://github.com/user-attachments/assets/9d09f801-0514-49f7-9406-bb1557cf5023" />

---

## **2. Application Architecture: Detailed Layers**

### **Presentation Layer: MVC Architecture**
Most presentation layers follow the **Model-View-Controller (MVC)** architecture:
*   **Model**: Represents the business logic.
*   **View**: Handles the presentation of data, which depends on the display device (e.g., monitor vs. mobile screen).
*   **Controller**: Receives events (user actions), executes actions on the model, and returns a view to the user.

### **Business Logic Layer**
*   Provides high-level abstractions of entities (e.g., students, instructors, courses).
*   Enforces **business rules** (e.g., a student can only enroll if prerequisites are met and fees are paid).
*   Supports **workflows** that define sequences of steps for multi-participant tasks and error handling.

### **Data Access Layer and Object-Relational Mapping (ORM)**
*   Interfaces between the business logic layer and the underlying database.
*   **ORM**: Allows application code to be written using an object-oriented data model while storing data in a traditional relational database.
*   Designer provides a mapping (e.g., a Java class `Student` mapped to the relation `student`).
*   The application opens a **session** to connect to the database; objects are saved using commands like `session.save(object)`, which the mapping translates into SQL tuples.

---

## **3. Architecture Classification and Evolution**

### **Historical Evolution**
1.  **Mainframe Era (1960s and 70s)**: Centralized systems with terminals connected via proprietary networks or dial-up lines.
2.  **Personal Computer Era (1980s)**: Applications running on desktop PCs connected via Local Area Networks (LAN) to a database.
3.  **Web / Mobile Era (1990s onwards)**: Users access applications via web browsers or mobile apps over the Internet.

### **Tier Classifications**
*   **1-Tier Architecture**: All components (interface, middleware, and back-end data) reside on a single server or platform. This is the simplest but least scalable approach.
*   **2-Tier Architecture**: Based on the **Client-Server** model. Direct communication takes place between the client (Presentation) and the server (Database) without an intermediate layer.
*   **3-Tier Architecture**: Tiers (Presentation, Logic, and Data Access) are separated based on complexity and usage. It is the most widely used architecture for designing a DBMS.
*   **n-Tier Architecture**: Distributes components of the 3 tiers across different servers and adds interface tiers for workload balancing.

---

## **4. Sample Applications in Multiple Tiers**

| Application | Presentation | Logic | Data |
| :--- | :--- | :--- | :--- |
| **Web Mail** | Login, Inbox view, Mail composer, Filters | User authentication, Connection to Mail Server (SMTP, etc.), Encryption/Decryption | Mail Users, Address Book, Mail Items |
| **Net Banking** | Login, Account View, Fund Transfer interface | Beneficiary authentication, Transaction validation, Bank/Gateway connection | Account Holders, Beneficiaries, Transactions |
| **Timetable** | Login, Add/Delete Courses, Assignments view | Assignment logic, Teacher-Course allocation logic | Courses, Teachers, Rooms, Slots |
