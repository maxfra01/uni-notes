# Introduction to Spark

Spark is a general-purpose framework to analyze big amount of [[Big Data]].
It is more **general**, has **lower** **latency** (compared to Hadoop), **fault** **tolerant** and **simpler**.

MapReduce for Hadoop is very slow when it has to perform iterative jobs: it requires lots of disk I/O which is generally very slow.
Also during the years, the cost of memory decreased, so the basic idea is to **keep more data in main memory**: 

![[Pasted image 20231106172251.png]]

In Spark, data are represented as **Resilient Distributed Datasets (RDDs)**, which are collections of objects spread across the nodes of a cluster (stored in main memory).
Spark provides a programming abstraction and transparent mechanisms to execute code in parallel on RDDs:
- **Transformation** operations such as map, filter or join
- **Action** operations such as count, collect or save
 
 RDDs are automatically rebuilt on machine failure.

![[Pasted image 20231106173037.png]]

# Main components of Spark

Spark is based on the **Spark Core**, which contains all important APIs and method to manipulate RDDs.
Since all components are based on the core, if the core efficiency increases also the efficiency of high level components will increase.

The **SparkSQL component** allows to sql-like query datasets and dataframes. It also has a query optimizer engine.

The **Spark Streaming component** in near real time: the APIs of the streaming components operates on RDDs.

**MLlib e GraphX** are two libraries for Machine Learning and graph analysis: they both provides algorithms.

Then we have 3 available schedulers in Spark: **Standalone Spark Scheduler, YARN scheduler and Mesos**.

![[Pasted image 20231107131342.png]]

# Basic concepts of Spark

RDDs are the primary abstraction of data in Spark: they are split in partitions and **each node of the cluster contains at least one partition**. 
This partitions are saved in main memory (if it is sufficient) or in disks.
Each executor works on his partitions:

![[Pasted image 20231107133601.png]]

Spark does the following things:
- Manages scheduling and synchronization of the jobs.
- Manages the split of RDDs in partitions and allocates RDDs’ partitions in the nodes of the cluster.
- Hides complexities of fault-tolerance and slow machines.
### RDDs

**RDDs are immutable** once constructed, also Spark through a Direct Acyclic Graph (**DAG**) tracks information to efficiently recompute lost data.
We can create an RDD by:
- **Parallelizing existing collections** of the hosting programming language (like a collection of lists in python), so the number of partition is specified by the user
- **From (large) files stored in HDFS**, so the number of partitions is the same of number of HDFS block.
- **Transforming an existing RDD**, so the partitions depend on the type of transformation
- **From files stored in many traditional file system** (or database)

# Spark program

The **driver program in Spark** is the main class: it defines the workflow of the application, we create RDDs and manipulate them.
Spark can be accessed by a **SparkContext** object, which create a connection from the local application to the cluster.

```Java
SparkConf conf= new SparkConf().setAppName("Spark Word Count");

JavaSparkContext sc = new JavaSparkContext(conf);
```

All local variables in the code are stored in main memory, while RDDs are store inside nodes, specifically in main memory (or disks in memory is not sufficient).

![[Pasted image 20231107135545.png]]

Spark applications **can be executed locally**: Local threads are used to parallelize the execution of the application on RDDs on a single PC.

![[Pasted image 20231107135745.png]]

### Terminology

| Concept         | Meaning                                                                                                                      |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Application     | User program built on Spark: composed by a driver and executors                                                              |
| Cluster Manager | An external service for acquiring resources on the cluster                                                                   |
| Deploy Mode     |                                                                                                                              |
| Worker node     | Any node of the cluster that can run application code in the cluster                                                         |
| Task            | A unit of work that will be sent to one executor                                                                             |
| Executor        | A process launched for an application on a worker node, that runs tasks and keeps data in memory or disk storage across them |
| Job             | A parallel computation consisting of multiple tasks that gets spawned in response to a Spark action (e.g. save, collect)     |
| Stage           | Each job gets divided into smaller sets of tasks called stages: the output of one stage is the input of the next stage. A shuffle operations is always executed between stages (heavy operations) |

# Spark Example

Counting lines of a text input file:

```java
package it.polito.bigdata.spark.linecount;
import org.apache.spark.api.java.*;
import org.apache.spark.SparkConf;

public class DriverSparkBigData {
	public static void main(String[] args) {
		
		//Declare local variables
		String inputFile;
		long numLines;
		inputFile=args[0];
		
		// Create a conf object and set the name of the application
		SparkConf conf=new SparkConf().setAppName("Spark Line Count");
		
		// Create a Spark Context object
		JavaSparkContext sc = new JavaSparkContext(conf);
		
		// Build an RDD of Strings from the input textual file
		// Each element of the RDD is a line of the input file
		JavaRDD<String> lines=sc.textFile(inputFile);
		
		// Count the number of lines in the input file
		// Store the returned value in the local variable numLines
		numLines=lines.count();
		
		// Print the output in the standard output (stdout)
		System.out.println("Number of lines="+numLines);
		
		// Close the Spark Context object
		sc.close();
		}
}
```
