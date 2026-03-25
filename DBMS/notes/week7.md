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





---



# **7.2: Application Design and Development/2: Web Applications**



## **1. Objectives**
*   To familiarize with the fundamental notions and technologies of the Web.
*   To learn about the role and types of scripting in web applications.
*   To learn about Java Servlets and their implementation.

---

## **2. Web Fundamentals**

### **2.1. The World Wide Web (WWW)**
*   **Definition**: A distributed information system based on **hypertext**.
*   **HTML**: Most web documents are formatted via HyperText Markup Language. They contain text, formatting instructions, hypertext links, and forms for data entry.

### **2.2. Uniform Resource Locators (URLs)**
*   **Function**: Provide the functionality of pointers on the web.
*   **Structure**: 
    *   **Protocol**: (e.g., `http` or `https`) indicates how the document is accessed.
    *   **Machine Name**: Identifies the unique machine on the Internet.
    *   **Path/Identifier**: Locates the document or program within that specific machine.
*   **Examples**:
    *   `http://www.google.com/search?q=silberschatz`: An identifier of a program plus arguments.
    *   `file:///C:/WINDOWS/...`: Accesses a local file.

### **2.3. Identifiers: URI, URL, and URN**
*   **URI (Uniform Resource Identifier)**: The broad classification.
*   **URL (Uniform Resource Locator)**: Resembles a person's street address; provides a method for finding an item.
*   **URN (Uniform Resource Name)**: Functions like a person's name; defines an item's identity regardless of location.

<img width="1277" height="644" alt="image" src="https://github.com/user-attachments/assets/00e342ae-ee56-48ff-89b2-a9421759d572" />

### **2.4. HTML and HTTP**
*   **HTML**: Provides formatting, images, tables, and input features (menus, radio buttons, text boxes).
*   **HTTP**: The protocol used for communication with web servers. It is **connectionless**—the server closes the connection and "forgets" the request after replying.

### **2.5. Sessions and Cookies**
*   **Motivation**: Because HTTP is connectionless, information services need a way to track session-specific data (e.g., user authentication).
*   **Cookies**: A small piece of text containing identifying information.
    *   Sent by the server to the browser on first interaction to identify a session.
    *   Stored by the browser and sent back to the server on subsequent interactions.

### **2.6. Web Infrastructure**
*   **Web Browser**: Application software that fetches content and displays it via a **rendering engine**. It transforms HTML and resources into a visual representation.
*   **Web Server**: Software/hardware that accepts HTTP/HTTPS requests. It can execute programs identified in a URL and deliver the results as HTML.
*   **CGI (Common Gateway Interface)**: A standard interface between a web server and an application server.
*   **Web Services**: Remote procedure call mechanisms (e.g., **REST** returning JSON/XML).

---

## **3. Scripting for Web Applications**

### **3.1. Definition**
*   **Script**: A list of text commands embedded in a web page or server, interpreted and executed by a scripting engine.

### **3.2. Types of Scripting**
1.  **Client-Side Scripting**: 
    *   Responsible for interaction within the web page.
    *   Downloaded to the client and executed by the browser (e.g., **JavaScript**).
2.  **Server-Side Scripting**: 
    *   Responsible for tasks at the server end (e.g., database access).
    *   Executes on the server, generates HTML, and sends only the result to the client.

### **3.3. Client-Side Scripting and JavaScript**
*   **Benefits**: Permits flexible user interaction, animations, and correctness checks (input validation) without round trips to the server.
*   **Security**: Runs in "safe mode"—scripts generally cannot write to the client disk or make direct system calls outside the browser.
*   **JavaScript**: The basis for **Web 2.0** and **Ajax**. It can modify the **DOM (Document Object Model)** tree to change the page without refreshing.

### **3.4. Server-Side Scripting**
*   **Languages**: JSP, PHP, VBScript, Perl, Python.
*   **Function**: Simplifies connecting databases to the web by allowing SQL queries to be embedded directly in HTML documents.

---

## **4. Servlets and Java Server Pages (JSP)**

### **4.1. Java Servlets**
*   **Definition**: An API for communication between the web/application server and an application program running in the server.
*   **Mechanism**: The servlet is loaded into the server; each user request spawns a new **thread** in the server, which is closed once serviced.
*   **Sessions**: The Servlet API supports session handling by setting cookies and allowing the storage of attribute value pairs (e.g., `session.setAttribute("userid", userid)`).

### **4.2. Java Server Pages (JSP)**
*   **Concept**: HTML pages with embedded Java code.
*   **JSP vs. Pure Servlets**: JSP is more convenient for designing HTML than using millions of `println` statements in a servlet.
*   **JSP vs. JavaScript**: JavaScript is "Client-Side" (executed after server response); JSP is "Server-Side" (executed before server response and can access resources like databases).

### **4.3. PHP**
*   **Function**: Widely used for web server scripting with extensive libraries for database access via **ODBC**.
