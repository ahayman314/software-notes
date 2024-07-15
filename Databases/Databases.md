Here, we start with an overview of databases.
### Goal
What the goal of databases? Where are they implemented? What problem are we trying to solve?
How do we ensure consistency, efficiency, redundancy, integrity, atomic updates, isolation, concurrency & security with databases?

### Definitions
- Database-management system (DBMS): collection of interrelated data and a set of programs to access it for convenience and efficiency
- Database: collection of data
- File processing system: store data in a bunch of files and write programs to read/write/update them
- Data model: way of describing data, relationships, semantics and constraints. It models the relevant aspects of reality like entities, relationships, constraints, etc.
- Relational model: data stored in tables with columns as attributes and rows as entries. A table is called a relation
- Data view: level at which you are viewing the database system
- Logical schema: overall logical structure of the database - what information is present in the database?
- Physical schema: physical structure of the stored data
- Physical Data Independence: modify physical schema without changing logical schema. Goal of abstraction from different view levels.
- Instance: content of database at point in time (like the value of a variable at a given moment)
- Schema: overall design of the database (like a variable declaration)

- **Data Definition Language (DDL):** notation for defining schema like 'create table instructor (ID char(5)...'
- Data dictionary: contains metadata including database schema, integrity constraints, authorization
- **Data Manipulation Language (DML) / Query Language:** language to access and update data

- Pure: relational algebra - used for proofs
- Commercial: SQL
- Procedural: user specifies what data is needed + how to get it
- Declarative / Non-procedural: user specifies what data is needed (SQL)

- Query Language: Part of DML involving information retrieval. Basically the same as DML
- Query: statement requesting retrieval of information
- SQL: Structure Query Language is declarative and usually embedded in high-level language. Apps usually access databases through language extensions (i.e. Python) or API to query database

- Application program: programs designed to interact with database through things like higher level languages
- Logical design: database schema based on business decisions (what to record) and CS decisions (what relations)
- Physical design: low-level data structures storing the data
- Database design: mainly about the design of the database schema based on user needs
- Functional requirements: the operations users want to perform

- Database engine: drives the database system with different modules (storage manager, query processor, transaction management, etc.)
- Storage manager: interface between low-level data stored and application programs that submit queries. Interacts with OS file manager, ensures efficient queries & includes components like authorization & integrity manager, transaction manager, file manager & buffer manager.
- Data files: part of storage manager that stores the database itself
- Data dictionary: stores metadata about the structure of the database, mainly schemas
- Indices: fast access to data items via pointers
- Query processor: includes DDL interpreter, DML compiler & query evaluation engine (executes compiled queries)
- Transaction: collection of operations that performs a single unit of work (example: bank transaction)
- Atomicity: single transaction definition that happens entirely, or not at all
- Transaction management ensures consistent states despite system failures & transaction failures
- Concurrency control manager ensures consistency during concurrent transactions
- Tier:

- 2-tier: app resides at client machine & invokes db system functionality on the server
- 3-tier: client doesn't do database calls - inputs to forms on server which in turn communicate with db server

- Users:

- Naive: use apps (end users)
- Application programmers: write apps
- Sophisticated users: use query language
- Specialized: specialized apps like graphic data

- Database Administrator (DBA): controls schema definition, storage structure, permissions, maintenance, backups, etc.

### File Processing System
In the early days, we had files with data and programs to manage those files. This was bad:

- Data redundancy and inconsistency: multiple file formats and duplication of information across files
- Access difficulty: new program for carrying out each task
- Data isolation: multiple files and formats
- Integrity issues: constraints become part of program code instead of being clearly stated. Difficult to change or add new constraints
- Atomicity of updates: failures could leave inconsistent states with partial updates
- Concurrency: leads to inconsistencies
- Security: hard to provide access to the right users

This is the motivation for a database management system.

### Data Model Types
- Relational model
- Entity-Relationship data model (for design)
- Object-based data model (OO)
- Semi-structured data model (XML)
- etc.

### Database Views & Abstraction
- Physical level: the DS and algos. that power the system
- Logical level: what the programmer sees?
- View level: what the user sees on a website or something

Like operating systems, we want to abstract away & give the impression of simplicity.

### History
- 1950's & 1960's: magnetic tapes for storage with sequential access
- 1960's & 1970's: hard disks allow direct data access. Ted Codd defines relational data model
- 1980's: SQL becomes industry standard. Parallel and distributed database systems
- 1990's: Terabyte data warehouses, web commerce, data mining
- 2000's: big data storage systems, NoSQL systems, XMLL, JSON, MySQL, PostgreSQL
- 2010's: massively parallel, multi-core main-memory databases

### Examples
Present in all parts of life:

- Sales: customer, product, purchase information
- Accounting: payments, receipts, account balances, assets
- HR: employees, salaries, payroll, taxes, benefits, paychecks
- Manufacturing: tracking production
- Banking: customer info, transactions, accounts, loans
- Finance: holdings, sales, stocks, bonds
- Universities: courses, students, professors, grades
- Airlines: reservation and schedule information
- Telecomm: calls, texts, data usage, bills
- Social media: users, posts, ratings
- Online retailers: sales, product views, search history
- Navigation systems: places of interest, routes, buses

Two broad applications:

- Online transaction processing: users interacting with websites
- Data analytics: storing data to analyze