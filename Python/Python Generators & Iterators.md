In [[Python]], you have the concept of generators and iterators.

### Iterators
An object that can derive an iterator is an **iterable**. Iterables produce iterators when iterated upon. 
Each iterable object has a `__iter__()` and `__next__()` method to track the internal state. 
- `__iter__()`: creates the iterator for the next object
- `__next__()`: iterates once over the object

Lists, tuples & sets are all iterables that we commonly use. Note that they are not iterators themselves - only `iter(list)` and `iter(set)` are iterators. 

### Generators
We typically use generators to create iterators. The idea here is to only get what you need when you really need it without storing everything in memory. 
Generators look like functions, but use `yield` instead of return. The state of the generator is remembered after the call. 
A generator expression looks like `(num**2 for num in range(5))`. 