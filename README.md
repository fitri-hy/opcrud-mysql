This documentation explains the use of the CRUD (Create, Read, Update, Delete) API built using Express.js to manage data in a database. This API is designed to make it easier for developers to perform basic operations on the database via HTTP requests, allowing for more efficient and structured data management.

**This API supports a variety of operations, including**:

- Creating new records in a database table.
- Reading data from a table, including the ability to count the total records, search for data based on certain criteria, and get distinct (distinct) values.
- Updating existing data based on ID.
- Deleting records or tables that are no longer needed.

**Key Features**

- CRUD Operations: Supports basic operations for data management, including creating, reading, updating, and deleting data.
- Batch Operations: printing multiple records in a single request for greater efficiency.
- Pagination: Organizing data in pages to make it easier to navigate through large results.
- Search & Filter: Makes it easier to search data based on certain columns and search terms.
- Grouping: Supports grouping data based on certain columns with aggregation functions.
- Detail Joins: retrieves record details by combining data from multiple tables.
- Retrieve Allowed Table Names: Retrieves permitted table names from the database to ensure that operations are performed on valid tables.
- Table Name Validation: Validates table names before performing operations to ensure that the given table name is valid and does not cause errors in the database.

**Prerequisites**

Before using this API, make sure you have Node.js installed and have access to the database you want to manage. You will also need to adjust the database connection parameters in the code to match your system settings.

This API is designed for use in applications that require flexible and efficient data management. Please refer to the remaining sections of this documentation for more information on how to use each API endpoint.

# Choose How to Use:
- <a href="./README-MYSQL.md">OpCrud/MySQL Version</a>
- <a href="./README-JSON.md">OpCrud/Json Version</a>