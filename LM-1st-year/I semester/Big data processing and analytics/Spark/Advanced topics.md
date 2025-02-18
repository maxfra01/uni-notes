[[Spark]]

# Persistence and Cache

If we have to process the same RDD multiple times, we can cache the RDD. When an RDD is in cache, each node memorizes its partition of data.
- The first time an action is executed, nodes stores data in memory
- Other actions will read data from main memory
Caching is useful only in programs with more than 2 actions.

We can choose different storage levels:
- Main memory of nodes
- Disks of nodes
- Both solution together 

![[Pasted image 20231127161436.png]]
![[Pasted image 20231127161448.png]]

We can mark an RDD to be persisted by using the command:

```java
JavaRDD<T> persist(StorageLevel level);
```

Where `StorageLevel` is one of the following options:
- StorageLevel.MEMORY_ONLY()
- StorageLevel.MEMORY_ONLY()
- StorageLevel.MEMORY_AND_DISK()
- StorageLevel.MEMORY_ONLY_SER()
- StorageLevel.MEMORY_AND_DISK_SER()
- StorageLevel.DISK_ONLY()
- StorageLevel.NONE()
- StorageLevel.OFF_HEAP()
- StorageLevel.MEMORY_ONLY_2()
- StorageLevel.MEMORY_AND_DISK_2()
- StorageLevel.MEMORY_ONLY_SER_2()
- StorageLevel.MEMORY_AND_DISK_SER_2()

We can cache an RDD also:

```Java
JavaRDD<T> cache();
```

This approach is equivalent to `persist(StorageLevel.MEMORY_ONLY()`
Both of this operations return a new RDD, because RDD are immutable.
To manually remove an RDD we use: `unpersist()`.
Spark (theoretically) monitors cache usage and auto remove unused RDD.

# Accumulators

**Accumulators** are shared variable in Spark: they are used to compute simple statistics between all nodes.
Similar to RDD, accumulators are **lazily evaluated**, this means that values are updated only when there is an action that trigger the execution of the transformations.

We can define an accumulator using `longAccumulator()` or `doubleAccumulator()` methods of Spark Context class.
Then we update the value: `myAccumulator.add(<n>)`
Esempio:

![[Pasted image 20231127165456.png]]

To retrieve the value: `my_accumulator.value()`
The print of an accumulator must be after an action, otherwise if no actions is executed, the value of accumulator is still the value we initialized.

To process each line and update an accumulator we can use `foreach()` method of Java RDD class.

### Personalized accumulators

