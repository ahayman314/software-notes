An index is simply a **pointer** to data in a table. Without indexing, the query will need to search through the rows linearly for the result. 

Typically, the **primary key** is the **clustered index** - how the database rows are sorted in the table itself. When queries with criteria based on the clustered index is searched, it is very fast because it is sorted. You can change the clustered index to be on a different column as well. 

A **non-clustered index** is a **secondary index** that is a separate data structure sorted by a particular field, pointing to the physical location of the corresponding database record. 

In practice, this means indexing reduces searching from $O(n)$ to $O(logn)$. 

However, this slows down data modification requests. Indexes are not great when there is a high write-read ratio. 

### SQL
When you specify multiple columns, you are indexing by multiple columns, so it is best for queries that involve these multiple columns. 
```sql
CREATE INDEX index_name
ON table_name (col_name_1, col_name_2, ...)
```

- After the column, you can add things like `DESC` for reverse order - good for recent items

Good example for multiple columns: 
```sql
SELECT FirstName, LastName, Email
FROM Customer
WHERE FirstName = 'Mark' and LastName = 'Thompson'
```