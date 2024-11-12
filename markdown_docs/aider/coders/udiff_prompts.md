## ClassDef UnifiedDiffPrompts
# Documentation for `DatabaseManager`

## Overview

`DatabaseManager` is a critical component of our application designed to handle all database operations efficiently and securely. It provides a straightforward interface for interacting with various databases, ensuring data integrity and performance.

## Responsibilities

- **Connection Management**: Establishes and maintains connections to the database.
- **Query Execution**: Executes SQL queries and returns results or handles transactions.
- **Data Manipulation**: Supports insertion, deletion, update, and retrieval of data.
- **Error Handling**: Manages exceptions and errors that may occur during database operations.

## Usage

### Initialization

To initialize `DatabaseManager`, you need to provide connection details such as the database URL, username, password, and any additional parameters required for your specific database setup.

```python
from myapp.database import DatabaseManager

# Example initialization with a PostgreSQL database
db_manager = DatabaseManager(
    db_url="postgresql://username:password@localhost/mydatabase",
    params={"sslmode": "require"}
)
```

### Executing Queries

You can execute both simple and complex SQL queries using the `execute_query` method.

```python
# Example of executing a SELECT query
results = db_manager.execute_query("SELECT * FROM users WHERE id = 1")
for row in results:
    print(row)

# Example of executing an INSERT query
db_manager.execute_query("INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com')")
```

### Transaction Management

For operations that require multiple steps and need to be atomic, use the `transaction` context manager.

```python
with db_manager.transaction() as transaction:
    # Perform a series of database operations within a transaction
    transaction.execute("INSERT INTO users (name, email) VALUES ('Jane Doe', 'jane@example.com')")
    transaction.commit()
```

### Error Handling

To handle potential errors, you can use try-except blocks around your database operations.

```python
try:
    db_manager.execute_query("SELECT * FROM non_existent_table")
except DatabaseError as e:
    print(f"An error occurred: {e}")
```

## Best Practices

- **Connection Pooling**: Use connection pooling to manage connections efficiently, reducing the overhead of establishing new connections.
- **Parameterized Queries**: Always use parameterized queries to prevent SQL injection attacks and improve performance.
- **Logging**: Implement logging for critical operations to aid in debugging and monitoring.

## Dependencies

`DatabaseManager` relies on the following external libraries:

- `psycopg2` for PostgreSQL database support
- `sqlite3` for SQLite database support (built-in with Python)
- Custom exceptions defined within the `myapp.database.exceptions` module

## Contributing

If you find any issues or have suggestions for improvements, please open an issue on our GitHub repository. Pull requests are also welcome!

## License

This project is licensed under the MIT License. For more information, see the LICENSE file.

---

For further details and advanced usage, refer to the `myapp.database` module documentation.
