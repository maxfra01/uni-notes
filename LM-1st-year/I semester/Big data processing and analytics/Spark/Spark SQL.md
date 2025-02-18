[[Spark]]

# Spark SQL

**SparkSQL** is a Spark component that can manipulate structures similar to tables.
Similar to RDD, this component provides an abstraction called **Dataset**.
**Catalyst** is an sql-like optimizer that spark uses.

Due to catalyst, program based on datasets are usually faster than RDD based programs.

![[Pasted image 20231128154530.png]]

Dataset and Dataframes are similar, but different from RDD because:
- There is strong typing
- They can use powerful lambda function

# Dataframes

A **Dataframe** is a particular dataset organized into named columns.
Equivalent to a relational table.
To create these objects a **Spark Session** object must be used.
Dataframe is also the alias of `Dataset<Row>` object.
They can be used to **read efficiently from csv, json files**.

Using the load method we can create a dataframe:

```java
SparkSession ss = SparkSession.builder().appName("Tst").getOrCreate();

DataFrameReader dfr = ss.read().format("csv").option("header", true).option("inferSchema", true);

Dataset<Row> df = dfr.load("fileName.csv");
```

The option `inferSchema` allows the system to automatically retrieve the types of each column (age -> integer, name -> String...).
The option `header` allows to ignore the first line (which is the header).

Same approach is used with JSON files:

```java
SparkSession ss = SparkSession.builder().appName("Test").getOrCreate();

DataFrameReader dfr = ss.read().format("json");
Dataset<Row> df = dfr.load("fileName.json");
```

With JSON, Spark SQL automatically infers the schema.
In this particular case we suppose that we have **one JSON object for each line**.
If objects are on more than one line we need to add the option `.option("multiline", true)`, but this **is not recommended**.

### From Dataframe to RDD

We can use the method `javaRDD()` of the class Dataset: each row is like a vector containing the values of a record.

```java
JavaRDD<Row> JavaRDD(Dataset<Row>);
```

Some methods of the class `ROW`:
- `int fieldIndex(String columnName)` returns the integer index of a corresponding column
- To retrieve the content of a column we can use `getAs(String columnName)` but the result is a generical Object, then we must cast the value
- Another approach is by using `getString(int position)`, `getDouble(int position)`...

# Datasets

It' more specific than a dataframe: dataframes are composed by Row objects, while datasets are composed by class-specific objects.
Also, a dataset is way more efficient than a Java RDD: that's because of the Catalytic optimizer. Objects stored in dataset **must**:
- Implement serializable
- All attributes of the class must have all public getter and setter methods
- All attributes must be private

To create a dataset we use:

```java
Dataset<T> createDataset(java.util.List<T> data, Encoder<T> encoder);
```

There are default encoders for standard Java Type.

### From Dataframes to Datasets

First we define a dataframes, then we cast it to a dataset:

```Java
Dataset<T> as(Encoder e);
```

# Operations of Datasets

```java
void show(int numRows);
```

It shows the first `numRows` of a dataset, printing on the standard output.
The **printSchema** methos simply prints the schema of the dataset.

```java
void printSchema();
```

The **count method** counts the number of rows:

```java
long count();
```

**Distinct** method (shuffle phase is needed):

```java
Dataset distinct();
```

**Select** method returns a subset of the original dataset:

```java
Dataset<Row> select(String col1, ...,String coln)
```

