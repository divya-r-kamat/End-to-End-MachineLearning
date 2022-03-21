## What is Spark?

Spark is a platform for cluster computing. Spark lets spread data and computations over clusters with multiple nodes (each node can be thought as a separate computer). Splitting up data makes it easier to work with very large datasets because each node only works with a small amount of data.
As each node works on its own subset of the total data, it also carries out a part of the total calculations required, so that both data processing and computation are performed in parallel over the nodes in the cluster. It is a fact that parallel computation can make certain types of programming tasks much faster.

The first step in using Spark is connecting to a cluster. The cluster will be hosted on a remote machine that's connected to all other nodes. There will be one computer, called the master that manages splitting up the data and the computations. The master is connected to the rest of the computers in the cluster, which are called worker. The master sends the workers data and calculations to run, and they send their results back to the master.

Creating the connection is as simple as creating an instance of the `SparkContext` class. The class constructor takes a few optional arguments that allows to specify the attributes of the cluster connecting to. An object holding all these attributes can be created with the `SparkConf()` constructor. 

Few things to Note :
- It takes more time to start up Spark. 
- Running simpler computations might take longer than expected. That's because all the optimizations that Spark has under its hood are designed for complicated operations with big data sets. That means that for simple or small problems Spark may actually perform worse than some other solutions!

      # Verify SparkContext
      print(sc)
      <SparkContext master=local[*] appName=pyspark-shell>


      # Print Spark version
      print(sc.version)
      3.2.0

## Spark Data Structure

Spark's core data structure is the Resilient Distributed Dataset (RDD). This is a low level object that lets Spark work its magic by splitting data across multiple nodes in the cluster. However, RDDs are hard to work with directly, so Spark DataFrame can be used which is abstraction built on top of RDDs. The Spark DataFrame was designed to behave a lot like a SQL table (a table with variables in the columns and observations in the rows). Not only are they easier to understand, DataFrames are also more optimized for complicated operations than RDDs. When using RDDs, it's up to the data scientist to figure out the right way to optimize the query, but the DataFrame implementation has much of this optimization built in!


To start working with Spark DataFrames, we first have to create a `SparkSession` object from `SparkContext`. We can think of the `SparkContext` as connection to the cluster and the `SparkSession` as interface with that connection.

### Creating a SparkSession

Use the SparkSession.builder.getOrCreate() method to create SparkSession. This returns an existing SparkSession if there's already one in the environment, or creates a new one if necessary!

    # Import SparkSession from pyspark.sql
    from pyspark.sql import SparkSession

    # Create my_spark
    my_spark = SparkSession.builder.getOrCreate()

    # Print my_spark
    print(my_spark)
    
### Viewing tables

SparkSession has an attribute called catalog which lists all the data inside the cluster. This attribute has a few methods for extracting different pieces of information. One of the most useful is the .listTables() method, which returns the names of all the tables in the cluster as a list.

One of the advantages of the DataFrame interface is we can run SQL queries on the tables in Spark cluster. Running a query on the table is as easy as using the .sql() method on your SparkSession. This method takes a string containing the query and returns a DataFrame with the results!

    query = "select * from table"

    # Get the first 10 rows of table
    table_df = spark.sql(query)

    # Show the results
    table_df.show()
    
    
### Pandafy a Spark DataFrame

Spark DataFrames make that easy with the .toPandas() method. Calling this method on a Spark DataFrame returns the corresponding pandas DataFrame.

      pd_counts = spark_df.toPandas()

The .createDataFrame() method takes a pandas DataFrame and returns a Spark DataFrame. The output of this method is stored locally, not in the SparkSession catalog. This means that we can use all the Spark DataFrame methods on it, but can't access the data in other contexts. For example, a SQL query (using the .sql() method) that references the DataFrame will throw an error. To access the data in this way, we have to save it as a temporary table. We can do this using the .createTempView() Spark DataFrame method, which takes as its only argument the name of the temporary table we'd like to register. This method registers the DataFrame as a table in the catalog, but as this table is temporary, it can only be accessed from the specific SparkSession used to create the Spark DataFrame. There is also the method .createOrReplaceTempView(). This safely creates a new temporary table if nothing was there before, or updates an existing table if one was already defined. 

      # Create spark_temp from pd_temp
      spark_temp = spark.createDataFrame(pd_temp)
      
      # # Add spark_temp to the catalog
      spark_temp.createOrReplaceTempView("temp")



SparkSession has a .read attribute which has several methods for reading different data sources into Spark DataFrames. Using these we can create a DataFrame from a .csv file just like with regular pandas DataFrames
