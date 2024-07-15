This is a common programming technique for interacting between **application code and databases.** Instead of using raw tables, SQL queries, etc., you use **high level, object-oriented abstractions** for defining and interacting with tables. This is used in Python, Java, C#, etc.

**Each class represents a table**. The attributes of the class then represent the columns. Instances of the class then represent actual rows and data in the table. 

For queries, **SQL queries are abstracted away, making it more object-oriented in style and without writing raw SQL queries.**

The main advantages over writing raw SQL queries and sending them to the database is: 
- Abstract database complexities
- Work with object-oriented programing
- Database-agnostic, not tied down to a specific SQL provider
- Simplify relationships between tables
- **Easier to run and store query results without having to manually convert to/from objects. Removes tons of boilerplate.**

The alternative is using a specific database client library, like `psycopg2` for PostgreSQL in Python. You then run actual SQL queries against the database using the client library. In general, unless you are struggling with performance, ORMs are better. 

**However,** ORMs are considered a bit of an anti-pattern. It makes the easy bits easier and the hard bits harder. It also requires you to learn the ORM-specifics, instead of just using standard SQL. 

SQLAlchemy is considered one of the best. 

Many frameworks like Flask, Django, Ruby on Rails, etc. have ORMs. 

### Raw SQL
If you use raw SQL, you will need to create a data mapper with CRUD features. ORMs help by eliminating all of that boilerplate for you. 
However, it is still critical to still know all of the SQL stuff - ORMs are tools to help efficiency, not have a poor knowledge of SQL. 
There are also occasions where raw SQL is better. 
ORMs help you get up and running faster. D