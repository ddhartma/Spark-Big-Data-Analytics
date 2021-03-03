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
	- [Common Spark use cases](#intro_distr_sys)
	- [Introduction to distributed systems](#common_spark_uses)
	- [Other technologies in the big data ecosystems](#other_techs)


- [Setup Instructions](#Setup_Instructions)
- [Acknowledgments](#Acknowledgments)
- [Further Links](#Further_Links)

## The Power of Spark <a name="power_of_spark"></a>

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
```

## Common Spark use cases <a name="common_spark_uses"></a>

## Other technologies in the big data ecosystem <a name="other_techs"></a>


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
