# JDBC (Java Database Connectivity)

JDBC (Java Database Connectivity) is an **API (Application Programming Interface)** provided by Java to enable Java applications to interact with a variety of databases. It provides a standard interface for accessing and manipulating database data using the Java programming language. JDBC facilitates database connectivity, allowing applications to execute SQL queries and manage data in a **platform-independent manner**. It is a part of the **Java Development Kit (JDK)**.

![](https://github.com/adarshpandey18/notes/blob/main/images/JDBC_Introduction.png)

## JDBC Steps

### 1. Import Packages

Import necessary packages for working with JDBC:

```java
import java.sql.*;
```

### 2. Load Driver

Load the database driver class.

### 3. Register Driver

Register the driver with `DriverManager` (handled automatically in JDBC 4 and later).

### 4. Create Connection

Establish a connection to the database:

```java
Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "username", "password");

```

### 5. Create Statement

Create a `Statement` or `PreparedStatement` object to execute SQL queries:

```java
Statement statement = connection.createStatement();

```

### 6. Execute Statement

Execute SQL queries using the `Statement` object:

```java
ResultSet resultSet = statement.executeQuery("SELECT * FROM users");

```

### 7. Close

Close the `ResultSet`, `Statement`, and `Connection` to release resources:

```java
resultSet.close();
statement.close();
connection.close();
```

> **Note**: Registering and loading the driver were compulsory before JDBC 4. With JDBC 4 and later, the driver registration is done automatically by the `DriverManager`.

## Code 

```java
import java.sql.*;

public class Main {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // Loading and registering the MySQL JDBC driver
        Class.forName("com.mysql.cj.jdbc.Driver"); // This is good practice for compatibility

        // Create Connection
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/University", "root", "9969");
        /*
            The connection URL format:
            jdbc:mysql://localhost:3306/University
            Where:
                - jdbc: indicates it's a JDBC connection
                - mysql: specifies the MySQL database
                - localhost: the database server is on the same machine
                - 3306: the default MySQL port
                - University: the database name
         */

        // Create Statement
        Statement st = con.createStatement();
        String query = "SELECT * FROM Student";
        ResultSet rs = st.executeQuery(query);  // Using executeQuery to fetch data

        // Loop through the result set and print the Student_Name
        while (rs.next()) {
            System.out.println(rs.getString("Student_Name"));
        }

        // Close the connection
        rs.close();
        st.close();
        con.close();
    }
}
```
- `Class.forName()`: The `Class.forName("com.mysql.cj.jdbc.Driver")` loads the MySQL JDBC driver class dynamically during runtime, enabling the program to interact with the MySQL database.

- `DriverManager`:It is a utility class in JDBC that manages a list of database drivers and is responsible for selecting the appropriate driver based on the database URL.

-  `Connection`: Represents the database connection. It is used to create `Statement` or `PreparedStatement`, manage transactions, and execute queries.

-   `Statement`: Used to execute static SQL queries without parameters. It is vulnerable to SQL injection.
    
-   `PreparedStatement`: Used to execute SQL queries with parameters (placeholders). It helps prevent SQL injection and improves performance by precompiling queries.
## Problems with  `Statement` in JDBC:

1.  **Concatenation of Variables**:
    
    -   Tedious and error-prone.
    -   Example:
        
        ```java
        String query = "SELECT * FROM Student WHERE Student_Name = '" + name + "'";
        
        ```
        
    -   Becomes hard to maintain with more variables.
2.  **SQL Injection & Data Leakage**:
    
    -   Direct concatenation opens doors for SQL Injection.
    -   Example:
        
        ```java
        String name = "' OR '1'='1";  // Malicious input
        
        ```
        
    -   Query becomes:
        
        ```sql
        SELECT * FROM Student WHERE Student_Name = '' OR '1'='1';
        
        ```
        
3.  **Performance Issues**:
    
    -   Recompiles the query each time, reducing efficiency.
    -   Example:
        
        ```java
        stmt.executeQuery("UPDATE Student SET Age = " + i + " WHERE Student_ID = " + i);
        
        ```
        

----------

##   `PreparedStatement`:

1.  **Avoid Concatenation**:
    
    -   Use placeholders `?` instead of concatenation.
    -   Example:
        
        ```java
        String query = "SELECT * FROM Student WHERE Student_Name = ?";
        PreparedStatement pstmt = con.prepareStatement(query);
        pstmt.setString(1, name);
        
        ```
        
2.  **Prevents SQL Injection**:
    
    -   Parameterized queries safely handle inputs, avoiding injection.
3.  **Improved Performance**:
    
    -   The query is precompiled, so repeated executions are faster.
    
## CRUD Code
```java
import java.sql.*;

public class JdbcCrudOperations {
    public static void main(String[] args) {
        String jdbcUrl = "jdbc:mysql://localhost:3306/University";
        String username = "root";
        String password = "9969";
        
        try (Connection con = DriverManager.getConnection(jdbcUrl, username, password)) {
            // Create Operation
            createRecord(con);
            
            // Read Operation
            readRecords(con);
            
            // Update Operation
            updateRecord(con);
            
            // Delete Operation
            deleteRecord(con);

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Create Operation (Insert a new record)
    private static void createRecord(Connection con) throws SQLException {
        String insertQuery = "INSERT INTO Student (Student_Name, Age, Course) VALUES (?, ?, ?)";
        try (PreparedStatement pstmt = con.prepareStatement(insertQuery)) {
            pstmt.setString(1, "John Doe");
            pstmt.setInt(2, 22);
            pstmt.setString(3, "Computer Science");
            int rowsAffected = pstmt.executeUpdate();
            System.out.println(rowsAffected + " row(s) inserted.");
        }
    }

    // Read Operation (Fetch records from the database)
    private static void readRecords(Connection con) throws SQLException {
        String selectQuery = "SELECT * FROM Student";
        try (Statement stmt = con.createStatement();
             ResultSet rs = stmt.executeQuery(selectQuery)) {
            while (rs.next()) {
                System.out.println("Student ID: " + rs.getInt("Student_ID") +
                                   ", Name: " + rs.getString("Student_Name") +
                                   ", Age: " + rs.getInt("Age") +
                                   ", Course: " + rs.getString("Course"));
            }
        }
    }

    // Update Operation (Modify an existing record)
    private static void updateRecord(Connection con) throws SQLException {
        String updateQuery = "UPDATE Student SET Age = ?, Course = ? WHERE Student_ID = ?";
        try (PreparedStatement pstmt = con.prepareStatement(updateQuery)) {
            pstmt.setInt(1, 23);  // Update Age
            pstmt.setString(2, "Software Engineering");  // Update Course
            pstmt.setInt(3, 1);  // Assuming Student_ID = 1
            int rowsAffected = pstmt.executeUpdate();
            System.out.println(rowsAffected + " row(s) updated.");
        }
    }

    // Delete Operation (Remove a record)
    private static void deleteRecord(Connection con) throws SQLException {
        String deleteQuery = "DELETE FROM Student WHERE Student_ID = ?";
        try (PreparedStatement pstmt = con.prepareStatement(deleteQuery)) {
            pstmt.setInt(1, 1);  // Assuming Student_ID = 1
            int rowsAffected = pstmt.executeUpdate();
            System.out.println(rowsAffected + " row(s) deleted.");
        }
    }
}

```

