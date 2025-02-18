Design patters for MapReduce job in [[Hadoop]].

# Summarization patterns

Are used to implement applications that produce top-level/summarized view of the
data: numerical summarizations, inverted index, counting with counters

### Numerical Summarizations

In **numerical summarizations** patterns our goal is to group records by a **key fields** and calculate a **numerical aggregate** per group.
This provide a top level view of large input data

- **Mapper**: Its output is (key, value) where key is associated with the fields used to create groups, and value is associated with the fields used to compute the aggregate
- **Reducer**: Receive a set of numerical values for each “group-by” key and compute the final statistics for each “group”
- **Combiner**: If the computed statistic has specific properties (e.g., it is commutative and associative), combiners can be used to speed up performances

![[Pasted image 20231023172021.png]]

### Inverted Index

In **Inverted index** patters our goal is to build an index from the input data to support **faster searches** for data enrichment.
It is used to improve search efficiency.

- **Mappers**: Output (key, value) pairs where key is the set of fields to index (a keyword) and value is a unique identifier of the objects to associate with each “keyword”
- **Reducers**:Receive a set of identifiers for each keyword and simply concatenate them
- **Combiners**: Usually are not useful when using this pattern; Usually there are no values to aggregate

![[Pasted image 20231023172006.png]]

### Counting with counters

In this pattern we simply need to **compute count summarizations** of data set. Similar to numerical summarizations, this approach provides top level view of data.

- Inside **Driver** we define counters
- **Mapper**: process each input record and increment a set of counter
- **Map-only job**: combiners and reducers are useless!

# Filtering patterns

The main goal is to keep just the records we are interested in/ filter out records that are not of interest.
Doing that, we can focus analysis only on useful record.

The input of the mapper is a set of records
- Key = primary key
- Value = record
- **Mappers**: Output one (key, value) pair for each record that satisfies the enforced filtering rule. Key is associated with the primary key of the record. Value is associated with the selected record.
- **Reducers**: The reducer is useless in this pattern because a map-only job is executed (number of reduce set to 0).

### Top K

The goal is to select top k items based on a **ranking** **functions**.
The focus is on the most important data of the entire dataset.

Each mapper initializes an in-mapper(local) top k list where:
- k is usually small
- The local top k-records of each mapper can be stored in main memory.
- Initialization performed inside setup method of the mapper
Then, the map function compute the local top k list.
The output of the mapper should be pairs (null, top k list) for each subset of data.

The **reducer** is unique, because all values have the same key: the reducer computes the final top K list by merging all the lists.

![[Pasted image 20231024131836.png]]

### Distinct

In some applications, **duplicate values are useless**, so the goal of this pattern is to **find a unique set** of values.

The mapper class:
- Emit one (key, value) pair: the key is the input record, value is null

The reducer class:
- emit one (key, value) pair: key is the input, value is null

Key are unique, so don't care about duplicates: the result is the same set of input data but without duplicates.

![[Pasted image 20231024132503.png]]

# Data Organizations pattern

Are used to organize/split in subset our input data.

### Binning

The goal is to move/organize input records into **category**.
We can use **multiple output Hadoop feature**.
Inside the driver we define output paths, then inside the mapper for each record we select the correct output section and emit the pair.

![[Pasted image 20231024151848.png]]

### Shuffling

The goal is to **randomize the order of data**.

Mappers :Emit one (key, value) for each input record
- key is a random key (i.e., a random number)
- value is the input record
Reducers: Emit one (key, value) pair for each value in \[list-of-
values] of the input (key, \[list-of-values\]) pair

![[Pasted image 20231024151938.png]]

### Job-chaining

The goal is to execute a **list of jobs** (**synchronizing** them).
The single driver contains the workflow of the applications.
Then each job has an associated mapper, combiner and reducer.

![[Pasted image 20231024152256.png]]

# Join patterns

### Reduce Side Natural Join

The goal is to perform a natural join of 2 tables.

There are two mapper classes:
- One mapper class for each table
Mappers:
- Emit one (key, value) pair for each input record
	- Key is the value of the common attribute(s)
	- Value is the concatenation of the name of the table of the current record and the content of the current record.
	
Reducers: Iterate over the values associated with each key (value of the common attributes) and compute the “local natural join” for the current key
- Generate a copy for each pair of values such that one record is a record of the first table and the other is the record of the other table.

![[Pasted image 20231024152758.png]]

### Map Side natural join

Must be used only when a table is small, and can be stored in main memory.
The small table is loaded by means of distributed cache.
The mapper compute the local natural join for each record.

![[Pasted image 20231024153638.png]]

This two pattern can be used to implement also theta join, semi join, outer join.