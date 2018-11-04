
## How to run : spark shell
	https://stackoverflow.com/questions/30139951/error-while-trying-to-run-spark


apache spark
	1) It is distributed cluster computing
	2) it can do the batch and real time processing.
	3) It is not 100% real time processing but it uses the micro batch processing. Still it will give you the real time processing feel.

spark abstractions and concepts
	1) Resilient distributed dataset
	2) Direct acylic graph
	3) spark context
	4) Transformations
	5) Actions

##why spark is faster than the hadoop,I case of system failure?

	Using the map/reduce operation, we can control the large cluster of many machines. Hadoop is famous because it is fault tolerant.Even if a machine fails, machine can recover from the failures.

	Okay, so that all sounds really great. Why don't we just use Hadoop? Why bother with Spark then? Well, fault-tolerance in Hadoop/MapReduce comes at a cost. Between each map and reduce step, 
	in order to recover from potential failures, Hadoop will shuffle its data over the network and write intermediate data to **disk**. So it's doing a lot of network and disk operations.

	we know that reading and writing to disk is actually 100 times slower. It's actually 100 times slower than in-memory. And network communication was up to 1,000,000 times slower than doing
	computations in-memory if you can. So that's a big difference.

	### Reasons to Speedup the spark 

	1) It will not write the intermediate output data into the disk like hadoop. This saves the **lot of IO time**. Instead it uses ideas from functional programming to deal with this problem 
	of latency.What spark tries to do is it tries to keep all of its data, or all of the data that is needed for computational as much as possible **in-memory**. And data always 
	immutable. And if I **remember the functional transformations** on the data. Now even if a failure occures we can get the

	So in particular, what's really cool about Spark is that it uses ideas from functional programming to deal with this problem of latency to try and get rid of writing to disk a lot and doing a
	lot of network communication. What spark tries to do is it tries to keep all of its data, or all of the data that is needed for computational as much as possible **in-memory**. And it's 
	always immutable, data is always immutable. If I can keep immutable data in-memory, and I use these nice, functional transformations, like we learned in Scala collections, like doing a map 
	on a list and get another list back.

	If we do these sorts of things we can build up chains of transformations on this immutable, functional data. So we can get full tolerance by just keeping track of the functional transformations 
	that we make on this functional data. And in order to figure out how to recompute a piece of data on a node that might have crashed, or to restart that work that was supposed to be done on 
	that node somewhere else, all we have to do is remember the transformations that were done to this immutable data. 

	Read that data in somewhere else or on the same node, and then replay those functional transformations over the original data set. So this is how Spark managed to figure out a way to 
	achieve fault tolerance without having to regularly write intermediate results to disk. So everything stays in-memory, and only when there's a failure is a bunch of network communication or 
	reading and writing operations actually done. 


## Transformations and actions ?
	When you apply some kind of filter operation on list, It will give you **collection of values** in a list --> This is called transformation.
	## Action :  When you apply some filter , If it returns single value --> we call it as action

	Actions are eager and transformations are lazy.

## Key Idea
	We need to first decide which data should present **in-memory**.
	Once decided we keep the data **in memory**, to avoid the lot of I/O.
	Put the in-memory : If it is reusable.

## Cluster topology
	Spark contains the driverProgram (master) and workerNode (executor). they communicate using cluster management like (YARN).
	When you are executing a code -> code is sent to the executors. We are sending code to executor because data is present at the executor. To avoid the I/O we send the code.
	## driver   
	          1) is where the main method of your program runs. 
		  2) the process running the code that creates the **Spark context**, creates **RDD's** and it stages up and it sends offtransformations and actions. 

	Spark application is a set of process running on a cluster.
	These processes that run computation and store the data for our application are **executors**.

	## Executors
		  1) Run the tasks that represent the application
		  2) Return computed result to driver
		  3) provide in-memory storage for cached RDD's.		 	

## Execution of spark program
	1) driver program run the application, create the **sparkContext**.
	2) Using the **sparkContext** connects to cluster management (For allocating resources)
	3) After that Spark requires executors on nodes in the clusters, which are processes that run computation and store data for your application.
	4) driver program will send the code to executors. (like apply the some transformations and action.)
	5) Finally, **sparkContext** sends the tasks for the executors to run.


	
------------------------------------------------------------------

To store the data, there are two approaches

	1) Scale up or vertical scalling      : 		
		Putting all the data into the single hard drive
		This will offer more response time
	
	2) Scale out or horizontal scaling    : 	
		putting the data into multiple machines
		This will offer less response time
		Machines may be failure. To avoid the failures we need the replication. 


HDFS Commands
	1) hdfs -dfs -ls -R -h						(listing the files)
	2) hdfs -dfs -du -h						(Disk usuage)
	3) hdfs -dfs -rm -r Directory_name				(Removing the files recursively in a folder)
	4) hdfs -dfs -touchz file_name					(creating a file)
	5) hdfs -dfs -put <source_file path> <hdfs destination>		(To place a file from local file system to hdfs file system)
	
HDFS Federation:
	1) In traditional hdfs: there is single namenode but incase of hdfs federation there are multiple namenodes and are independent to each other	
	2) It wil mainly useful when you have large cluster and single namenode then it will reduce the performance instead we can create the multiple namenodes in the cluster. No need to have 
	   co-ordination among the namesnode.
	3) This will add the scalability

NameNode architecture: 
	1) Name node keeps the the all metadata about the files in RAM.
	2) Block is designed as 128MB. Reason is Meta data size will reduce.
	3) If namenode fails --> entire cluster is unavailable. It is a single point of failure.
	4) To avoid the failures we can use (write ahead log). This will help during the recovery process.
	5) However Write ahead log is not enough to reproduce the namenode state.You should also have snapshot of memory at some point of time from which you can replay transaction stored in the edit log.
	6) If we use (write ahead log) it will take several hours to boot the system.As you can guess this is not appropriate for a high demand service.
	7) Because of above reason hadoop developers invented the secondary name node.
	 

		
