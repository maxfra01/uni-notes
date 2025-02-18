# Spark Streaming

Spark streaming is a framework for **large scale stream processing**. It scales to hundreds of nodes, and it provides a simple API for complex algorithm.

![[Pasted image 20240109131757.png]]

First part of a stream processing is **stream discretization**: input is split in partition (called **batches**) and then each batch is processed by the spark engine.

![[Pasted image 20240109132646.png]]

### Get input Data

A **DStream** is a sequence of RDD  representing a discretized
version of the input stream of data, while a **PairDStream** is a sequence of PairRDDs representing a stream of pairs.
There are also **transformations and actions** (output operations).

```Java
JavaStreamingContext jssc =
new JavaStreamingContext(conf,Durations.seconds(10));
```

This means that a batch is created each 10 seconds.

To get input data we can analyze the traffic on a **TCP socket**:

```Java
String hostName ="localhost";
Integer port = 8000;

JavaReceiverInputDStream<String> lines=jssc.socketTextStream(hostName, port);

```

To read data from a HDFS folder we can use a specific method:

```java
String inputFolder = "./home/etc/...";

JavaDStream<String> lines =
jssc.textFileStream(inputFolder);
```

### Windows

It is possible to create **windows** with the `window()` method: we have to pass two parameters which are **window length** and **sliding interval**.

### Transformations and actions

There are basic transformations such as `map()`, `flatMap()`, `filter`, `reduce()`, `reduceByKey()`, `countByValue()` ecc...
Actions
