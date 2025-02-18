# Hadoop

It's a scalable fault tolerant distributed system for [[Big Data]].
It has 2 components: Distributed database for big data and map reduce paradigm.

Hadoop is designed for **data intensive workloads**, but for bigger amount of data and you need calculations you have to use **HPC High performance computer**

### Main Components
There are 2 base components: the first one is distributed big data processing infrastructure based on MapReduce paradigm, and the second is a **HDFS** (Hadoop distributed file system).
Both of this two components are fault-tolerant.

![[Pasted image 20231003165740.png]]

Each disk has a specific number of chunks (same number for all server). Data are spread around all server so we can recover data (usually 3 copies for chunk).
The combination of all disks is the HDFS.
Data inside HDFS are rarely updated, but reads and appends operations are common.
Each chunks/block contains a part of content of one single file.
You cannot have content from two different files inside the same blocks.

```ad-important
If we have to store many lightweight files Hadoop is not the solution, because each file is stored a block, HUGE WASTE OF MEMORY.
(1000 files of 10 KB means 1000 disks)
```

```ad-note
If there's a failure in a disk, the HDFS will immediatly create a copy of the chunk inside that broken disk, and put that copy in a different server 
```

Due to this infrastructure, when we program we tell servers just **what** we want to do, not how, because the management of the system is Hadoop's goal. We do not care about synchronization. 

Another main component is the **Master node**: it's a special node that stores HDFS metadata, like the mapping between the name of a file and its location.

![[Pasted image 20231003171820.png]]

# MapReduce in Hadoop

three main actors in map reduce in Hadoop:
- Driver, running on client machine
- Mapper, running on cluster
- Reducer, running on the cluster
We create three different classes for each of these elements: then we have a Driver (instance of Driver class, coordinates the application's flow), a Mapper (instance of Mapper class, realizes the mapping function) and the Reducer (instance of Reducer class, realizes the reducing function). We **don't care about sort & shuffle phase**.
A **Job** is the execution of a MapReduce code on a set of data
A **Task** is a subset of a Job, the execution of a MapReduce code on a slice of data.

A **Mapper** is applied on a list of elements (input) and emits a **Set of (key, value) pairs**.
After the shuffle and sort phase we have a **temporary set of (key, list of values) pairs**.
The **Reducer** takes this set in input and emit a final **set of (key, value) pairs** which is stored as the final result in HDFS.

In order to parallelize data: a mapper is instantiates for each input splits and there a user-specified numbers of reducers

![[Pasted image 20231009115017.png]]

### Driver

The **Driver** contains a main method and also a **run method**: the second one is needed to configure the job, so we need to specify the name of the job, its input file format and output format.
The same procedure is for the **Mapper**: we decide the name of the class, types of its input and output. Same for **Reducer**.

### Mapper

Contains the **map method** which is empty and we need to write the correct procedure. Hadoop automatically calls this method for each key value pairs.

### Reducer

Contains the **reduce method** which is automatically called for each (key, list of values) pairs and produces final result.

```ad-important
In Hadoop all values associated at the same key are sent by the mapper to the **a single instance** of reducer.

```
### Data types

We cannot directly use native Java types, because during the process we send data on the network. We use particular classes

| MapReduce types | Java corresponding types |
| --------------- | ------------------------ |
| Text            | like Java String         |
| IntWritable     | like Java Integer        |
| LongWritable    | like Java Long           |
| FloatWritable   | like Java Float                         |

To define a particular type we need to implement the **Writable** and **Writable Comparable** interfaces: All keys are instances of writing Comparable and all values are instances of writing.

### Input Format

The input file of a MapReduce program is a HDFS file, we need to define something to split the input in different chunks: we just need to specify the format of input.
There are **predefined** class: **TextInput format**, **KeyValueTextInput** and SequenceFileInput.

Example of TextInput format:

![[Pasted image 20231009164312.png]]

Example of KeyValueTextInput:

![[Pasted image 20231009164229.png]]

For the **Output format** it's the same: we need to declare output format.
The standard format is **TextOutputFormat** class:

```
key \t value \n
key \t value \n
```

To run a Hadoop project we need to create a config file for VS code: we need to add arguments to our program:

```
"args": ["2", "FolderWithExampleData/", "OutputFolder/"]
```

# Combiner

In a standard MapReduce program, data is sent from mappers to reducers: to limit the amount of network data we can use **Combiners**, also called mini-reducers.
The purpose of a combiner is similar to a reducer's job:

![[Pasted image 20231010140607.png]]

Note that combiners are called **locally**! After this job we have a smaller list of key value pairs and a lighter use of the network.

A combiner works only is its function is **commutative and associative**.
In Hadoop combiner's execution is not guaranteed: Hadoop decide if create or not an instance of combiner (based on internal statistics): a MapReduce program **should not depend on the combiner execution**.

A combiner has similar characteristics of a reducer:
- It's an instance of reducer class
- runs on the cluster
- has a reduce method

# Personalized data types

It's useful to have custom data types for complex structures: to create a new one we must define the methods **ReadField** and **write**.
Also it's good practice to override **to String** method.

```
public class SumAndCountWritable {
	private float sum;
	private int count;
	
	@Override
	public void write(DataOutput out) throws IOException {
		out.writeFloat(sum);
		out.writeInt(count);
	}

	@Override
	public void readFields(DataInput in) throws IOException {
		sum=in.readFloat();
		count=in.readInt();
	}
}
```

# Parameters in Driver, mapper and reducer

![[Pasted image 20231016161549.png]]

# Counters

Hadoop provides a set of basic counters to store some statistics about job. But we also define 'ad-hoc' user-defined counters to compute statistics associated with our application.
The user-defined are defined by means of **Java enum**: by default the starting value is zero, then in the mapper and reducer we can increment it.
At the end of the job we can access the global final value of the counters.
To increment a counter inside mapper/reducer:

```java
context.getCounter(countername).increment(value)
```

Pay attention that `value` must be Integer, then we can use `getCounters()` or `findCounter()` to retrive the values of the counters.

In order **to use a counter it must be imported**.

Example:

```java
//inside driver
public static enum COUNTERS{
	ERROR_COUNT,
	MISSING_FIELDS_RECORD_COUNT
}
//inside mapper
context.getCounter(COUNTERS.ERROR_COUNT).increment(1)

//finally, inside driver
Counter errorCounter = job.getCounters().findCounter(COUNTERS.ERROR
_COUNT);
```

# Map-only job

In some job **all the work can be done by the mapper**.
Hadoop allows this approach: reduce phase is avoided as the shuffle and sort phase. The **output is stored directly in HDFS**.
Even if we don't have a reducer class, we must specify the input and output format for the reducer class.
To configure a map only job we just need to set the number of reducers to 0:

```java
job.setNumReduceTasks(0);
```

# In-Mapper combiner


# Advanced Topics

### Multiple inputs

In some application we need to read data from more than one dataset.
Hadoop allows you from **multiple inputs** with **different formats**: however we need to declare a **different mapper for each input dataset**, but the (key, value) output pair must be **consistent** in terms of data types.

Inside the driver we need to specify the path and type for each input dataset:

```Java
MultipleInputs.addInputPath(job, new Path(args[1]),
		TextInputFormat.class, Mapper1.class);

MultipleInputs.addInputPath(job, new Path(args[2]),
		TextInputFormat.class, Mapper2.class);
```

### Multiple outputs

In some applications it's useful to **store output pairs in different files**.
**Each file has a prefix** that specify the content, however all the files are stored inside the same directory.

Inside the driver we use the `MultipleOutputs.addNamedOutput` method: it has 4 parameters, job object, prefix name, key output format and value output format.

```Java
MultipleOutputs.addNamedOutput(job, "hightemp",
	TextOutputFormat.class, Text.class, NullWritable.class);

MultipleOutputs.addNamedOutput(job, "normaltemp",
	TextOutputFormat.class, Text.class, NullWritable.class);
```

### Distributed cache

Some applications need to share and cache small read-only file to perform their task. These files should be accessed from every node of the cluster efficiently.
`DistributedCache` is a facility provided from Hadoop to cache files.

![[Pasted image 20231024151036.png]]

If we want to share a file by means of distributed cache, we must use this method:

```Java
job.addCacheFile('patha/to/file');
```

Se il file è in locale allora usiamo `new Path('urlToPath').toUri();` come argomento di `addCacheFile`.

Usually the content of the shared file is read inside the `setup` method of the mapper class:

```Java
URI[] urisCachedFiles = context.getCacheFiles();
BufferedReader file = new BufferedReader(new FileReader(
	new File(new Path(urisCachedFiles[0].getPath()).getName())));
	
// Iterate over the lines of the file
while ((line = file.readLine()) != null) 
	// process the current line
```
