# 📦 Neo4j Database Singleton (Python)

This project provides a simple implementation of a **Singleton pattern** for managing a Neo4j database connection in Python using the official driver.

## 🚀 Overview

The goal of this module is to ensure that:

- Only **one database connection** is created and reused
- Multiple calls return the **same instance**
- Connection management is centralized and controlled

---

## 🧠 Concept

This implementation uses:

- A **class-level static variable** (`db_driver`)
- A **classmethod** to control access to the database connection
- A safeguard in `__init__` to prevent multiple instantiations

---

## 🛠️ Requirements

- Python 3.x
- Neo4j Python Driver

Install the dependency:

```bash
pip install neo4j
```

---

## ▶️ Usage

### Initialize the connection (once)

```python
from database import Database

db = Database.getConnection("bolt://localhost:7687", "test_user", "root")
```

---

### Reuse the connection anywhere

```python
from database import Database

db = Database.getConnection()
```

---

### Verify Singleton behavior

```python
db1 = Database.getConnection("bolt://localhost:7687", "test_user", "root")
db2 = Database.getConnection()

print(db1 is db2)  # True
```

---

### Execute a query

```python
from database import Database

driver = Database.getConnection()

with driver.session() as session:
    result = session.run("MATCH (n) RETURN n LIMIT 5")
    
    for record in result:
        print(record)
```

---

### Close the connection

```python
driver = Database.getConnection()
driver.close()
```

---

## ⚙️ How It Works

1. The first call to `getConnection()`:
   - Creates a new Neo4j driver instance
   - Stores it in `db_driver`

2. Subsequent calls:
   - Return the existing instance
   - Ignore new parameters (if provided)

---

## ⚠️ Limitations

- ❗ No connection validation (does not check if the connection is still alive)
- ❗ No support for multiple databases or environments
- ❗ Thread safety is not guaranteed
- ❗ Credentials cannot be updated after initialization

---

## 💡 Possible Improvements

- Add **connection health checks**
- Support **multiple environments (dev/staging/prod)**
- Implement **thread-safe Singleton**
- Add **connection pooling management**
- Use **context managers** for sessions

---

## 🧪 Example Use Case

Useful in:

- Small applications
- Scripts
- Prototypes
- Services where only one DB connection is needed

---

## 📄 License

MIT License
