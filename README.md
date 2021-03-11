[image1]: assets/hardware_component.png "image1"
[image2]: assets/cpu.png "image2"
[image3]: assets/ram.png "image3"
[image4]: assets/disk.png "image4"
[image5]: assets/when_small_gets_bigger.png "image5"
[image6]: assets/medium_data.png "image6"
[image7]: assets/hist_of_distr_comp.png "image7"
[image8]: assets/hadoop_tools.png "image8"
[image9]: assets/spark_tools.png "image9"
[image10]: assets/map_reduce_flow.png "image10"
[image11]: assets/spark_cluster.png "image11"
[image12]: assets/spark_use_cases.png "image12"
[image13]: assets/func_vs_proc_prog.png "image13"
[image14]: assets/dag.png "image14"
[image15]: assets/data_storage.png "image15"
[image16]: assets/spark_session.png "image16"
[image17]: assets/imp_vs_decl_prog.png "image17"
[image18]: assets/wrangling_matplotlib.png "image18"
[image19]: assets/rdds.png "image19"
[image20]: assets/aws_emr_setup_pt1.png "image20"
[image21]: assets/aws_emr_setup_pt2.png "image21"
[image22]: assets/aws_setup_standalone.png "image22"
[image23]: assets/spark_scipts.png "image23"
[image24]: assets/script_for_submission.png "image24"
[image25]: assets/submit_command.png "image25"
[image26]: assets/storage_s3.png "image26"
[image27]: assets/open_notebook.png "image27"
[image28]: assets/load_from_s3.png "image28"
[image29]: assets/s3_hdfs_spark.png "image29"
[image30]: assets/hdfs_storage_aws.png "image30"
[image31]: assets/submit_json_to_hdfs.png "image31"
[image32]: assets/hdfs_new_folder.png "image32"
[image33]: assets/copyFromFile.png "image33"
[image34]: assets/hdfs_file_in_folder.png "image34"
[image35]: assets/hdfs_read.png "image35"
[image36]: assets/data_error.png "image36"
[image37]: assets/debug_via_print.png "image37"
[image38]: assets/accumulator.png "image38"
[image39]: assets/web_ui.png "image39"
[image40]: assets/metrics_and_cohort_analysis.png "image40"
[image41]: assets/data_skew.png "image41"

# Spark
How to deal with ***Big Data***?

Why Learn Spark?
Spark is currently one of the most popular tools for big data analytics. You might have heard of other tools such as Hadoop. Hadoop is a slightly older technology although still in use by some companies. Spark is generally faster than Hadoop, which is why Spark has become more popular over the last few years.

There are many other big data tools and systems, each with its own use case. For example, there are database system like Apache Cassandra and SQL query engines like Presto. But Spark is still one of the most popular tools for analyzing large data sets.

Instead of using the own computer it is easier to use a ***distributed system*** of multiple computers (e.g. hundereds of servers on the Amazon Data Center). Spark is one tool to allow this.

Here is an outline of the topics:


## Outline
- [The Power of Spark](#power_of_spark)
	- [Numbers Everyone should know](#numbers_to_know)
	- [Review of the hardware behind big data](#review_hardware)
	- [When is data it big data?](#Big_Data)
	- [Introduction to distributed systems](#intro_distr_sys)
	- [The Hadoop Ecosystem](#hadoop)
	- [Map Reduce](#map_reduce)
	- [The Spark Cluster](#spark_cluster)
	- [Common Spark use cases](#intro_distr_sys)
	- [Introduction to distributed systems](#common_spark_uses)
	- [Other technologies in the big data ecosystems](#other_techs)

- [Data Wrangling with Spark](#data_wrangling)
	- [Functional Style of Programming](#func_prog)
	- [Why Functional Programming?](#why_func)
	- [Procedual Programming](#proc_prog)
	- [The Spark DAGs: Recipe for Data](#dag)
	- [Maps and Lambda Functions](#map_lambda)
	- [Example: Functional Programming - sc- parallize - map - collect](#example_func)
	- [Data Formats](#data_formats)
	- [Data Stores](#data_stores)
	- [Spark Session](#spark_session)
	- [Reading and Writing to Spark Dataframes](#read_write_df)
	- [Imperative vs Declarative programming](#imp_vs_decl)
	- [Data Wrangling with DataFrames](#wrangle_dataframes)
	- [Wrangling Data Functions Overview](#wrangle_data_fun_overview)
	- [Some important aggregate function using agg](#agg_func)
	- [Difference between collect(), show(), take()](#differences_spark_collect_show_take)
	- [Spark SQL](#spark_sql)
	- [RDDs](#rdds)

- [Debugging and Optimization](#debug_optimize)
	- [Setup Instructions for AWS](#aws)
	- [Standalone setup on AWS](#aws_standalone)
	- [Spark Scripts](#spark_scripts)
	- [Submitting Spark Scripts](#submit_spark_scripts)
	- [Storing and Retrieving Data on the Cloud - S3](#storing_S3)
	- [HDFS and Spark](#hdfs_spark)
	- [Storing and Retrieving Data on the Cloud - HDFS](#storing_hdfs)
	- [Debugging - Data Errors](#debug_data_errors)
	- [Problem of Data Skew](#data_skew)
	- [Sparkify Key Metrics and Cohort Analysis](#spark_key_metrics)
	- [Troubleshooting Other Spark Issues](#troubleshooting)


- [Setup Instructions](#Setup_Instructions)
- [Acknowledgments](#Acknowledgments)
- [Further Links](#Further_Links)

# The Power of Spark <a name="power_of_spark"></a>

## Numbers Everyone should know <a name="numbers_to_know"></a>
- Understanding harware components is the key to know if a task/dataset is a "big data" problem or if it's easier to analyze the data locally on your own computer.

	![image1]

## Review of the hardware behind big data <a name="review_hardware"></a>

***CPU (Central Processing Unit)***
- The CPU is the "brain" of the computer. Every process on your computer is eventually handled by your CPU. This includes calculations and also instructions for the other components of the compute.

- Suppose you have 6000 tweets persecond. Each tweet has 200 bytes. Then you have data about 1.2 million bytes per second. A typical CPU can make 2.5 Billion Operations per second (per core). 

- So wrt CPU this is no problem for a single machine

	![image2]

***Memory (RAM)***
- When your program runs, data gets temporarily stored in memory before getting sent to the CPU. Memory is ephemeral storage - when your computer shuts down, the data in the memory is lost.
- It takes 250 longes to find and load a random byte from memory than to process that same byte with CPU.

- Via distributed systems, memory itensive task can be distributed/shared to many nodes.

	![image3]

***Storage (SSD or Magnetic Disk)***
- Storage is used for keeping data over long periods of time. When a program runs, the CPU will direct the memory to temporarily load data from long-term storage.

- Much cheaper but much slower than RAM. 

	| Header One     | Compared to RAM     |
	| :------------- | :------------- |
	| Magnetic Disk       | 200 times slower     |
	| SSD       | 15 times slower     |

	![image4]

***Network (LAN or the Internet)***
- Network is the gateway for anything that you need that isn't stored on your computer. The network could connect to other computers in the same room (a Local Area Network) or to a computer on the other side of the world, connected over the internet.

- Normally, processing takes 20 times longer when you have to download it from another machine.

- Minimize shuffling data back and forth across different computers


***Other Numbers to Know?***
- Other numbers are L1 and L2 Cache, mutex locking, and branch mispredicts. 

- Check out [Peter Norvig's original blog post](http://norvig.com/21-days.html) from a few years ago, 

- Check out this link - an [interactive version for today's current hardware](https://colin-scott.github.io/personal_website/research/interactive_latency.html).


## When is data it big data? <a name="Big_Data"></a>

### Problem when small data becomes big
- When Data is small, the data fits perfect into the memory of the machine and a simple Python program is perfect to solve the task
- However things can change when the amount of data is getting larger and larger

	![image5]

### Medium Data numbers
- If a dataset is larger than the size of your RAM, you might still be able to analyze the data on a single computer. By default, the Python pandas library will read in an entire dataset from disk into memory. If the dataset is larger than your computer's memory, the program won't work.

- However, the Python pandas library can ***read in a file in smaller chunks***. Thus, if you were going to calculate summary statistics about the dataset such as a sum or count, you could read in a part of the dataset at a time and accumulate the sum or count.

- [Iterating through files chunk by chunk](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#io-chunking)

	![image6]

### Must be Big Data automatically a large dataset?
- Is it only big data, when the data does not fit into the memory?
- No. Even 2GB could Big Data, e.g. training a Deep Learning model. This could be easier with more hardware. There is no simple definition for Big Data.


## Introduction to distributed systems <a name="intro_distr_sys"></a>
***Distributed systems***
- Distributed systems originally referred to computer networks.
- Each node has its own memory and processor
- Communication between npdes via messages

***Parallel computing***
- Its a tightly coupled distributing computing
- However, here all processors share one memory 

	![image7]

## The Hadoop Ecosystem <a name="hadoop"></a>

**Hadoop** - an ecosystem of tools for big data storage and data analysis. Hadoop is ***an older system than Spark*** but is still used by many companies. The major difference between Spark and Hadoop is how they use memory. Hadoop ***writes intermediate results to disk*** whereas Spark ***tries to keep data in memory whenever possible***. This makes Spark faster for many use cases.

- **Hadoop MapReduce** - a system for processing and analyzing large data sets in parallel. Normally people mean this tool when they are talking about Hadoop.

- **Hadoop YARN** - a resource manager that schedules jobs across a cluster. The manager keeps track of what computer resources are available and then assigns those resources to specific tasks.

- **Hadoop Distributed File System (HDFS)** - a big data storage system that splits data into chunks and stores the chunks across a cluster of computers.

**As Hadoop matured**, other tools were developed to make Hadoop easier to work with. These tools included:

- **Apache Pig** - a SQL-like language that runs on top of Hadoop MapReduce
- **Apache Hive** - another SQL-like interface that runs on top of Hadoop MapReduce


**How is Spark related to Hadoop?**
- Spark, which is the main focus of this course, is another big data framework. Spark contains libraries for data analysis, machine learning, graph analysis, and streaming live data. Spark is generally faster than Hadoop. This is because Hadoop writes intermediate results to disk whereas Spark tries to keep intermediate results in memory whenever possible.

- The Hadoop ecosystem includes a distributed file storage system called HDFS (Hadoop Distributed File System). Spark, on the other hand, does not include a file storage system. You can use Spark on top of HDFS but you do not have to. Spark can read in data from other sources as well such as Amazon S3.

	![image8]

- **Streaming Data** - Data streaming is a specialized topic in big data. The use case is when you want to store and analyze data in real-time such as Facebook posts or Twitter tweets.
Spark has a streaming library called [Spark Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.html) although it is not as popular and fast as some other streaming libraries. Other popular streaming libraries include Storm and Flink. Streaming won't be covered in this course, but you can follow these links to learn more about these technologies.

	![image9]

## Map Reduce <a name="map_reduce"></a>

- **MapReduce** is a programming technique for manipulating large data sets. "Hadoop MapReduce" is a specific implementation of this programming technique.

- The technique works by first ***dividing up a large dataset*** and ***distributing the data across a cluster***. In the ***map step***, each ***data is analyzed and converted into a (key, value) pair***. Then these key-value pairs are shuffled across the cluster so that all keys are on the same machine. In the ***reduce step***, the values with the same keys are ***combined*** together.

- While ***Spark doesn't implement MapReduce***, you can write Spark programs that behave in a similar way to the map-reduce paradigm. In the next section, you will run through a code example.

	![image10]

- Open Jupyte Notebook ```MapReduce.ipynb```

	```
	# Install mrjob library. This package is for running MapReduce jobs with Python
	# In Jupyter notebooks, "!" runs terminal commands from inside notebooks 

	! pip install mrjob
	```

	```
	%%file wordcount.py
	# %%file is an Ipython magic function that saves the code cell as a file

	from mrjob.job import MRJob # import the mrjob library

	class MRSongCount(MRJob):
		
		# the map step: each line in the txt file is read as a key, value pair
		# in this case, each line in the txt file only contains a value but no key
		# _ means that in this case, there is no key for each line
		def mapper(self, _, song):
			# output each line as a tuple of (song_names, 1) 
			yield (song, 1)

		# the reduce step: combine all tuples with the same key
		# in this case, the key is the song name
		# then sum all the values of the tuple, which will give the total song plays
		def reducer(self, key, values):
			yield (key, sum(values))
			
	if __name__ == "__main__":
		MRSongCount.run()
	```

	```
	# run the code as a terminal command
	! python wordcount.py songplays.txt
	```

## The Spark Cluster <a name="spark_cluster"></a>
- Each node of a cluster is responsible for a set of operations on a subset of the data
- How do the nodes know which task to run and in which order?
- Hirarchy: Master-Worker
- Master Node: Orchestrating the tasks across the cluster
- Worker Nodes: Performing the actual computations

- There are ***4 nodes*** tos setup Spark:
- **Local Mode** 
	- Everything happens on a single machine 
	- No real distributed computing
	- Usefuk to learn syntax and for prototyping
- **Cluster Modes**: - **Standalone** - **Yarn** - **Mesos**
	- Distributed computing
	- Yarn and Mesos implement a **Cluster Manager**
	- Standalone implements a **Driver** program. It acts as the master
	- Cluster Manager monitors available resources
	- Makes sure that all machines are responsive during the job
	- Yarn and Mesos are usefull when you are sharing a cluster with a team


	![image11]

## Common Spark use cases <a name="common_spark_uses"></a>

Spark Use Cases and Resources
Here are a few resources about different Spark use cases:

- [Data Analytics](http://spark.apache.org/sql/)
- [Machine Learning](http://spark.apache.org/mllib/)
- [Streaming](http://spark.apache.org/streaming/)
- [Graph Analytics](http://spark.apache.org/graphx/)

	![image12]

## Other technologies in the big data ecosystem <a name="other_techs"></a>

### You Don't Always Need Spark
- Spark is meant for big data sets that cannot fit on one computer. But you don't need Spark if you are working on smaller data sets. In the cases of data sets that can fit on your local computer, there are many other options out there you can use to manipulate data such as:

	- [AWK](https://en.wikipedia.org/wiki/AWK) - a command line tool for manipulating text files
	- [R](https://www.r-project.org/) - a programming language and software environment for statistical computing
	- [Python PyData Stack](https://pydata.org/downloads/), which includes pandas, Matplotlib, NumPy, and scikit-learn among other libraries
	
- Sometimes, you can still use pandas on a single, local machine even if your data set is only a little bit larger than memory. Pandas can read data in chunks. Depending on your use case, you can filter the data and write out the relevant parts to disk.

- If the data is already stored in a relational database such as [MySQL](https://www.mysql.com/) or [Postgres](https://www.postgresql.org/), you can leverage SQL to extract, filter and aggregate the data. If you would like to leverage pandas and SQL simultaneously, you can use libraries such as [SQLAlchemy](https://www.sqlalchemy.org/), which provides an abstraction layer to manipulate SQL tables with generative Python expressions.

The most commonly used Python Machine Learning library is [scikit-learn](https://scikit-learn.org/stable/). It has a wide range of algorithms for classification, regression, and clustering, as well as utilities for preprocessing data, fine tuning model parameters and testing their results. However, if you want to use more complex algorithms - like deep learning - you'll need to look further. [TensorFlow](https://www.tensorflow.org/) and [PyTorch](https://pytorch.org/) are currently popular packages.

### Spark's Limitations
Spark has some limitation.

- ***Spark Streaming’s latency*** is at least 500 milliseconds since it operates on micro-batches of records, instead of processing one record at a time. Native streaming tools such as [Storm](http://storm.apache.org/), [Apex](https://apex.apache.org/), or [Flink](https://flink.apache.org/) can push down this latency value and might be more suitable for low-latency applications. Flink and Apex can be used for batch computation as well, so if you're already using them for stream processing, there's no need to add Spark to your stack of technologies.

- Another limitation of Spark is its ***selection of machine learning algorithms***. Currently, Spark only supports algorithms that scale linearly with the input data size. In general, deep learning is not available either, though there are many projects integrate Spark with Tensorflow and other deep learning tools.

### Hadoop versus Spark
- The Hadoop ecosystem is a slightly older technology than the Spark ecosystem. In general, Hadoop MapReduce is slower than Spark because Hadoop writes data out to disk during intermediate steps. However, many big companies, such as Facebook and LinkedIn, started using Big Data early and built their infrastructure around the Hadoop ecosystem.

W- hile Spark is great for iterative algorithms, there is not much of a performance boost over Hadoop MapReduce when doing simple counting. Migrating legacy code to Spark, especially on hundreds of nodes that are already in production, might not be worth the cost for the small performance boost.

- Beyond Spark for Storing and Processing Big Data
Keep in mind that Spark is not a data storage system, and there are a number of tools besides Spark that can be used to process and analyze large datasets.

- Sometimes it makes sense to use the power and simplicity of SQL on big data. For these cases, a new class of databases, know as NoSQL and NewSQL, have been developed.

- For example, newer database storage systems like [HBase](https://hbase.apache.org/) or [Cassandra](https://cassandra.apache.org/). There are also distributed SQL engines like [Impala](https://impala.apache.org/) and [Presto](https://prestodb.io/). Many of these technologies use query syntax.


# Data Wrangling with Spark <a name="data_wrangling"></a> 
##  Functional Style of Programming  <a name="func_prog"></a>
- Spark is written in a language called Scala
- However: There are Programming Interfaces even for Python --> PySpark
- But even in PySpark a funtional programming style is preferred against a procedual one.
- Even Python is not a functional programming language, the PySpark is written with functional programming principles in mind.
- Underneath the hood, the Python code uses py4j to make calls to the Java Virtual Machine (JVM).
	![image13]

## Why Functional Programming? <a name="why_func"></a>
- Functional programming is perfect for distributed systems
- If on machine crashes it will not affect other machines
- The crashed machine can be restarted independently
- In distributed systems functions shouldn't have side effects on variables outside their scope, since this could interfere with other functions running on your cluster
- In distributed systems you have to be careful with how you design your functions. Whenever some functions run on some input data it can alter it in the process.
- ***Write functions that preserve their inputs and avoid side effects. PURE FUNCTIONS***

## Example: Procedual Programming <a name="proc_prog"></a>
- Open Notebook ```procedural_prog.ipynb````
	```
	log_of_songs = [
        "Despacito",
        "Nice for what",
        "No tears left to cry",
        "Despacito",
        "Havana",
        "In my feelings",
        "Nice for what",
        "Despacito",
        "All the stars"
	]
	```
	```
	play_count = 0
	```
	```
	def count_plays(song_title):
		global play_count
		for song in log_of_songs:
			if song == song_title:
				play_count = play_count + 1
		return play_count
	```
	```
	count_plays("Despacito")
	Result: 3
	```
	```
	count_plays("Despacito")
	Result: 6
	```

## The Spark DAGs: Recipe for Data <a name="dag"></a>
- Every Spark functions makes a copy of its input data
- It never changes the original parent data. Spark is immutable
- Chaining functions to wrangle data. Each function accomplish a small chunk of the work. 
- Lazy Evaluation: Before Spark does anything wirh the data in the program, it first buils step by step directions of what functions and data it will need. --> It is like a recipe --> DIRECTED ACYCLICAL GRAPH (DAG)
- Sparks looks at the recipe before it mixes everything together in one big step.
- Spark builds the DAG from the code and waits till the last possible moment to get the data.
	![image14]

## Maps and Lambda Functions <a name="map_lambda"></a>
 - One of the most common functions is maps
 - Maps simply makes a copy of the original input data and transforms that copy to whatever function you put inside of the map
 - The term map comes from the mathematical concept ***mapping inputs to outputs***
 - Directions for the Data - telling each input how to get to the output

### Example: Functional Programming <a name="example_func"></a>
- Open Jupyter Notebook ```maps_and_lazy_evaluation.ipynb```
- After some initialization to use Spark in our notebook we convert a log of songs which is just a normal Python list, to a distributed dataset that Spark can use.
- This is the **SPARK CONTEXT OBJECT - sc** 
- SC has a method **PARALLIZE** that takes a Python object and distributes the object across the machines in your cluster
- Spark can use then its functional features on the Dataset
- Now: Let's do something with the Data - "**lower case**" the song title as a common preprocessing step to standardize data with the Python function **convert_songs_to_lowercase**
- Next: we apply **MAP** to convert_songs_to_lowercase on each song of the dataset
- So far Spark has not really converted the Songs to lowercase yet (**Lazy Evaluation**). 
- Maybe there are other processing steps, like removing punctuation 
- Spark will wait until the last minute to see if it can streamline its work and combine these into a single **stage** before getting the actual data
- If one wants to do some action with the data use the **COLLECT** function. It gathers the results from all the machines in the cluster back to the machine running this notebook. 
- Spark didn't mutate the original dataset. Spark made a copy of the dataset but left the original distributed song log with all of its uppercase letters.
- **Anonymous Functions**: It's a Python feature from functional programming --> **LAMBDA** function. This shortens the code. It's useful to put the Python lowercase function directly inside the map function
- lambda x : x.lower()
- left side is the input argument 
- right side: what you return 

### In short form:
- **sc** - Spark context object 
- **parallize** - distribute Python object across cluster machines
- **map** - apply a Python function to each dataset record
- **collect** - gathers results from all machines back to the notebook machine

	```
	import pyspark
	sc = pyspark.SparkContext(appName="maps_and_lazy_evaluation_example")

	log_of_songs = [
			"Despacito",
			"Nice for what",
			"No tears left to cry",
			"Despacito",
			"Havana",
			"In my feelings",
			"Nice for what",
			"despacito",
			"All the stars"
	]

	# parallelize the log_of_songs to use with Spark
	distributed_song_log = sc.parallelize(log_of_songs)
	```
	Convert song names to lowercase
	```
	def convert_song_to_lowercase(song):
		return song.lower()

	convert_song_to_lowercase("Havana")
	Result: 'havana'
	```
	The map step: The map step will go through each song in the list and apply the convert_song_to_lowercase() function. But not instantly --> LAZY EVALUATION
	```
	distributed_song_log.map(convert_song_to_lowercase)
	```
	To get Spark to actually run the map step, you need to use an "action". The collect() method takes the results from all of the clusters and "collects" them into a single list on the master node.
	```
	distributed_song_log.map(convert_song_to_lowercase).collect()

	Result:
	['despacito',
	'nice for what',
	'no tears left to cry',
	'despacito',
	'havana',
	'in my feelings',
	'nice for what',
	'despacito',
	'all the stars']
	```
	Note as well that Spark is not changing the original data set: Spark is merely making a copy. You can see this by running collect() on the original dataset.
	```
	distributed_song_log.collect()

	Result:
	['Despacito',
	'Nice for what',
	'No tears left to cry',
	'Despacito',
	'Havana',
	'In my feelings',
	'Nice for what',
	'despacito',
	'All the stars']
	```
	Use Lambda function to shorten code 
	```
	distributed_song_log.map(lambda song: song.lower()).collect()

	Result:
	['despacito',
	'nice for what',
	'no tears left to cry',
	'despacito',
	'havana',
	'in my feelings',
	'nice for what',
	'despacito',
	'all the stars']
	```

## Data Formats <a name="data_formats"></a>
- Before we can start any Data Wrangling we need to load Data into Spark
- Most common: CSV, JSON, HTML, XML

## Data Stores <a name="data_stores"></a>
- When we need distributed computing we have so much data that we need distributed storage as well. 
- Distributed file systems, many storage services and distributed databases store data in a fault tolerant way, i.e. if machine breaks or become unavailable, the collected information is not lost 
- Haddop has a Distributed File System - HDFS - to store data
- HDFS splits files into 64, 128 MB Blocks and replicates these blocks across the cluster. This way the data is stored in a fault tolerant way and can be accessed in digestible chunks.
- If we do not want to maintain our own cluster, use Amaton Simple Service Storage (S3)

	![image15]


## Spark Session <a name="spark_session"></a>
1. **SparkContext**: Main Entry point for Spark functionality and connects the cluster to the application
2. In case of Lower level usage: Create objects with SparkContext.
	Create a **SparkConf** object to specify some information about the application (name, master node's IP address)
3. To read DataFrames we need **SparkSession** (a Spark SQL equivalent). Similar to SparkConf we can specify some information about the application

	![image16]



## Reading and Writing to Spark Dataframes <a name="read_write_df"></a>
- Open Jupyter Notebook ```data_inputs_and_outputs.ipynb```
- Let's import and export data ro and from Spark dataframes.  
	1. Import **SparkSession**
	2. **getOrcreate()** - if session exists we will update it, if not a new one will be created

- ***Load*** a json file from HDFS on the cluster via ```user_log = spark.read.json(path)```
- user_log is a DataFrame
- print the Schema with the **printSchema()** method ```user_log.printSchema()```
- There are fileds describing the user (userID, firstName, lastName)
- There is also info about the request: page user accessed, HTTP method, status of request
- Use the **describe()** method to get info about the dataframe ```user_log.describe()```
- Take a look at a particular record e.g. the first with the **show()** method via ```user_log.show(n=1)``` 
- With the **take()** method you can grab the first few methods ```user_log.take(5)``` 
- ***Save*** data into a csv file via ```user_log.write.save(out_path, format = "csv", header=True)```
- ***Load*** the previously saved csv file into another datraframe via ```user_log_2 = spark.read.csv(out_path, header = True)``` 

	Import SparkConf and SparkSession
	```
	import pyspark
	from pyspark import SparkConf
	from pyspark.sql import SparkSession
	```
	Update some of the parameters, such as application's name
	```
	spark = SparkSession \
		.builder \
		.appName("Our first Python Spark SQL example") \
		.getOrCreate()	
	```
	Let's check if the change went through
	```
	spark.sparkContext.getConf().getAll()

	Result:
	[('spark.app.name', 'Our first Python Spark SQL example'),
	('spark.app.id', 'local-1614853873597'),
	('spark.driver.port', '39871'),
	('spark.rdd.compress', 'True'),
	('spark.serializer.objectStreamReset', '100'),
	('spark.master', 'local[*]'),
	('spark.executor.id', 'driver'),
	('spark.submit.deployMode', 'client'),
	('spark.driver.host', 'fbbb5c24867d'),
	('spark.ui.showConsoleProgress', 'true')]
	```
	```
	spark

	Result:
	SparkSession - in-memory

	SparkContext

	Spark UI

	Version
		v2.4.3
	Master
		local[*]
	AppName
		Our first Python Spark SQL example
	```
	Let's create our first dataframe from a fairly small sample data set. 
	```
	path = "data/sparkify_log_small.json"
	user_log = spark.read.json(path)
	user_log.printSchema()

	Result:
	root
	|-- artist: string (nullable = true)
	|-- auth: string (nullable = true)
	|-- firstName: string (nullable = true)
	|-- gender: string (nullable = true)
	|-- itemInSession: long (nullable = true)
	|-- lastName: string (nullable = true)
	|-- length: double (nullable = true)
	|-- level: string (nullable = true)
	|-- location: string (nullable = true)
	|-- method: string (nullable = true)
	|-- page: string (nullable = true)
	|-- registration: long (nullable = true)
	|-- sessionId: long (nullable = true)
	|-- song: string (nullable = true)
	|-- status: long (nullable = true)
	|-- ts: long (nullable = true)
	|-- userAgent: string (nullable = true)
	|-- userId: string (nullable = true)
	```
	Describe the dataframe
	```
	user_log.describe()

	Result:
	DataFrame[summary: string, artist: string, auth: string, firstName: string, gender: string, itemInSession: string, lastName: string, length: string, level: string, location: string, method: string, page: string, registration: string, sessionId: string, song: string, status: string, ts: string, userAgent: string, userId: string]
	```
	```
	user_log.show(n=1)

	Result:
	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	|       artist|     auth|firstName|gender|itemInSession|lastName|   length|level|            location|method|    page| registration|sessionId|                song|status|           ts|           userAgent|userId|
	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	|Showaddywaddy|Logged In|  Kenneth|     M|          112|Matthews|232.93342| paid|Charlotte-Concord...|   PUT|NextSong|1509380319284|     5132|Christmas Tears W...|   200|1513720872284|"Mozilla/5.0 (Win...|  1046|
	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	only showing top 1 row
	```
	```
	user_log.take(5)

	Result:
	[Row(artist='Showaddywaddy', auth='Logged In', firstName='Kenneth', gender='M', itemInSession=112, lastName='Matthews', length=232.93342, level='paid', location='Charlotte-Concord-Gastonia, NC-SC', method='PUT', page='NextSong', registration=1509380319284, sessionId=5132, song='Christmas Tears Will Fall', status=200, ts=1513720872284, userAgent='"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36"', userId='1046'),
	Row(artist='Lily Allen', auth='Logged In', firstName='Elizabeth', gender='F', itemInSession=7, lastName='Chase', length=195.23873, level='free', location='Shreveport-Bossier City, LA', method='PUT', page='NextSong', registration=1512718541284, sessionId=5027, song='Cheryl Tweedy', status=200, ts=1513720878284, userAgent='"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36"', userId='1000'),
	Row(artist='Cobra Starship Featuring Leighton Meester', auth='Logged In', firstName='Vera', gender='F', itemInSession=6, lastName='Blackwell', length=196.20526, level='paid', location='Racine, WI', method='PUT', page='NextSong', registration=1499855749284, sessionId=5516, song='Good Girls Go Bad (Feat.Leighton Meester) (Album Version)', status=200, ts=1513720881284, userAgent='"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.78.2 (KHTML, like Gecko) Version/7.0.6 Safari/537.78.2"', userId='2219'),
	Row(artist='Alex Smoke', auth='Logged In', firstName='Sophee', gender='F', itemInSession=8, lastName='Barker', length=405.99465, level='paid', location='San Luis Obispo-Paso Robles-Arroyo Grande, CA', method='PUT', page='NextSong', registration=1513009647284, sessionId=2372, song="Don't See The Point", status=200, ts=1513720905284, userAgent='"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36"', userId='2373'),
	Row(artist=None, auth='Logged In', firstName='Jordyn', gender='F', itemInSession=0, lastName='Jones', length=None, level='free', location='Syracuse, NY', method='GET', page='Home', registration=1513648531284, sessionId=1746, song=None, status=200, ts=1513720913284, userAgent='"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.94 Safari/537.36"', userId='1747')]
	```
	Save and Read again
	```
	out_path = "data/sparkify_log_small.csv"
	user_log.write.save(out_path, format="csv", header=True)
	user_log_2 = spark.read.csv(out_path, header=True)
	user_log_2.printSchema()

	...
	```

## Imperative vs Declarative programming <a name="imp_vs_decl"></a>
- Imperarive programming cares about **HOW?** --> How are we getting the result?
- Declarative programming cares about **WHAT?** --> What is the result?

- Declarative systems are an **Abstraction Layer** of an imperative system

	![image17]

## Data Wrangling with DataFrames <a name="wrangle_dataframes"></a>
- Open Jupyter Notebook ```data_wrangling.ipynb```

	### Import libraries, instantiate a SparkSession, and then read in the data set 
	```
	from pyspark.sql import SparkSession
	from pyspark.sql.functions import udf
	from pyspark.sql.types import StringType
	from pyspark.sql.types import IntegerType
	from pyspark.sql.functions import desc
	from pyspark.sql.functions import asc
	from pyspark.sql.functions import sum as Fsum

	import datetime

	import numpy as np
	import pandas as pd
	%matplotlib inline
	import matplotlib.pyplot as plt
	```
	```
	spark = SparkSession \
		.builder \
		.appName("Wrangling Data") \
		.getOrCreate()
	```
	```
	path = "data/sparkify_log_small.json"
	user_log = spark.read.json(path)
	```
	### EXPLORATE THE DATA
	```
	user_log.take(5)

	Result:
	[Row(artist='Showaddywaddy', auth='Logged In', firstName='Kenneth', gender='M', itemInSession=112, lastName='Matthews', length=232.93342, level='paid', location='Charlotte-Concord-Gastonia, NC-SC', method='PUT', page='NextSong', registration=1509380319284, sessionId=5132, song='Christmas Tears Will Fall', status=200, ts=1513720872284, userAgent='"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36"', userId='1046'),

	...
	]
	```
	```
	user_log.printSchema()

	Result:
	root
	|-- artist: string (nullable = true)
	|-- auth: string (nullable = true)
	|-- firstName: string (nullable = true)
	|-- gender: string (nullable = true)
	|-- itemInSession: long (nullable = true)
	|-- lastName: string (nullable = true)
	|-- length: double (nullable = true)
	|-- level: string (nullable = true)
	|-- location: string (nullable = true)
	|-- method: string (nullable = true)
	|-- page: string (nullable = true)
	|-- registration: long (nullable = true)
	|-- sessionId: long (nullable = true)
	|-- song: string (nullable = true)
	|-- status: long (nullable = true)
	|-- ts: long (nullable = true)
	|-- userAgent: string (nullable = true)
	|-- userId: string (nullable = true)
	```
	```
	user_log.describe().show()

	Result:
	see above --> complex to read
	```
	Take a look at individual columns with the following (here column artist)
	```
	user_log.describe("artist").show()

	Result:
	+-------+-----------------+
	|summary|           artist|
	+-------+-----------------+
	|  count|             8347|
	|   mean|            461.0|
	| stddev|            300.0|
	|    min|              !!!|
	|    max|ÃÂlafur Arnalds|
	+-------+-----------------+
	```
	A column of numeric type will look like this (1000 session id, max values 7144 etc.)
	```
	user_log.describe("sessionId").show()

	Result:
	+-------+------------------+
	|summary|         sessionId|
	+-------+------------------+
	|  count|             10000|
	|   mean|         4436.7511|
	| stddev|2043.1281541827557|
	|    min|                 9|
	|    max|              7144|
	+-------+------------------+
	```
	Check the number of rows
	```
	user_log.count()

	Result:
	10000
	```
	Check the sort of page requests by looking at the page field. Drop diblicates and sort it. User can log in/out, can visit the homepage, play a song etc. ...
	Submit Downgrade is interesting. These are users who choose to downgrade their paid account nto get a free one.
	```
	user_log.select("page").dropDuplicates().sort("page").show()

	Result:
	+----------------+
	|            page|
	+----------------+
	|           About|
	|       Downgrade|
	|           Error|
	|            Help|
	|            Home|
	|           Login|
	|          Logout|
	|        NextSong|
	|   Save Settings|
	|        Settings|
	|Submit Downgrade|
	|  Submit Upgrade|
	|         Upgrade|
	+----------------+
	```
	Get events with a particular user ID by filtering a particular value
	```
	user_log.select(["userId", "firstname", "page", "song"]).where(user_log.userId == "1046").collect()

	Result:
	[Row(userId='1046', firstname='Kenneth', page='NextSong', song='Christmas Tears Will Fall'),
	Row(userId='1046', firstname='Kenneth', page='NextSong', song='Be Wary Of A Woman'),
	Row(userId='1046', firstname='Kenneth', page='NextSong', song='Public Enemy No.1'),
	Row(userId='1046', firstname='Kenneth', page='NextSong', song='Reign Of The Tyrants'),
	Row(userId='1046', firstname='Kenneth', page='NextSong', song='Father And Son'),
	Row(userId='1046', firstname='Kenneth', page='NextSong', song='No. 5'),
	Row(userId='1046', firstname='Kenneth', page='NextSong', song='Seventeen'),
	Row(userId='1046', firstname='Kenneth', page='Home', song=None),
	Row(userId='1046', firstname='Kenneth', page='NextSong', song='War on war'),
	... ]
	```
	### Calculating Statistics by Hour
	First convert timestamps to datetime from epoch time to get the hour of the day. Create a user defined function called get_hour
	```
	get_hour = udf(lambda x: datetime.datetime.fromtimestamp(x / 1000.0).hour)
	```
	With this function we can add a new column called 'hour' 
	```
	user_log = user_log.withColumn("hour", get_hour(user_log.ts))
	```
	Take a look at the first record. Hour is 10pm for the first record
	```
	user_log.head()

	Result:
	Row(artist='Showaddywaddy', auth='Logged In', firstName='Kenneth', gender='M', itemInSession=112, lastName='Matthews', length=232.93342, level='paid', location='Charlotte-Concord-Gastonia, NC-SC', method='PUT', page='NextSong', registration=1509380319284, sessionId=5132, song='Christmas Tears Will Fall', status=200, ts=1513720872284, userAgent='"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36"', userId='1046', hour='22')
	```
	Now compute the next song page request and group by the hour column. Aggregate it by counting and order the resultsby hour
	```
	songs_in_hour = user_log.filter(user_log.page == "NextSong").groupby(user_log.hour).count().orderBy(user_log.hour.cast("float"))
	```
	```
	songs_in_hour.show()

	Result:
	+----+-----+
	|hour|count|
	+----+-----+
	|   0|  456|
	|   1|  454|
	|   2|  382|
	|   3|  302|
	|   4|  352|
	|   5|  276|
	|   6|  348|
	|   7|  358|
	|   8|  375|
	|   9|  249|
	|  10|  216|
	|  11|  228|
	|  12|  251|
	|  13|  339|
	|  14|  462|
	|  15|  479|
	|  16|  484|
	|  17|  430|
	|  18|  362|
	|  19|  295|
	+----+-----+
	only showing top 20 rows
	```
	Let's turn tzhe small Spark dataframe into a Pandas dataframe 
	```
	songs_in_hour_pd = songs_in_hour.toPandas()
	songs_in_hour_pd.hour = pd.to_numeric(songs_in_hour_pd.hour)
	```
	Use matplotlib to plot the graph
	```
	plt.scatter(songs_in_hour_pd["hour"], songs_in_hour_pd["count"])
	plt.xlim(-1, 24);
	plt.ylim(0, 1.2 * max(songs_in_hour_pd["count"]))
	plt.xlabel("Hour")
	plt.ylabel("Songs played");
	```
	![image18]

	
	### Drop Rows with Missing Values
	With dropna-any method we drop all records that have none in the subset user_ID and sessionId
	```
	user_log_valid = user_log.dropna(how = "any", subset = ["userId", "sessionId"])
	```
	```
	user_log_valid.count()

	Result:
	10000
	```
	As you see, it turns out there are no missing values in the userID or session columns. But there are userID values that are empty strings.
	```
	user_log.select("userId").dropDuplicates().sort("userId").show()

	Result:
	+------+
	|userId|
	+------+
	|      |
	|    10|
	|   100|
	|  1000|
	|  1003|
	|  1005|
	|  1006|
	|  1017|
	|  1019|
	|  1020|
	|  1022|
	|  1025|
	|  1030|
	|  1035|
	|  1037|
	|   104|
	|  1040|
	|  1042|
	|  1043|
	|  1046|
	+------+
	only showing top 20 rows
	```
	Let's filter out rows with empty string 
	```
	user_log_valid = user_log_valid.filter(user_log_valid["userId"] != "")
	```
	Count values again
	```
	user_log_valid.count()
	```
	### Users Downgrade Their Accounts
	Find when users downgrade their accounts and then flag those log entries. Then use a window function and cumulative sum to distinguish each user's data as either pre or post downgrade events.

	So first let's find the users who downgraded their service
	```
	user_log_valid.filter("page = 'Submit Downgrade'").show()

	Result: Kelly downgraded her service

	+------+---------+---------+------+-------------+--------+------+-----+--------------------+------+----------------+-------------+---------+----+------+-------------+--------------------+------+----+
	|artist|     auth|firstName|gender|itemInSession|lastName|length|level|            location|method|            page| registration|sessionId|song|status|           ts|           userAgent|userId|hour|
	+------+---------+---------+------+-------------+--------+------+-----+--------------------+------+----------------+-------------+---------+----+------+-------------+--------------------+------+----+
	|  null|Logged In|    Kelly|     F|           24|  Newton|  null| paid|Houston-The Woodl...|   PUT|Submit Downgrade|1513283366284|     5931|null|   307|1513768454284|Mozilla/5.0 (Wind...|  1138|  11|
	+------+---------+---------+------+-------------+--------+------+-----+--------------------+------+----------------+-------------+---------+----+------+-------------+--------------------+------+----+
	```
	Take a look at her user activity. Via the level column one can see 
	```
	user_log.select(["userId", "firstname", "page", "level", "song"]).where(user_log.userId == "1138").collect()

	Result:
	[Row(userId='1138', firstname='Kelly', page='Home', level='paid', song=None),
	Row(userId='1138', firstname='Kelly', page='NextSong', level='paid', song='Everybody Everybody'),
	Row(userId='1138', firstname='Kelly', page='NextSong', level='paid', song='Gears'),
	Row(userId='1138', firstname='Kelly', page='NextSong', level='paid', song='Use Somebody'),
	
	...
	
	Row(userId='1138', firstname='Kelly', page='NextSong', level='paid', song='The Razor (Album Version)'),
	Row(userId='1138', firstname='Kelly', page='NextSong', level='paid', song='Idols and Anchors'),
	Row(userId='1138', firstname='Kelly', page='Downgrade', level='paid', song=None),
	Row(userId='1138', firstname='Kelly', page='Submit Downgrade', level='paid', song=None),
	Row(userId='1138', firstname='Kelly', page='Home', level='free', song=None),
	Row(userId='1138', firstname='Kelly', page='NextSong', level='free', song='Bones'),
	Row(userId='1138', firstname='Kelly', page='Home', level='free', song=None),
	Row(userId='1138', firstname='Kelly', page='NextSong', level='free', song='Grenouilles Mantidactylus (Small Frogs)')]
	```
	Create a new column which divides events of a particular user based on this special downgrade event --> let's call the column phase. The user have a differnt value in the phase column before and after downgrading

	To get phase flag records that refer to that special event. Create a lambda function which returns 1 to downgrade and zero otherwise
	```
	flag_downgrade_event = udf(lambda x: 1 if x == "Submit Downgrade" else 0, IntegerType())
	```
	Create a column downgraded
	```
	user_log_valid = user_log_valid.withColumn("downgraded", flag_downgrade_event("page"))
	```
	```
	user_log_valid.head()

	Result:
	Row(artist='Showaddywaddy', auth='Logged In', firstName='Kenneth', gender='M', itemInSession=112, lastName='Matthews', length=232.93342, level='paid', location='Charlotte-Concord-Gastonia, NC-SC', method='PUT', page='NextSong', registration=1509380319284, sessionId=5132, song='Christmas Tears Will Fall', status=200, ts=1513720872284, userAgent='"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36"', userId='1046', hour='22', downgraded=0)
	```
	Now compute 'phase'
	```
	from pyspark.sql import Window
	```
	Define a window function, where we partition (it's similar to group_by) by user_id, order events in a descending time order, take into account all previous rows, but no rows afterwards with 0.
	```
	windowval = Window.partitionBy("userId").orderBy(desc("ts")).rangeBetween(Window.unboundedPreceding, 0)
	```
	```
	user_log_valid = user_log_valid.withColumn("phase", Fsum("downgraded").over(windowval))
	```
	```
	user_log_valid.select(["userId", "firstname", "ts", "page", "level", "phase"]).where(user_log.userId == "1138").sort("ts").collect()	

	Result:
	[Row(userId='1138', firstname='Kelly', ts=1513729066284, page='Home', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513729066284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513729313284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513729552284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513729783284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513730001284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513730263284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513730518284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513730768284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513731182284, page='NextSong', level='paid', phase=1),

	...

	 Row(userId='1138', firstname='Kelly', ts=1513767413284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513767643284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513768012284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513768242284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513768452284, page='NextSong', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513768453284, page='Downgrade', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513768454284, page='Submit Downgrade', level='paid', phase=1),
	Row(userId='1138', firstname='Kelly', ts=1513768456284, page='Home', level='free', phase=0),
	Row(userId='1138', firstname='Kelly', ts=1513814880284, page='NextSong', level='free', phase=0),
	Row(userId='1138', firstname='Kelly', ts=1513821430284, page='Home', level='free', phase=0),
	Row(userId='1138', firstname='Kelly', ts=1513833144284, page='NextSong', level='free', phase=0)]
	```

## Wrangling Data Functions Overview <a name="wrangle_data_fun_overview"></a>
### General Functions
- **select()**: returns a new DataFrame with the selected columns
- **filter()**: filters rows using the given condition
- **where()**: is just an alias for filter()
- **groupBy()**: groups the DataFrame using the specified columns, so we can run aggregation on them
- **sort()**: returns a new DataFrame sorted by the specified column(s). By default the second parameter 'ascending' is True.
- **dropDuplicates()**: returns a new DataFrame with unique rows based on all or just a subset of columns
- **withColumn()**: returns a new DataFrame by adding a column or replacing the existing column that has the same name. The first parameter is the name of the new column, the second is an expression of how to compute it.

### Aggregate Functions:
- Spark SQL provides built-in methods for the most common aggregations such as **count()**, **countDistinct()**, **avg()**, **max()**, **min()**, etc. in the **pyspark.sql.functions module**. These methods are not the same as the built-in methods in the Python Standard Library, where we can find min() for example as well, hence you need to be careful not to use them interchangeably.

- In many cases, there are multiple ways to express the same aggregations. For example, if we would like to compute one type of aggregate for one or more columns of the DataFrame we can just simply chain the aggregate method after a groupBy(). If we would like to use different functions on different columns, **agg()** comes in handy. For example **agg({"salary": "avg", "age": "max"})** computes the **average salary and maximum age**.

### User defined functions (UDF)
- In Spark SQL we can define our own functions with the **udf** method from the **pyspark.sql.functions module**. The default type of the returned variable for UDFs is string. If we would like to return an other type we need to explicitly do so by using the different types from the pyspark.sql.types module.

### Window functions
- Window functions are a way of **combining the values of ranges of rows in a DataFrame**. When defining the window we can **choose how to sort and group** (with the **partitionBy** method) the rows and how wide of a window we'd like to use (described by rangeBetween or rowsBetween).

For further information see the [Spark SQL, DataFrames and Datasets Guide](https://spark.apache.org/docs/latest/sql-programming-guide.html) and the [Spark Python API Docs](https://spark.apache.org/docs/latest/api/python/index.html).

## Some important aggregate function using agg <a name="agg_func"></a>
- min and max
	```
	max_desc_len =df.agg({"DescLength": "max"}).collect()[0][0]
	max_desc_len

	Result:
	7521
	```
	Or use:
	```
	df.agg(max("DescLength")).show()

	Result:
	7521
	```
	```
	min_desc_len = df.agg({"DescLength": "min"}).collect()[0][0]
	min_desc_len

	Reusult:
	4
	```
	Or Use
	```
	df.agg(min("DescLength")).show()

	Reusult:
	4
	```
- mean
	```
	df.agg(mean("DescLength")).show()
	```
- stddev
	```
	df.agg(stddev("DescLength")).show()
	```
- mean and stdev
	```
	df.agg(avg("DescLength"), stddev("DescLength")).show()

	Result:
	+---------------+-----------------------+
	|avg(DescLength)|stddev_samp(DescLength)|
	+---------------+-----------------------+
	|      180.28187|     192.10819533505023|
	+---------------+-----------------------+
	```
- avg
	```
	df.agg(avg("DescLength")).show()
	```
- Chaining aggregates:
	```
	df.groupby("DescGroup").agg(avg(col("DescLength")), avg(col("NumTags")), count(col("DescLength"))).orderBy("avg(DescLength)").show()

	```

## Difference between collect(), show(), take() <a name="differences_spark_collect_show_take"></a>

- df.show() - shows only content
	```
	df.show()
	
	Result:
	+----+-------+
	| age|   name|
	+----+-------+
	|null|Michael|
	|  30|   Andy|
	|  19| Justin|
	+----+-------+
	```
- df.collect() - shows content and structure/metadata 
	```
	df.collect()
	
	Result:
	[Row(age=None, name=u'Michael'),
	Row(age=30, name=u'Andy'),
	Row(age=19, name=u'Justin')]
	```
- df.take(2) -  to see only first two rows of the dataframe
	```
	df.take(2)
	
	Result:
	[Row(age=None, name=u'Michael'), Row(age=30, name=u'Andy')]
	```

## Spark SQL <a name="spark_sql"></a>
- Declarative Appoach of Programming
- [Spark SQL built-in functions](https://spark.apache.org/docs/latest/api/sql/index.html)
- [Spark SQL guide](https://spark.apache.org/docs/latest/sql-getting-started.html)
- Open Jupyter notebook ```data_wrangling_sql.ipynb```
- Further queries: Open Jupyter Notebook ```spark_sql_quiz_solution.ipynb```
	```
	from pyspark.sql import SparkSession
	from pyspark.sql.functions import udf
	from pyspark.sql.types import StringType
	from pyspark.sql.types import IntegerType
	from pyspark.sql.functions import desc
	from pyspark.sql.functions import asc
	from pyspark.sql.functions import sum as Fsum

	import datetime

	import numpy as np
	import pandas as pd
	%matplotlib inline
	import matplotlib.pyplot as plt
	```
	```
	spark = SparkSession \
    	.builder \
		.appName("Data wrangling with Spark SQL") \
		.getOrCreate()
	```
	```
	path = "data/sparkify_log_small.json"
	user_log = spark.read.json(path)
	```
	```
	user_log.take(1)

	Result:
	[Row(artist='Showaddywaddy', auth='Logged In', firstName='Kenneth', gender='M', itemInSession=112, lastName='Matthews', length=232.93342, level='paid', location='Charlotte-Concord-Gastonia, NC-SC', method='PUT', page='NextSong', registration=1509380319284, sessionId=5132, song='Christmas Tears Will Fall', status=200, ts=1513720872284, userAgent='"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36"', userId='1046')]
	```
	```
	user_log.printSchema()

	Result:
	root
	|-- artist: string (nullable = true)
	|-- auth: string (nullable = true)
	|-- firstName: string (nullable = true)
	|-- gender: string (nullable = true)
	|-- itemInSession: long (nullable = true)
	|-- lastName: string (nullable = true)
	|-- length: double (nullable = true)
	|-- level: string (nullable = true)
	|-- location: string (nullable = true)
	|-- method: string (nullable = true)
	|-- page: string (nullable = true)
	|-- registration: long (nullable = true)
	|-- sessionId: long (nullable = true)
	|-- song: string (nullable = true)
	|-- status: long (nullable = true)
	|-- ts: long (nullable = true)
	|-- userAgent: string (nullable = true)
	|-- userId: string (nullable = true)
	```
	### Create a view and run queries
	The code below creates a temporary view against which you can run SQL queries.
	It updates a view if one already exists or creates a new one if not. It is temporarily. By closing the notebook it is gone.
	```
	user_log.createOrReplaceTempView("user_log_table")
	```
	```
	spark.sql("SELECT * FROM user_log_table LIMIT 2").show()

	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	|       artist|     auth|firstName|gender|itemInSession|lastName|   length|level|            location|method|    page| registration|sessionId|                song|status|           ts|           userAgent|userId|
	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	|Showaddywaddy|Logged In|  Kenneth|     M|          112|Matthews|232.93342| paid|Charlotte-Concord...|   PUT|NextSong|1509380319284|     5132|Christmas Tears W...|   200|1513720872284|"Mozilla/5.0 (Win...|  1046|
	|   Lily Allen|Logged In|Elizabeth|     F|            7|   Chase|195.23873| free|Shreveport-Bossie...|   PUT|NextSong|1512718541284|     5027|       Cheryl Tweedy|   200|1513720878284|"Mozilla/5.0 (Win...|  1000|
	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	```
	```
	spark.sql('''
          SELECT * 
          FROM user_log_table 
          LIMIT 2
          '''
          ).show()

	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	|       artist|     auth|firstName|gender|itemInSession|lastName|   length|level|            location|method|    page| registration|sessionId|                song|status|           ts|           userAgent|userId|
	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	|Showaddywaddy|Logged In|  Kenneth|     M|          112|Matthews|232.93342| paid|Charlotte-Concord...|   PUT|NextSong|1509380319284|     5132|Christmas Tears W...|   200|1513720872284|"Mozilla/5.0 (Win...|  1046|
	|   Lily Allen|Logged In|Elizabeth|     F|            7|   Chase|195.23873| free|Shreveport-Bossie...|   PUT|NextSong|1512718541284|     5027|       Cheryl Tweedy|   200|1513720878284|"Mozilla/5.0 (Win...|  1000|
	+-------------+---------+---------+------+-------------+--------+---------+-----+--------------------+------+--------+-------------+---------+--------------------+------+-------------+--------------------+------+
	```
	```
	spark.sql('''
          SELECT COUNT(*) 
          FROM user_log_table 
          '''
          ).show()
	
	Result:
	+--------+
	|count(1)|
	+--------+
	|   10000|
	+--------+
	```
	```
	spark.sql('''
          SELECT userID, firstname, page, song
          FROM user_log_table 
          WHERE userID == '1046'
          '''
          ).collect()
	
	Result:
	[Row(userID='1046', firstname='Kenneth', page='NextSong', song='Christmas Tears Will Fall'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='Be Wary Of A Woman'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='Public Enemy No.1'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='Reign Of The Tyrants'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='Father And Son'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='No. 5'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='Seventeen'),
	Row(userID='1046', firstname='Kenneth', page='Home', song=None),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='War on war'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='Killermont Street'),
	Row(userID='1046', firstname='Kenneth', page='NextSong', song='Black & Blue'),

	...
	]
	```
	DISTINCT is like drop_dublicates
	```
	spark.sql('''
          SELECT DISTINCT page
          FROM user_log_table 
          ORDER BY page ASC
          '''
          ).show()

	Result:
	+----------------+
	|            page|
	+----------------+
	|           About|
	|       Downgrade|
	|           Error|
	|            Help|
	|            Home|
	|           Login|
	|          Logout|
	|        NextSong|
	|   Save Settings|
	|        Settings|
	|Submit Downgrade|
	|  Submit Upgrade|
	|         Upgrade|
	+----------------+
	```
	### User Defined Functions
	In contrast to the imperative approach, here udfs have to be registered
	```
	spark.udf.register("get_hour", lambda x: int(datetime.datetime.fromtimestamp(x / 1000.0).hour))
	```
	```
	spark.sql('''
          SELECT *, get_hour(ts) AS hour
          FROM user_log_table 
          LIMIT 1
          '''
          ).collect()

	Result
	[Row(artist='Showaddywaddy', auth='Logged In', firstName='Kenneth', gender='M', itemInSession=112, lastName='Matthews', length=232.93342, level='paid', location='Charlotte-Concord-Gastonia, NC-SC', method='PUT', page='NextSong', registration=1509380319284, sessionId=5132, song='Christmas Tears Will Fall', status=200, ts=1513720872284, userAgent='"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36"', userId='1046', hour='22')]
	```
	```
	songs_in_hour = spark.sql('''
          SELECT get_hour(ts) AS hour, COUNT(*) as plays_per_hour
          FROM user_log_table
          WHERE page = "NextSong"
          GROUP BY hour
          ORDER BY cast(hour as int) ASC
          '''
          )
	```
	```
	songs_in_hour.show()

	Result:
	+----+--------------+
	|hour|plays_per_hour|
	+----+--------------+
	|   0|           456|
	|   1|           454|
	|   2|           382|
	|   3|           302|
	|   4|           352|
	|   5|           276|
	|   6|           348|
	|   7|           358|
	|   8|           375|
	|   9|           249|
	|  10|           216|
	|  11|           228|
	|  12|           251|
	|  13|           339|
	|  14|           462|
	|  15|           479|
	|  16|           484|
	|  17|           430|
	|  18|           362|
	|  19|           295|
	+----+--------------+
	only showing top 20 rows
	```
	### Converting Results to Pandas
	```
	songs_in_hour_pd = songs_in_hour.toPandas()
	print(songs_in_hour_pd)

	Resut:
	   hour  plays_per_hour
	0     0             456
	1     1             454
	2     2             382
	3     3             302
	4     4             352
	5     5             276
	6     6             348
	7     7             358
	8     8             375
	9     9             249
	10   10             216
	11   11             228
	12   12             251
	13   13             339
	14   14             462
	15   15             479
	16   16             484
	17   17             430
	18   18             362
	19   19             295
	20   20             257
	21   21             248
	22   22             369
	23   23             375
	```


## RDDs <a name="rdds"></a>
- RDDs are a low-level abstraction of the data. In the first version of Spark, you worked directly with RDDs. You can think of RDDs as long lists distributed across various machines. You can still use RDDs as part of your Spark code although data frames and SQL are easier. This course won't go into the details of RDD syntax, but you can find some further explanation of the difference between RDDs and DataFrames in Databricks' [A Tale of Three Apache Spark APIs: RDDs, DataFrames, and Datasets](https://databricks.com/blog/2016/07/14/a-tale-of-three-apache-spark-apis-rdds-dataframes-and-datasets.html) blog post.

Here is a link to the Spark documentation's [RDD programming guide](https://spark.apache.org/docs/latest/rdd-programming-guide.html).
![image19]


# Debugging an Optimization <a name="debug_optimize"></a>

## Setup Instructions for AWS <a name="aws"></a>
- Amazon  offers Elastic MapReduce --> EMR
- EC2 instancies qith many big data technologies like Hadoop Spark etc. already installed and configured
- Switch to Amazon [AWS](https://aws.amazon.com/de/free/?trk=ps_a134p000003yhZ6AAI&trkCampaign=acq_paid_search_brand&sc_channel=ps&sc_campaign=acquisition_DACH&sc_publisher=google&sc_category=core&sc_country=DACH&sc_geo=EMEA&sc_outcome=Acquisition&sc_detail=amazon%20aws&sc_content=Amazon%20AWS_e&sc_matchtype=e&sc_segment=456911459028&sc_medium=ACQ-P|PS-GO|Brand|Desktop|SU|AWS|Core|DACH|EN|Text&s_kwcid=AL!4422!3!456911459028!e!!g!!amazon%20aws&ef_id=EAIaIQobChMInuHS54-X7wIVybTtCh2NDgNAEAAYASAAEgJzD_D_BwE:G:s&s_kwcid=AL!4422!3!456911459028!e!!g!!amazon%20aws&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc) 
- Create an SSH key pair to securely connect to the cluster
	- Services
	- EC2
	- Click on Create Key Pair
	- Provide a name like ***spark-cluster***
	- Check the downloaded ***pem file*** 
- Switch to EMR Service 
	- Provide a Cluster name, e.g. spark-udacity
	- Enable Logging to track errors
	- Keep default S3 location
	- Use Cluster Setting (Launch mode)
	- Select emr-5.20.0 
	- Use Spark: Spark 2.4.0 on Hadoop 2.8.5 YARN with Ganglia 3.7.2 and Zeppelin 0.8.0
	- Use m5.xlarge for Harware configuration 
		- M --> multitype family, 
		- R --> Ram, 
		- C --> CPU
		- 5 --> means 5th generation of hardware (cheaper and more powerful than previous ones)
		- xlarge --> size of the instance indicates the hardware quality 
		- small --> would have less CPU, memory, storage and networking than extra large
	- Pick the EC2 key pair (spark-cluster) to connect with th cluster


	![image20]
	![image21]

## Standalone setup on AWS <a name="aws_standalone"></a>
- Login to AWS in a rented Spark Cluster and using S3 Data Storage
	![image22]

## Spark Scripts <a name="spark_scripts"></a> 
- Use Spark scripts to automate updates 
	![image23]

## Submitting Spark Scripts  <a name="submit_spark_scripts"></a>  
- Be logged in to Hadoop EMR
- Open Terminal 
- The py script for submission
	![image24]

- The submission command
	![image25]


## Storing and Retrieving Data on the Cloud  - S3 <a name="storing_S3"></a>
- S3 is like "Dropbox" or "iCloud" for Big Data
	![image26]

- Open Notebook under Amazon EMR
	![image27]

- Set the loacation via ```s3n://...```
- Load data from S3 like this
	![image28]

# HDFS and Spark <a name="hdfs_spark"></a>
- By using S3 you separate data storage from the cluster
- Downside: You have to download across the network into the Spark cluster --> slow process bottleneck 
- HDFS is much faster: Store Data on the Spark Cluster 
- HDFS comes preinstalled on Spark - only little setup needed
- When Spark needs Data from HDFS it crabs the closest copy
- This reduces time that data travels around the network
- Downside: You have to maintain Data by yourself 
- S3 is much easier to handle 
	![image29]


## Storing and Retrieving Data on the Cloud  - HDFS <a name="storing_hdfs"></a>
- Let's do the same storage process with HDFS

	![image30]

- Write files to HDFS
- Open Terminal

	![image31]

- create a new folder on the cluster 
	![image32]
- use **copyFromLocal** to copy the json file to this folder
	![image33]

- Now the file is on the Cluster system
	![image34]

- Read Data from HDFS via ```hdfs:///...```
	![image35]

## Debugging - Data Errors  <a name="debug_data_errors"></a> 
-  Sometimes records can be broken like this
	![image36]

- As each node has their own copy of variables and print statements are also distributed to each worker debugging via print statements will not work. 
- Original values omn the driver remain unchanged
	![image37]

-  Use accumulators - these are like global variables for the entire cluster
	![image38]

- Use the web ui tool for debugging
	![image39]

## Problem of Data Skew #data_skew) <a name="data_skew"></a> 
- Sometimes 80% of your data comes from 20% of your users
	![image41]


## Troubleshooting Other Spark Issues <a name="troubleshooting"></a> 
You can debug based on error messages, loglines and stack traces.

Very common issue with Spark jobs that can be harder to address: everything working fine but just taking a very long time. So what do you do when your Spark job is (too) slow?

### Insufficient resources
Often while there are some possible ways of improvement, processing large data sets just takes a lot longer time than smaller ones even without any big problem in the code or job tuning. Using more resources, either by increasing the number of executors or using more powerful machines, might just not be possible. When you have a slow job it’s useful to understand:

How much data you’re actually processing (compressed file formats can be tricky to interpret) If you can decrease the amount of data to be processed by filtering or aggregating to lower cardinality, And if resource utilization is reasonable.

There are many cases where different stages of a Spark job differ greatly in their resource needs: loading data is typically I/O heavy, some stages might require a lot of memory, others might need a lot of CPU. Understanding these differences might help to optimize the overall performance. Use the Spark UI and logs to collect information on these metrics.

If you run into out of memory errors you might consider increasing the number of partitions. If the memory errors occur over time you can look into why the size of certain objects is increasing too much during the run and if the size can be contained. Also, look for ways of freeing up resources if garbage collection metrics are high.

Certain algorithms (especially ML ones) use the driver to store data the workers share and update during the run. If you see memory issues on the driver check if the algorithm you’re using is pushing too much data there.

### Data skew
If you drill down in the Spark UI to the task level you can see if certain partitions process significantly more data than others and if they are lagging behind. Such symptoms usually indicate a skewed data set. Consider implementing the techniques mentioned in this lesson:

Add an intermediate data processing step with an alternative key Adjust the spark.sql.shuffle.partitions parameter if necessary

The problem with data skew is that it’s very specific to a dataset. You might know ahead of time that certain customers or accounts are expected to generate a lot more activity but the solution for dealing with the skew might strongly depend on how the data looks like. If you need to implement a more general solution (for example for an automated pipeline) it’s recommended to take a more conservative approach (so assume that your data will be skewed) and then monitor how bad the skew really is.

### Inefficient queries
Once your Spark application works it’s worth spending some time to analyze the query it runs. You can use the Spark UI to check the DAG and the jobs and stages it’s built of.

Spark’s query optimizer is called Catalyst. While Catalyst is a powerful tool to turn Python code to an optimized query plan that can run on the JVM it has some limitations when optimizing your code. It will for example push filters in a particular stage as early as possible in the plan but won’t move a filter across stages. It’s your job to make sure that if early filtering is possible without compromising the business logic than you perform this filtering where it’s more appropriate.

It also can’t decide for you how much data you’re shuffling across the cluster. Remember from the first lesson how expensive sending data through the network is. As much as possible try to avoid shuffling unnecessary data. In practice, this means that you need to perform joins and grouped aggregations as late as possible.

When it comes to joins there is more than one strategy to choose from. If one of your data frames are small consider using broadcast hash join instead of a hash join.

Further reading
Debugging and tuning your Spark application can be a daunting task. There is an ever-growing community out there though, always sharing new ideas and working on improving Spark and its tooling, to make using it easier. So if you have a complicated issue don’t hesitate to reach out to others (via user mailing lists, forums, and Q&A sites).

You can find more information on tuning [Spark](https://spark.apache.org/docs/latest/tuning.html) and [Spark SQL](https://spark.apache.org/docs/latest/sql-performance-tuning.html) in the documentation.


## Sparkify Key Metrics and Cohort Analysis <a name="spark_key_metrics"></a> 
- ***Monthly active user***: The number of users who listend to at least one song in the past month. This gives you a better sense of how  many people are using your site on a regular basis
- This number is better than looking at the total number of accounts
- ***Daily active users***:  The number of users who listend to at least one song at each day on the last month. Both total number and percentage wrt user last month. Daily active users give you a hint of how engaged your users are.
- ***Total number of paid and unpaid users***
- ***Total Ads Served in the last month*** - This indicates the monthly revenue that Sparkify earns

	![image40]



## Setup Instructions <a name="Setup_Instructions"></a>
The following is a brief set of instructions on setting up a cloned repository.

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.



### Prerequisites: Installation of Python via Anaconda and Command Line Interaface <a name="Prerequisites"></a>
- Install [Anaconda](https://www.anaconda.com/distribution/). Install Python 3.7 - 64 Bit

- Upgrade Anaconda via
```
$ conda upgrade conda
$ conda upgrade --all
```

- Optional: In case of trouble add Anaconda to your system path. Write in your CLI
```
$ export PATH="/path/to/anaconda/bin:$PATH"
```

### Clone the project <a name="Clone_the_project"></a>
- Open your Command Line Interface
- Change Directory to your project older, e.g. `cd my_github_projects`
- Clone the Github Project inside this folder with Git Bash (Terminal) via:
```
$ git clone https://github.com/ddhartma/Spark-Big-Data-Analytics.git
```

- Change Directory
```
$ cd Spark-Big-Data-Analytics
```

- Create a new Python environment, e.g. spark_env. Inside Git Bash (Terminal) write:
```
$ conda create --name spark_env
```

- Activate the installed environment via
```
$ conda activate spark_env
```

- Install the following packages (via pip or conda)
```
numpy = 1.17.4
pandas = 0.24.2
pyspark
```

- Check the environment installation via
```
$ conda env list
```

## Acknowledgments <a name="Acknowledgments"></a>
* This project is part of the Udacity Nanodegree program 'Data Science'. Please check this [link](https://www.udacity.com) for more information.

## Further Links <a name="Further_Links"></a>

Git/Github
* [GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html)
* [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
* [5 types of Git workflows](https://buddy.works/blog/5-types-of-git-workflows)

Docstrings, DRY, PEP8
* [Python Docstrings](https://www.geeksforgeeks.org/python-docstrings/)
* [DRY](https://www.youtube.com/watch?v=IGH4-ZhfVDk)
* [PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)
