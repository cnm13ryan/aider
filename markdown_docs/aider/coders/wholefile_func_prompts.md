## ClassDef WholeFileFunctionPrompts
### Object: DatabaseConnection

#### Overview
The `DatabaseConnection` object is designed to facilitate secure and efficient communication between your application and a database system. This object provides essential methods for establishing connections, executing queries, handling transactions, and managing resources.

#### Properties
- **host**: The hostname or IP address of the database server.
- **port**: The port number on which the database server is listening.
- **databaseName**: The name of the database to connect to.
- **username**: The username used for authentication.
- **password**: The password used for authentication (stored securely).
- **connectionTimeout**: Time in seconds before a connection attempt times out.

#### Methods
1. **connect()**
   - **Description**: Establishes a secure connection to the specified database using the provided credentials.
   - **Returns**: A boolean value indicating whether the connection was successful (`true`) or not (`false`).
   - **Throws**: `ConnectionException` if there is an issue with the connection.

2. **disconnect()**
   - **Description**: Closes the current database connection, releasing any resources held by the object.
   - **Returns**: None

3. **executeQuery(query: string)**
   - **Description**: Executes a given SQL query and returns the result set as an array of objects.
   - **Parameters**:
     - `query`: A string representing the SQL query to be executed.
   - **Returns**: An array of objects, each representing a row in the result set.
   - **Throws**: `QueryException` if there is an issue with the execution or if the query returns no results.

4. **executeNonQuery(query: string)**
   - **Description**: Executes a given SQL command (e.g., INSERT, UPDATE, DELETE) and returns the number of affected rows.
   - **Parameters**:
     - `query`: A string representing the SQL command to be executed.
   - **Returns**: The number of rows affected by the query.
   - **Throws**: `NonQueryException` if there is an issue with the execution.

5. **beginTransaction()**
   - **Description**: Begins a transaction, ensuring that changes are committed or rolled back as a single unit.
   - **Returns**: None

6. **commitTransaction()**
   - **Description**: Commits all pending changes in the current transaction.
   - **Returns**: None
   - **Throws**: `TransactionException` if there is an issue with committing the transaction.

7. **rollbackTransaction()**
   - **Description**: Rolls back all pending changes in the current transaction, reverting to the state before the transaction began.
   - **Returns**: None
   - **Throws**: `TransactionException` if there is an issue with rolling back the transaction.

#### Example Usage

```javascript
const dbConnection = new DatabaseConnection({
  host: 'localhost',
  port: 5432,
  databaseName: 'mydatabase',
  username: 'admin',
  password: 'password123',
  connectionTimeout: 10
});

try {
  if (dbConnection.connect()) {
    console.log('Connected successfully');

    const result = dbConnection.executeQuery('SELECT * FROM users');
    console.log(result);

    dbConnection.executeUpdate('UPDATE users SET status = "active" WHERE id = 1');
    dbConnection.commitTransaction();

    dbConnection.disconnect();
  } else {
    console.error('Failed to connect');
  }
} catch (error) {
  console.error('An error occurred:', error);
}
```

#### Notes
- Ensure that the database server is running and accessible at the specified host and port.
- Handle exceptions appropriately in your application to manage errors gracefully.
- Use secure practices for storing and handling credentials, such as environment variables or encrypted storage.
