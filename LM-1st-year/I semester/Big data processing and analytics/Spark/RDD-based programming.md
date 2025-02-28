[[Spark]]

# Create RDDs 

We always start from a **SparkContext** object `sc`.
### RDD from files

```java
JavaRDD<String> lines = sc.textFile(inputfile); 
```

Each line of the input file is associated with a string inside the created RDD.
If the input file is stored inside HDFS the partition number is equal to the number of blocks of HDFS used to store input data.
If the `inputfile` is a folder, then Spark will read all files inside that folder.

Number of partitions can also be specified manually:

```java
JavaRDD<String> lines = sc.textFile(inputfile, 5);
```

### RDD from collections

Starting from a list in Java we can use the following method:
Number of partitions can be specified.

```java
List<String> lista = Array.asList("first", "second");
JavaRDD<String> rdd = sc.parallelize(lista, 10);
```

# Save RDDs

We can use the method `saveAsTextFile`.

```java
JavaRDD<String> result;
result.saveAsTextFile( String output_path);
```

Similar to Hadoop, Spark cannot overwrite an existing folder inside the file system.

# Retrieve Data from RDDs

It is based on `collect()`.
PAY ATTENTION TO THE SIZE OF RDDS, DO NOT USE AT THE EXAM.

```java
JavaRDD<String> lines;
List<String> temp = lines.collect();
```

# RDDs operations

**Transformations** are **lazily computed**, the new RDD is not generated until an action is executed on that new RDD.

An **action** is **immediately executed** and they return local variables, so we need to pay attention to variable size.

# Functions

### Named classes

When we execute transformations we need to pass a function (or Lambda functions): we can define user-defined function using Spark interface:

![[Pasted image 20231107145717.png]]

So, if we need to create a function returning a Boolean value:

```
class filterClass implement Function<String,Boolean>{
	public Boolean call(String s){
		return s.contains("error");
	}
}
```

Then we can use that class inside Spark driver program:

```
lines.filter( new filterClass());
```

### Anonymous classes

Same approach but we define the function each time we invoke filter:

```
JavaRDD<String> errorsRDD = inputRDD.filter(
	new Function<String, Boolean>() {
		public Boolean call(String x) {
			return x.contains("error");
		}
	}
);
```

### Lambda functions

```
JavaRDD<String> errorsRDD = inputRDD.filter(
	line -> line.contains("error");
);
```

# Transformation

![[Pasted image 20231107153002.png]]
![[Pasted image 20231107153017.png]]
![[Pasted image 20231107153027.png]]
![[Pasted image 20231107153039.png]]
![[Pasted image 20231107153051.png]]

# Actions

![[Pasted image 20231107153504.png]]
![[Pasted image 20231107153517.png]]
![[Pasted image 20231107153527.png]]
![[Pasted image 20231107153541.png]]
# Pair RDD - Transformation

First method to creare a pair RDD is by using `mapToPair()` which map each input element of a traditional RDD into an element of a pair RDD.
From a collection in Java we can use `parallelizePairs()` to create a pair RDD.

To represent a pair we can use the scala class `scala.Tuple2<K,V>` where K and V are associated to the type of each element that form the tuple.
Both key and value part of a tuple can be retrieved by using `._1()` or `._2()`

### Reduce by key

The goal is to create a new pair RDD where there is a single tuple for each key k: user-defined function must be associative and commutative.
Differently from reduce method, this one returns a set of (key, value) pairs while reduce method returns a single value.

### Fold by key

It's the same goal of reduce by key but the function must be associate, but it's not required the function to be commutative.
It is also characterized by a zero value.

### Combine by key

The goal is to compute a similar job of reduce by key, but data types can be different from the source pair RDD.
The function has the following signature: 

```java
JavaPairRDD<K,U> combineByKey(
	Function<V,U> createCombiner,
	Function2<U,V,U> mergeValue,
	Function2<U,U,U> mergeCombiner);
```

### Group by key

The goal is to create a new pair RDD which there is a single entry for each key, and the value is an iterable of all values associated to that key.

```ad-danger
title: Exam
Use `groupByKey()` only in few use scenario.
At the exam try using reduceByKey and not groupByKey.

```

The most common use case scenario is with **a function that is not commutative**.

### Map Values

It creates a new pair RDD but we apply a specific function just on the value of each pair.
It does not require to send data on the networks

```Java
JavaPairRDD<K,U> mapValues(Function<V,U> f);
```

There is also **flat map values**, but the return type of the values can be different.

### Keys

This method convert a pair RDD to a normal RDD, where we have only the **key part**:

```JAVA
JavaRDD<K> keys();
```

### Values

Same as above, but with values:

```java
JavaRDD<V> values();
```

### Sort by key

Return a new pair RDD obtained by sorting in ascending order the pairs of the input pair RDD by key.

### Transformation recap

![[Pasted image 20231121132407.png]]
![[Pasted image 20231121132419.png]]
![[Pasted image 20231121132428.png]]
![[Pasted image 20231121132437.png]]
![[Pasted image 20231121132448.png]]
![[Pasted image 20231121132500.png]]

# Pair RDD - Transformation on 2 pair RDD

Spark supports also some transformations on two pair RDD:

### Subtract by key

Used to implement a sort of "subtract": the result is a pair RDD where the key doesn't appear in the second RDD as a key.

```java
JavaPairRDD<K,V> subtractByKey(JavaPairRDD <K,U> other);
```

### Join

The goal is to perform a join between two pair RDD based on the key.
The new pair RDD has as key the same key of the input RDD; as Value it has a tuple with the (value of the first RDD, value of the second RDD) both associated with the same key.
A **shuffle** operation is performed from this transformation.

### Co Group

The goal is to associate each key of the input with two lists: first list with all values of the input RDD associated with the key; second list with values of the other RDD associated to the key

```JAVA
JavaPairRDD <K, Tuple2<Iterable<V>,Iterable<U>>> cogroup(JavaPairRDD<K,U>);
```

Also `cogroup()` requires a shuffle operations.

```ad-danger
title: cogroup
Similar to `collect()` and `groupByKey()`, `cogroup()` should not be used at the exam. 

```

# Pair RDD - Actions

Sulle slide.

# Pattern

For sliding windows analysis we always use flat Map to return a number of elements equal to the size of the windows, then we use group by key.