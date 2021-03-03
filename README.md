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

### Why Functional Programming?
- Functional programming is perfect for distributed systems
- If on machine crashes it will not affect other machines
- The crashed machine can be restarted independently

### Example: Procedual Programming:
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


## Setup Instructions <a name="Setup_Instructions"></a>
The following is a brief set of instructions on setting up a cloned repository.

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites: Installation of Python via Anaconda and Command Line Interaface <a name="Prerequisites"></a>
- Install [Anaconda](https://www.anaconda.com/distribution/). Install Python 3.7 - 64 Bit
- If you need a Command Line Interface (CLI) under Windows you could use [git](https://git-scm.com/). Under Mac OS use the pre-installed Terminal.

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
$ git clone https://github.com/ddhartma/Recommendation-Engines.git
```

- Change Directory
```
$ cd Recommendation-Engines
```

- Create a new Python environment, e.g. rec_eng. Inside Git Bash (Terminal) write:
```
$ conda create --name rec_eng
```

- Activate the installed environment via
```
$ conda activate rec_eng
```

- Install the following packages (via pip or conda)
```
numpy = 1.17.4
pandas = 0.24.2
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
