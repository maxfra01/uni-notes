# Relational operators

Relational algebra operators in Hadoop **are useful only when a full scan on the input table is needed**: selective queries are not efficient using a map reduce approach.
Most of data preprocessing activities involve relational operators (ETL in data warehousing).

Note that relations/tables can be stored inside HDFS (divided in chunks and spread across the ile system).

This approach is a bonus of Hadoop [[Design patterns]].
### Selection

Select only a limited number of records, using a predicate: we can use the **filtering pattern**:
- Map-only job
- Each mapper analyzes one record at a time and if the condition (predicate) is true then the mapper emits a (key, value) where **key is the record itself and value is null**.

### Projection

The goal is to select a limited number of attributes S.
Each mapper:
- Analyzes one record at a time of its split
	- For each record r in R
	- It selects the values of the attributes in S and constructs a new record r’
	- It emits a (key, value) pair with key=r’ and value=null

Each reducer:
- Emits one (key, value) pair for each input (key, \[list of values\]) pair with key=r’ and value=null

### Union

Mappers:
- For each input record t in R, emit one (key, value) pair with key=t and value=null
- For each input record t in S, emit one (key, value) pair with key=t and value=null

Reducers:
- Emit one (key, value) pair for each input (key, \[list of values\]) pair with key=t and value=null
- i.e., one single copy of each input record is emitted

### Intersection

Mappers:
- For each input record t in R, emit one (key, value) pair with key=t and value=“R”
- For each input record t in S, emit one (key, value) pair with key=t and value=“S”

Reducers:
- Emit one (key, value) pair with key=t and value=null for each input (key, \[list of values\]) pair with \[list of values\] containing two values
▪ It happens if and only if both R and S contain t

### Difference

Mappers
- For each input record t in R, emit one (key, value) pair with key=t and value=name of the relation (i.e., R)
- For each input record t in S, emit one (key, value) pair with key=t and value=name of the relation (i.e., S)
Two mapper classes are needed: one for each relation

Reducers:
- Emit one (key, value) pair with key=t and value=null for each input (key, \[list
of values\]) pair with \[list of values\] containing only the value R
- It happens if and only if t appears in R but not in S

### Other operations

Join: can be implemented by using the join patterns
Group by and Aggregations: can be implemented using the summarizations pattern.