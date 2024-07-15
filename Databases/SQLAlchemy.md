This is the go-to Python library for interacting with relational databases in SQL, supporting PostgreSQL, MySQL, SQLite and Oracle. It is **database-agnostic**. 

It uses an [[ORM (Object Relational Mapping)]]. 

### Engine
An engine is the component that interfaces between Python and the relational database. It's basically responsible for parsing and translating Python to SQL, just like how a JavaScript engine is responsible for parsing and executing JavaScript. 

An engine is always tied to a real database - when initializing an engine, you connect it to a database URL. 

Using the engine, you then execute queries. **The first step is always to create the engine / establish the connection.**
```python
from sqlalchemy import create_engine
engine = create_engine('sqlite:///mydatabase.db', echo=True)
result = engine.execute("SELECT * FROM mytable")
```

The engine manages a pool of connections, so you don't lose performance by establishing new connections. 

### Session
A session is a transactional context in which you interact with the database, just like a banking session. It is for a single unit of work. Basically, **a session gives you a transaction scope, so everything you do in the session is considered part of the same transaction.**

Changes made within the session are not immediately reflected - they are accumulated until you **commit the changes**. 

Multiple sessions can work concurrently without interfering. Each session uses a different connection from the connection pool. 

Having multiple transactions within a single session is okay, but ideally it should be a single atomic unit. 

If you have multiple transactions in a single commit, you have a **chained commit** or a **multi-commit**. However, this is not recommended, so you keep your transactions atomic. 

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)
session = Session()

new_user = User(username='john_doe', email='john@example.com') session.add(new_user) 
session.commit()
session.close()
```

The typical pattern is:
1. Try
	1. Create session
	2. Perform database operations
	3. Commit the transaction
2. Except
	1. Handle any errors and rollback if necessary 
3. Finally
	1. Close the session

In modern Python, we like to use context managers: 
```python
# create session and add objects
with Session(engine) as session, session.begin():
    session.add(some_object)
    session.add(some_other_object)
# inner context calls session.commit(), if there were no exceptions
# outer context calls session.close()
```

Or a custom one: 
```python
@contextmanager
def get_session():
    session = get_the_session_one_way_or_another()

    try:
        yield session
    except:
        session.rollback()
        raise
    else:
        session.commit()
```

### Base
A **declarative model** is a class that represents a database table (in a declarative, high-level way). 
The base is used as a based class for all declarative models to inherit useful stuff. 
Also, it means you can **create the tables** after declaring them with the `create_all` method. 
After that, you use the classes as a basis for SQL queries. 
The attributes in the classes **all represent columns in the table.**

```python
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'  # Name of the database table

    id = Column(Integer, primary_key=True)
    username = Column(String(50), unique=True, nullable=False)
    email = Column(String(100), unique=True, nullable=False)

Base.metadata.create_all(engine)

# Now, query
new_user = User(username='john_doe', email='john@example.com') session.add(new_user)
```
### Core & ORM
SQLAlchemy comes with two parts: core & ORM that operate at different levels of abstraction. The core is lower level while the [[ORM (Object Relational Mapping)]] is higher level.

```python
# Core
users = Table('users', metadata,
    Column('id', Integer, primary_key=True),
    Column('username', String(50), unique=True, nullable=False)
)
insert_statement = insert(users).values(username='john_doe')
with engine.connect() as conn: 
	conn.execute(insert_statement)

# ORM
new_user = User(username='john_doe') 
session.add(new_user)
```
### Core
The 'core' of SQLAlchemy gives you 
- Low-level SQL expression language: execute raw SQL queries
- Provides core objects like tables, columns, etc.
- Provides Pythonic way of doing SQL queries

```python
from sqlalchemy import create_engine, text

engine = create_engine('sqlite:///mydatabase.db')

# Using SQLAlchemy Core to execute a raw SQL query
with engine.connect() as conn:
    query = text("SELECT username FROM users WHERE id=:user_id")
    result = conn.execute(query, user_id=1)
    username = result.scalar()
```

### ORM
This is the typical way of using SQLAlchemy & leverages ORM capabilities. It **builds on and extends the core** and uses many parts of it. 
- High-level ORM 
- Declarative models with base
- Object oriented queries
- 