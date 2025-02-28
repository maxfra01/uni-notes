# Definition of Big Data

Data whose **scale, diversity, complexity** require new architectures, techniques and algorithm to manage it.

Big data can also be described by Vs:
- **Volume**, which is the scale of data. During the years the quantity of data increases exponentially
- **Variety**, big data can have many forms and applications can generate numerical, audio, video ....
- **Velocity**, generation rate of data is fast 

There are also **Veracity and value**: the first indicates the data quality, the second indicates how to translate data in business advantage.

### Big Data value chain

| Generation                                                                                                                                 | Acquisition                                       | Storage | Analysis |
| ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- | ------- | -------- | 
| Three type of data: Passive recording (like bank transaction), Active generated (like social network interactions), Automatics productions | Collection, then transmission, then preprocessing |HDD, SDD, NAS, DAS, specific file system (HDFS, MongoDB)         |Programming paradigma to extract information          |

To process data we need processors; data are stored in hard drives.
Due to the volume of data there is a **bottleneck effect**.
The solution consists in **transfer processing power to data** and **parallel processing**:

![[Pasted image 20231002172137.png]]

# Big Data Architectures

A big data architecture is designed to handle the ingestion, processing, and analysis of data that is too large or complex for traditional database systems.
This solutions involve the following types of workload:
- **Batch processing**
- **Real time processing**
- **Interactive exploration** of data
- **Predictive analytics and machine learning**

When do we need a specific big data architecture?
This architecture is needed when we work with:
- **Volume of data too big** for traditional database
- **Unstructured** data we want to transform
- Unbounded **stream of data** we want to process in real time (or with low latency)

### Lambda architecture
Hadoop can parallelize data, but with high latency.
NoSQL database have great scalability by offering a limited data model. Also databases are mutable, they're not human-fault tolerant.

Lambda architecture combines all these technologies in order to make a **scalable system for arbitrary data problems, human-fault tolerant and with minumum complexity**.

Another definition is "Lambda architecture is a way of processing
massive quantities of data (i.e. “Big Data”) that provides access to batch-processing and stream-processing methods with a hybrid approach"

From Wikipedia: "Lambda architecture is a data-processing architecture designed to handle massive quantities of data by taking advantage of both batch and stream-processing methods"

The **requirements** of Lambda architecture are:
- **Fault tolerant** against human actions and hardware failures
- **Support variety of use cases**, like real time querying
- Linear **Scale out capabilities**
- **Extensible**, so that the system can accomodate new features

All query can be done by a function: **query = function(all data)** and the result will be a new dataset.
Each query has parameters like latency, timeliness (freshness and consistency) and accuracy.

Consider this model:

![[Pasted image 20231003133332.png]]

All new data is dispatched to both path:
- The first path is the 'Cold path' (**Batch layer**): it stores all raw new data in an immutable, append-only database and also pre-compute data in **batch views**. The **serving layer** indexes the views so that they can be queried in low latency.
- The other path is 'Hot path' ( **Speed layer**) and it deals with real data only, creating **real time views**

Note that queries can be made on both type of views!

![[Pasted image 20231003133823.png]]

### Data center
Current system must scale to address:
- Vertical scalability (**scale up**) means have less nodes with greater computational power (high price, less fault tolerant)
- Horizontal scalability (**Scale out**) means increase the number of nodes with less computational power.
This **horizontal approach is preferred** for big data applications because it can process more data (parallelism) and it's more fault tolerant.

