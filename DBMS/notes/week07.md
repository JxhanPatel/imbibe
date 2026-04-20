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





---




# **7.3: Application Design and Development/3: SQL and Native Language**



## **1. Objectives**
*   To understand the "missing piece" in application development: how to connect high-level programming logic (Middle Tier) to backend SQL data (Data Access Layer).
*   To learn the implementation of SQL within a programming language using two dominant frameworks: **Connectionist** and **Embedding**.

---

## **2. The Interaction Challenge**

### **2.1. The Paradigm Mismatch**
*   Middle-tier logic is typically written in an **Object-Oriented** language (Java, C++, Python) where data is modeled as objects and methods.
*   Backend data is **Relational**, accessible only via SQL.
*   **Challenge**: From the middle tier, one must execute SQL commands and retrieve results (tables/relations) back into the high-level program variables.

<img width="1275" height="433" alt="image" src="https://github.com/user-attachments/assets/984503af-cb3d-4beb-be06-87712c380210" />
<img width="1309" height="613" alt="image" src="https://github.com/user-attachments/assets/323d110f-86ec-4374-922f-6d36d0a7681b" />



### **2.2. Two Dominant Frameworks**
1.  **Connectionist (API-based)**: The application uses an Application Program Interface (API) to send SQL as character strings to the database server and fetch results one-by-one.
2.  **Embedding**: SQL commands are placed directly inside the source code of the native language, identified by specific markers (e.g., `EXEC SQL`).

---

## **3. Connectionist Frameworks**

### **3.1. ODBC (Open Database Connectivity)**
*   **Definition**: A standard API for accessing a DBMS, aimed at being independent of specific database systems and operating systems.
*   **Portability**: Applications can be ported across platforms with minimal changes to data access code.
*   **Process**: Open connection $\rightarrow$ Send queries/updates $\rightarrow$ Fetch results.

**Python ODBC Example (`pyodbc`):**
```python
import pyodbc
# Connection string using a Data Source Name (DSN)
conn = pyodbc.connect('DSN=SQLS; UID=test01; PWD=test01')
cursor = conn.cursor()
# Executing SQL commands
cursor.execute("create table rvtest (col1 int, col3 varchar(10))")
cursor.execute("insert into rvtest values (1, 'ABC')")
cursor.execute("select * from rvtest")
# Fetching data using the cursor as an iterator
while True:
    row = cursor.fetchone()
    if not row: break
    print(row)
```

### **3.2. JDBC (Java Database Connectivity)**
*   **Definition**: A Java-specific API defining how a client accesses a database. It is part of the Java Standard Edition platform.
*   **Communication Model**:
    1.  **Open a Connection**: Uses a connection URL (server, port, database name, credentials).
    2.  **Create a Statement Object**: Serves as a handle between Java and the database.
    3.  **Execute Queries**: Send SQL and fetch results into a `ResultSet` object.
    4.  **Exception Handling**: Uses a try-catch mechanism to manage errors (e.g., wrong password).

**Java JDBC Snippet:**
```java
// Connection setup
String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=AdventureWorks";
try (Connection con = DriverManager.getConnection(connectionUrl);
     Statement stmt = con.createStatement();) {
    // Execute query
    String SQL = "SELECT * FROM Production.Product;";
    ResultSet rs = stmt.executeQuery(SQL);
    // Iterate through ResultSet
    while (rs.next()) {
        System.out.println(rs.getString("ProductID") + ":" + rs.getString("Name"));
    }
}
```
<img width="1352" height="525" alt="image" src="https://github.com/user-attachments/assets/ecf86726-45a2-452b-bde8-3f626db4c341" />


### **3.3. Bridge Configurations**
*   **Definition**: A special driver that translates source function-calls into target function-calls (e.g., **ODBC-to-JDBC bridge**).
*   Used when a programmer lacks a direct driver for a database but has access to a different target driver.
<img width="1122" height="558" alt="image" src="https://github.com/user-attachments/assets/e32686a9-ee88-47de-83d5-ecd15643ccde" />

---

## **4. Embedded SQL**

### **4.1. Concept and Host Languages**
*   SQL standards define embedding for languages like C, C++, Java, and Fortran.
*   **Host Language**: The native language where SQL is embedded.
*   **Markers**: Request to pre-processors are usually identified by `EXEC SQL` (C/C++) or `#sql` (Java).

### **4.2. Host Variables and Cursors**
*   **Host Variables**: Native language variables used within SQL statements, preceded by a colon (e.g., `:credit_amount`) to distinguish them from SQL attributes.
*   **Cursors**: Declared to handle queries that return multiple tuples. The program iterates through the cursor to place tuple values into host variables.
*   **Operations**:
    *   **DECLARE**: Identifies the query.
    *   **OPEN**: Executes the query and saves results in a temporary relation.
    *   **FETCH**: Retrieves successive tuples into host variables.
    *   **CLOSE**: Deletes the temporary relation.

### **4.3. Examples of Embedded SQL**

**C Language (DB2 Style):**
```c
EXEC SQL BEGIN DECLARE SECTION;
    short sage, sid; char sname;
EXEC SQL END DECLARE SECTION;
// ... connection logic ...
EXEC SQL SELECT SNAME, AGE into :sname, :sage
FROM ONE.SAILOR
WHERE sid = :sid;
printf("Sailor %s's age is %d.", sname, sage);
```


**Java Language (SQLJ):**
*   Uses `#sql` syntax and iterators.
*   **Named Binding**: Returns values based on column names.
*   **Positional Binding**: Returns values by column position.

---

## **5. Summary Table: Comparison**

| Feature | Connectionist (ODBC/JDBC) | Embedded SQL |
| :--- | :--- | :--- |
| **Mechanism** | API calls (Functions/Methods) | Marker tags for Pre-processor |
| **SQL Handling** | Constructed as character strings at runtime | Hard-coded into source or pre-compiled |
| **Variable Access** | Via API getter/setter methods | Direct use with colon prefix (`:var`) |
| **Iterators** | `ResultSet` object | Cursors or Language Iterators |




---







# **7.4: Application Design and Development/4: Python and PostgreSQL**


### **1. Objectives**
*   To understand how to access a PostgreSQL database from Python.
*   To understand how to build a Python Web Application with PostgreSQL.


## **2. Python and PostgreSQL Integration**

### **2.1. Python Modules for PostgreSQL**
The following Python modules can be used to work with a PostgreSQL database server:
*   **psycopg2**
*   pg8000
*   py-postgresql
*   PyGreSQL
*   ocpgdb
*   bpgsql
*   SQLAlchemy (requires one of the above to be installed separately)

### **2.2. The `psycopg2` Package**
**Advantages of `psycopg2`:**
*   It is the most popular and stable module for working with PostgreSQL.
*   It is used in most Python and Postgres frameworks.
*   It is an actively maintained package supporting Python 2.x and 3.x.
*   It is thread-safe and designed for heavily multi-threaded applications.

**Installation:**
*   General: `pip install psycopg2`
*   Specific version: `pip install psycopg2==2.8.6`

---

## **3. Steps to Access PostgreSQL from Python**

The process follows a standard workflow where the Python program interacts with the database through the `psycopg2` module and Python Database APIs.

<img width="1097" height="352" alt="image" src="https://github.com/user-attachments/assets/7524e304-1f58-4382-998a-15c5e50659f7" />

### **3.1. Standard Workflow**
1.  **Create connection**: Use `psycopg2.connect()` with required arguments (database name, user, password, host, port).
2.  **Create cursor**: Create a cursor object using the `cursor()` method of the connection object to act as an iterator for result sets.
3.  **Execute the query**: The `execute()` method runs SQL commands.
4.  **Commit/rollback**: Use `commit()` to make changes persistent or `rollback()` to revert changes since the last commit.
5.  **Close the cursor**: Release the cursor resource.
6.  **Close the connection**: Close the database connection.

### **3.2. Core `psycopg2` APIs**

| Method / Attribute | Description |
| :--- | :--- |
| `psycopg2.connect(...)` | Opens a connection to the database. Returns a connection object. |
| `connection.cursor()` | Creates a cursor used throughout the program. |
| `cursor.execute(sql [, parameters])` | Executes an SQL statement. Supports placeholders like `%s` (e.g., `cursor.execute("insert into people values (%s, %s)", (who, age))`). |
| `cursor.rowcount` | A read-only attribute returning the number of rows modified, inserted, or deleted by the last `execute()`. |
| `cursor.fetchone()` | Fetches the next row of a query result set. |
| `cursor.fetchall()` | Fetches all remaining rows of a query result set. |
| `connection.commit()` | Commits the current transaction, making changes visible to other connections. |
| `connection.rollback()` | Rolls back any changes since the last `commit()`. |

---

## **4. CRUD Examples in Python**

The following examples assume the database "mydb", user "myuser", password "mypass", host "127.0.0.1", and port "5432".

### **4.1. Creating a Table**
```python
import psycopg2
def createTable():
    try:
        conn = psycopg2.connect(database="mydb", user="myuser", password="mypass", host="127.0.0.1", port="5432")
        cur = conn.cursor()
        cur.execute('''CREATE TABLE EMPLOYEE 
                      (emp_num INT PRIMARY KEY NOT NULL, 
                       emp_name VARCHAR(40) NOT NULL, 
                       department VARCHAR(40) NOT NULL)''')
        conn.commit()
        print("Table created successfully")
    except Exception as error:
        print(error)
```

### **4.2. Inserting and Updating Records**
*   **Insert**: `cur.execute("INSERT INTO EMPLOYEE (emp_num, emp_name, department) VALUES (%s, %s, %s)", (num, name, dept))`
*   **Update**: `cur.execute("UPDATE EMPLOYEE SET department = %s WHERE emp_num = %s", (dept, num))`

### **4.3. Selecting Records**
```python
cur.execute("SELECT emp_num, emp_name, department FROM EMPLOYEE")
rows = cur.fetchall()
for row in rows:
    print("ID =", row, "Name =", row, "Dept =", row)
```



### **4.4. Delete Example**
The `DELETE` operation removes specific rows from a table based on a condition. In this example, an employee record is deleted based on their employee number (`emp_num`).

```python
import psycopg2

def deleteRecord(num):
    conn = None
    try:
        # 1. Create connection to the PostgreSQL database
        conn = psycopg2.connect(
            database="mydb", 
            user="myuser", 
            password="mypass", 
            host="127.0.0.1", 
            port="5432"
        )
        
        # 2. Create a cursor object
        cur = conn.cursor()

        # 3. Execute the DELETE statement using %s as a placeholder for parameters
        # Note: The parameter must be passed as a tuple (num,)
        cur.execute("DELETE FROM EMPLOYEE WHERE emp_num = %s", (num,))

        # 4. Commit the changes to make them persistent in the database
        conn.commit()

        # 5. Use rowcount to verify the number of rows deleted
        print("Total number of rows deleted:", cur.rowcount)
        
        # 6. Close the cursor
        cur.close()

    except (Exception, psycopg2.DatabaseError) as error:
        print(error)
        if conn is not None:
            conn.rollback() # Revert changes if an error occurs
    finally:
        # 7. Close the connection
        if conn is not None:
            conn.close()

# Function call to delete employee with ID 110
deleteRecord(110)
```

### **4.5. Update Example**
The `UPDATE` operation modifies existing data. This example updates the department field for a specific employee identified by their employee number.

```python
import psycopg2

def updateRecord(num, dept):
    conn = None
    try:
        # 1. Create connection
        conn = psycopg2.connect(
            database="mydb", 
            user="myuser", 
            password="mypass", 
            host="127.0.0.1", 
            port="5432"
        )
        
        # 2. Create a cursor
        cur = conn.cursor()

        # 3. Execute the UPDATE statement with multiple placeholders
        cur.execute("UPDATE EMPLOYEE set department = %s where emp_num = %s", (dept, num))

        # 4. Commit the changes
        conn.commit()

        # 5. Report the number of rows modified
        print("Total number of rows updated:", cur.rowcount)
        
        cur.close()

    except (Exception, psycopg2.DatabaseError) as error:
        print(error)
        if conn is not None:
            conn.rollback()
    finally:
        if conn is not None:
            conn.close()

# Function call to update employee 110's department to "Finance"
updateRecord(110, "Finance")
```



---

## **5. Python Web Frameworks for PostgreSQL**

Python offers various frameworks for web and internet development:
*   **Micro-frameworks**: Flask, Bottle.
*   **Full-stack Frameworks**: Django, Pyramid.
*   **Advanced CMS**: Plone, django CMS.

### **5.1. Flask Micro-framework**
*   **Definition**: A lightweight WSGI (Web Server Gateway Interface) web application framework.
*   **Key components**:
    *   `Flask` class: The WSGI application object.
    *   `route()` function: A decorator that tells the application which URL should call the associated function.
    *   `run()` method: Runs the application on the local development server.

---

## **6. Case Study: Integrated Web Application with Flask**

This example demonstrates a "Candidate Email Database" application with a `Candidate` table.

### **6.1. Navigation and Rendering**
The `index.html` template provides links to add and view emails. Flask uses `render_template` to serve these pages:
```python
@app.route("/")
def index():
    return render_template("index.html")

@app.route("/add")
def add():
    return render_template("add.html")
```

<img width="402" height="295" alt="image" src="https://github.com/user-attachments/assets/01931e62-f2fc-4def-b91a-2dc3c90d2b1e" />

### **6.2. Adding Data (`add.html` and `saveDetails`)**
The HTML form uses `method="post"` and targets the `/savedetails` route. The Python function extracts form data via `request.form` and executes an `INSERT` statement:
```python
@app.route("/savedetails", methods = ["POST"])
def saveDetails():
    cno = request.form["cno"]
    name = request.form["name"]
    email = request.form["email"]
    # ... connection and cursor setup ...
    cur.execute("INSERT INTO Candidate (cno, name, email) VALUES (%s, %s, %s)", (cno, name, email))
    conn.commit()
    return render_template("success.html")
```

### **6.3. Viewing Data (`viewAll` and `viewall.html`)**
The application retrieves all rows and passes them to a Jinja2 template for rendering in a table:
```python
@app.route("/viewall")
def viewAll():
    # ... connection and cursor setup ...
    cur.execute("SELECT cno, name, email FROM Candidate")
    results = cur.fetchall()
    return render_template("viewall.html", rows=results)
```

**Template Snippet (`viewall.html`):**
```html
{% for row in rows %}
<tr>
    <td>{{row}}</td> <td>{{row}}</td> <td>{{row}}</td>
</tr>
{% endfor %}
```




---





# **7.5: Application Design and Development/5: Application Development and Mobile**



## **1. Objectives**
*   To explore the **Rapid Application Development (RAD)** process.
*   To understand the issues in **Application Performance and Security**.
*   To understand the similarities and differences between **Mobile Apps and Web Applications**.




## **2. Rapid Application Development (RAD)**

### **2.1 Definition and Approach**
*   A lot of effort is required to develop Web application interfaces, especially the rich interaction functionality associated with Web 2.0 applications.
*   **RAD Software** is an agile model that focuses on fast prototyping and quick feedback in app development to ensure speedier delivery and an efficient result.
*   With RAD, the time between prototypes and iterations is short, and integration occurs since inception.
*   It is often called a **"customer-in-the-loop"** kind of development.

### **2.2 App Development Phases**
Application development typically consists of 4 phases:
1.  **Business Modeling:** Defining requirements and what the business needs to satisfy.
2.  **Data Modeling:** Modeling items (books, appliances, etc.) to be stored.
3.  **Process Modeling:** Modeling workflows and sequence of steps.
4.  **Testing & Turnover:** Prototyping, receiving feedback, and finalizing the software.

### **2.3 Approaches to Speed up Development**
*   **Function libraries** to generate user-interface elements.
*   **Drag-and-drop features** in an IDE (e.g., Visual Studio) to create UI elements.
*   **Automatic code generation** for user interfaces from declarative specifications.

### **2.4 RAD Frameworks and Platforms**
*   **Java Server Faces (JSF):** A set of APIs for representing UI components, managing their state, handling events, input validation, and supporting internationalization and accessibility.
*   **Ruby on Rails:** Allows easy creation of simple CRUD (create, read, update, and delete) interfaces by code generation from a database schema or object model.
*   **ASP.NET and Visual Studio:** Provides a variety of controls interpreted at the server to generate HTML; supports drag-and-drop development and a **DataGrid** for displaying SQL results.
*   **RAD Platforms:** G Suite, Google App Engine, Microsoft Azure, Amazon Elastic Compute Cloud (EC2), and AWS Elastic Beanstalk.

---

## **3. Application Performance and Security**

### **3.1 Application Performance**
*   Performance is a major issue for popular Web sites that may be accessed by millions of users with thousands of requests per second at peak time.
*   **Caching techniques** are used to reduce the cost of serving pages by exploiting similarities between requests:
    *   **Connection Pooling:** Caching of JDBC connections between servlet requests.
    *   **Query Result Caching:** Caching results of database queries (must be updated if underlying data changes).
    *   **HTML Caching:** Caching of generated HTML pages.

### **3.2 Application Security: SQL Injection**
*   Occurs when a query is constructed using string concatenation, such as:
    `"select * from instructor where name = '" + name + "'"`.
*   If a user enters `X' or 'Y'='Y`, the statement becomes:
    `"select * from instructor where name = 'X' or 'Y'='Y'"`.
*   Because `'Y'='Y'` is always true, the query returns all instructors, bypassing intended filters.

### **3.3 Additional Security Threats**
1.  **Password Leakage:** Never store database passwords in clear text in scripts. Editor backup files (e.g., `file.jsp~`) may accidentally be served by web servers.
2.  **Authentication:** Password-based systems are weak due to reuse across sites and spyware. **Two-factor authentication (2FA)** adds security via one-time password (OTP) devices or SMS.
3.  **Application-Level Authorization:**
    *   SQL standards are often too coarse (table/column level) for fine-grained needs (e.g., "students see only their own grades").
    *   **Workaround:** Use views with `syscontext.user_id()` to identify the end user.
    *   **Oracle Virtual Private Database (VPD):** Transparently adds predicates (e.g., `WHERE ID = sys_context.user_id()`) to all queries to enforce row-level authorization.
4.  **Audit Trails:** Applications must log actions to detect security breaches, repair damage, and trace perpetrators. Trails are needed at both the database and application levels.

---

## **4. Mobile Applications**

### **4.1 Definition and Characteristics**
*   A type of application software designed specifically for small, wireless computing devices like smartphones and tablets.
*   **Device Constraints:** Limited memory, limited computing power, limited battery power, and limited bandwidth.
*   **Specialized Capabilities:** Mobiles provide sensors like **accelerometers** (to track speed), gravitational sensors (for screen rotation), and touchscreens for **gesture-based navigation**.

### **4.2 Mobile Website vs. Mobile App**
*   **Mobile Website:** Consists of browser-based HTML pages designed for small handheld displays and touchscreens; accessed over WiFi/3G/4G.
*   **Mobile App:** Actual applications downloaded and installed via device-specific portals (App Store, Google Play). Apps can download content for offline access.

### **4.3 Types of Mobile Apps**
*   **Native Apps:** Written in the platform's native language (iOS: Objective-C; Android: Java or C/C++). They are platform-specific and OS-dependent.
*   **Web Apps:** Run completely inside a Web browser using HTML, CSS, and languages like JavaScript or Ruby on Rails. They are portable across devices.
*   **Hybrid Apps:** Combine attributes of both native and web apps, attempting to use common redundant code while tailoring specific attributes to the native system.

### **4.4 Architecture of Mobile Apps**
*   Typically follows a **3-tier architecture**: Presentation, Business, and Data.
*   The **Data Layer** is often split between:
    *   **Local Data:** Small, cached database for quick turnaround and offline use.
    *   **Remote Data:** Bulk data stored on remote services, which is more expensive to access.

<img width="1062" height="501" alt="image" src="https://github.com/user-attachments/assets/612b86f4-f620-4efd-91a0-b418db2b1912" />
