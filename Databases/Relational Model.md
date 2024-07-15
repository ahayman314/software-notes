In [[Databases]], we have the relational model which is a structure for describing related data - this is just a **model** of our data structured as tables. To interact with a relational model, we **query** the relational data with SQL.
### Relational Model Structure
- Table/relation: a table is a relation with a set of unique tuples. **A relation is a table.** Named after the mathematical concept of relation (how sets of numbers are related). Set of unique tuples.
	- Relation/table schema: logical structure of table/relation, defining attributes & their domains.  Example schema is **student(ID, name, residence, year)** like `int x;` while the tuples/rows are the values.
	- Table heading: schema composed of attributes & domains (columns)
	- Table body: set of tuples (rows)
	- Relation/table **instance**: the set of tuples current in the table/relation
- Tuple/row: a tuple is a row, or entry of data in the table. It is a relationship between a set of values. Mathematically, a tuple is a sequence (list) of values. For n-values in a tuple, we have an n-tuple of values.
	- Tuple ordering: tuples are unordered in the table/relation.
	- Cardinality: set/number of tuples. 5 tuples/5 rows means a cardinality of 5
- Attribute/column: an attribute is a column in the table
	- Attribute domain: Set of allowed values for the attribute (like domain on x-axis in graph)
	- Atomic domain: the attribute's domain needs to be composed of atomic, indivisible units (i.e. list of courses you take is not atomic - each individual course is)
	- Arity: set/number of attributes. 5 attributes means an arity of 5
- Database:
	- Database schema: logical structure of the database. Collection of individual relation/table schemas. **A database schema is composed of table schemas**
	- Database instance: snapshot of data in the database at a given moment. Collection of individual relation/table instances.

Other important aspects: 
- Null value: allowed in any domain & means unknown value or does not exist
### Keys
We need a way to **distinguish** tuples in a table.

- Superkey: any list of attribute(s) that uniquely identify tuple. (year, month, date, major, minor) or (year, major, minor)
- Candidate key: smallest set of superkeys. There can be multiple candidate keys available to choose for a primary key (i.e. imagine 3 unique ID columns). (year, major, minor)
- Primary key: A list of 1+ attributes that is a candidate key chosen by designer as primary way of identifying tuple. **Not necessarily a single attribute - a candidate key can have multiple attributes.**
	- Primary key constraints: another way of referring to primary keys, mainly in terms of how it constrains the addition of new tuples to have a unique primary key.
- Foreign key: a list of 1+ of attributes in one relation that reference the primary keys of another relation
	- Foreign-key constraints: the values of the attributes referencing another relation must be present in that relation in its primary key
	- Referencing relation: the one defining the foreign key
	- Referenced relation: the one getting 'called'. Referenced attribute must be the primary key
	- Referential integrity constraint: same as foreign-key constraint but referenced attributes can be any attributes (not just a primary key)

### Goal
- How can we sensibly describe related data? How do to it avoiding duplications?
- Once we can sensibly describe it, how can we efficiently query it?
- How do we define a system that achieves data independence, that is, independent from the low-level implementation? The goal again is to abstract way the physical view and leave it as a design decision. By defining a high-level model, we can leave it to the database engineers to decide how to physically store it in a database management system.
By formalizing operations in algebra, we can leave it to an underlying compiler to optimize the operations.
By defining data in relations, we get a high-level description of data, and leave all of the implementation to other engineers. We can then define operations using high-level relational algebra, which helps arguments on optimization and soundness.
### Resources
Relational Algebra Practice: [https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/tutorials/tut1-ra.pdf](https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/tutorials/tut1-ra.pdf)
More relational algebra examples: [https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/lectures/03-ra.pdf](https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/lectures/03-ra.pdf)
Section 4: [https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/tutorials/tut2-ra.pdf](https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/tutorials/tut2-ra.pdf)
Relational algebra calculator: [https://dbis-uibk.github.io/relax/landing](https://dbis-uibk.github.io/relax/landing)
Assignment: [https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/assignments/a1/a1.pdf](https://www.eecs.yorku.ca/~papaggel/courses/eecs3421/docs/assignments/a1/a1.pdf)
Practice: [https://web.cs.dal.ca/~baig/6220/Test-Solution](https://web.cs.dal.ca/~baig/6220/Test-Solution)
Assignment: [https://www.cs.toronto.edu/~faye/343/f07/handouts/Assignment1v1.pdf](https://www.cs.toronto.edu/~faye/343/f07/handouts/Assignment1v1.pdf)

### Definitions
- Schema diagram: diagram showing relations and relationships between schemas. Relations/tables are shown with their title & attributes.

- Foreign-key constraints: arrows from referencing relation to primary key of referenced relation
- Referential integrity constraint: two-way arrow between the attributes
- Primary key: underlined

- Entity-Relationship diagram: a different way of showing schema diagrams. Similar, but different notations.

- Query language types:

- Imperative: set of instructions to get to desired result
- Declarative: describe information you want
- Functional: evaluation of set of functions