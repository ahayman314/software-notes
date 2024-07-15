From [[Databases]], we want an abstract way of doing a [[Relational Model]]. This is done with relational algebra.

### Summary
- Relational algebra: pure functional language with set operations that input 1+ relations & produce new relations as outputs
- Unary operation: operate on one relation (select, project, rename...)
- Binary: operate on pairs of relations (Cartesian product, union, set difference...)
- Relational algebra expression: composition of relational-algebra operations
- Relational algebra operations: six basic operations for relational algebra

###### Operations
We only need select & project to filter down columns & rows, and Cartesian products to join tables. Everything else uses those basic operations.

| Name              | Summary                                                                      | Relational Algebra                                           | Equation                                                                                                                                                                                     | SQL Equivalent |
| ----------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Select            | Filters down rows/tuples                                                     | $\sigma_{E(a1,...,an)}(r)$<br>$\sigma_{name='bob'}(student)$ | $\sigma$ = sigma (select)<br>3 boolean: <br>$\wedge$ = wedge (AND)<br>$\vee$ = vee (OR)<br>$\neg$ = neg (NOT)<br>6 comparators:<br>$\ne$, $\gt$, $\lt$, $\ge$, $\le$, = (ne, gt, lt, ge, le) | WHERE          |
| Project           | Filters down / selects columns                                               | $\Pi_{a1,...,an}(r)$<br>$\Pi_{name,age}(student)$            | $\Pi$ = Pi (project)                                                                                                                                                                         | SELECT         |
| Cartesian Product | Outer product between relations<br>For n, m tuples, we get n$\times$m tuples | $r=t1 \times t2$                                             |                                                                                                                                                                                              | OUTER JOIN     |
|                   |                                                                              |                                                              |                                                                                                                                                                                              |                |
| Join              | Select with Cartesian Product <br>Filter down rows after outer product       | $r_1\bowtie_{\theta} r_2 = \sigma_{\theta}(r_1 \times r_2$ ) | $\bowtie$ = bowtie ()<br>$\theta$ = theta (select)                                                                                                                                           | <br>           |
