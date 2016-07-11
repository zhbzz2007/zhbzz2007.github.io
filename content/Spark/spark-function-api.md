---
title: Spark RDD及编程接口
layout: page
date: 2016-07-04 21:02
---

## 1.Spark RDD ##

RDD(Resilient Distributed Datasets),弹性分布式数据集，即一个RDD代表一个被分区的只读数据集。一个RDD的生成只有两种途径，一是来自内存集合和外部存储系统，另一种是通过转换操作来自于其它RDD，比如map、filter、join等等。

RDD没必要随时被实例化，由于RDD的接口只支持粗粒度的操作（即一个操作会被应用在RDD的所有数据上），所有只要通过记录下这些作用在RDD之上的转换操作，来构建RDD的继承关系（lineage），就可以有效地进行容错处理，而不需要将实际的RDD数据进行记录拷贝。这对于RDD来说是一项非常强大的功能，也即在一个Spark程序中，我们所用到的每一个RDD，在丢失或者操作失败后都是可以重建的。

开发者还可以对RDD进行另外两个方面的控制操作：持久化和分区。开发者可以指明它们需要重用哪些RDD，然后选择一种存储策略（如in-memory storage）将它们保存起来。开发者还可以让RDD根据记录中的键值在集群的机器之间重新分区。

抽象的RDD采用如下五个接口来表示一个分区的、高效容错的而且能够持久化的分布式数据集。

|RDD接口|作用|
|--|--|
|partition | 分区，一个RDD会有一个或者多个分区 |
|preferredLocations(p)|对于分区p而言，返回数据本地化计算的节点|
|dependencies()|RDD的依赖关系|
|compute(p,context)|对于分区p而言，进行迭代计算|
|partitioner()|RDD的分区函数|

### 1.1RDD分区(partitions) ###

对于RDD的分区而言，用户可以自行指定多少分区，如果没有指定，将会使用默认值。可以利用RDD的成员变量partitions所返回的partition数组的大小来查询一个RDD被划分的分区数。

    scala> val rdd = sc.parallelize(1 to 100, 2)//指定分区为2
    rdd: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[0] at parallelize at <console>:27

    scala> rdd.partitions.size
    res0: Int = 2

创建RDD的时候不指定分区，系统默认的数值就是这个程序所分配到的资源的CPU核的个数。

    scala> val rdd = sc.parallelize(1 to 100)//不指定分区
    rdd: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[2] at parallelize at <console>:27

    scala> rdd.partitions.size
    res2: Int = 4


### 1.2RDD优先位置(preferred Locations) ###

RDD优先位置于Spark中的调度有关，返回的是此RDD的每个partition所存储的位置，按照“移动数据不如移动计算”的理念，在Spark进行任务调度的时候，尽可能地将任务分配到数据块所存储的位置，若以从Hadoop中读取数据生成RDD为例，preferredLocations返回每一个数据块所在的机器名或者IP地址，如果每一块数据是多份存储的，那么就会返回多个机器地址。本例中以ml-100k数据为例，返回一个空的ListBuffer。

    scala> val rdd = sc.textFile("/home/zhb/Desktop/work/SparkData/ml-100k")
    rdd: org.apache.spark.rdd.RDD[String] = /home/zhb/Desktop/work/SparkData/ml-100k MapPartitionsRDD[4] at textFile at <console>:27

    scala> val rdd = sc.textFile("/home/zhb/Desktop/work/SparkData/ml-100k/u.user")
    rdd: org.apache.spark.rdd.RDD[String] = /home/zhb/Desktop/work/SparkData/ml-100k/u.user MapPartitionsRDD[6] at textFile at <console>:27

    scala> val hadoopRDD = rdd.dependencies(0).rdd
    hadoopRDD: org.apache.spark.rdd.RDD[_] = /home/zhb/Desktop/work/SparkData/ml-100k/u.user HadoopRDD[5] at textFile at <console>:27

    scala> hadoopRDD.partitions.size
    res3: Int = 2

    scala> hadoopRDD.preferredLocations(hadoopRDD.partitions(0))
    res4: Seq[String] = ListBuffer()

### 1.3RDD依赖关系(dependencies) ###

RDD是粗粒度的操作数据集，每个转换操作都会生成一个新的RDD，所以RDD之间就会形成类似于流水线一样的前后依赖关系，在Spark中存在两种类型的依赖，即窄依赖（Narrow Dependencies）和宽依赖（Wide Dependencies）。

窄依赖：每一个父RDD的分区最多只被一个子RDD的一个分区所使用。

宽依赖：多个子RDD的分区会依赖同一个父RDD的分区。

    scala> val rdd = sc.makeRDD(1 to 10)
    rdd: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[7] at makeRDD at <console>:27

    scala> val mapRDD = rdd.map(x => (x,x))
    mapRDD: org.apache.spark.rdd.RDD[(Int, Int)] = MapPartitionsRDD[8] at map at <console>:29

    scala> mapRDD.dependencies
    res5: Seq[org.apache.spark.Dependency[_]] = List(org.apache.spark.OneToOneDependency@5b79ff65)

    scala> val shuffleRDD = mapRDD.partitionBy(new org.apache.spark.HashPartitioner(3))
    shuffleRDD: org.apache.spark.rdd.RDD[(Int, Int)] = ShuffledRDD[9] at partitionBy at <console>:31

    scala> shuffleRDD.dependencies
    res6: Seq[org.apache.spark.Dependency[_]] = List(org.apache.spark.ShuffleDependency@17d42714)
    scala> val rdd = sc.makeRDD(collect)
rdd: org.apache.spark.rdd.RDD[scala.collection.immutable.Range.Inclusive] = ParallelCollectionRDD[1] at makeRDD at <console>:29

### 1.4RDD分区计算(compute) ###

Spark中每个RDD的计算都是以partition（分区）为单位的，而且RDD中的compute函数都是在迭代器进行复合，不需要保存每次计算的结果。

### 1.5RDD分区函数(partitioner) ###

partitioner就是RDD分区函数，目前Spark实现了两种类型的分区函数，即HashPartitioner（哈希分区）和RangePartitioner（区域分区），且partitioner这个属性只存在（K,V）类型的RDD中，对于非（K,V）类型的partitioner的值就是None。partitioner函数既决定了RDD本身的分区数量，也可作为其父RDD Shuffle输出（MapOutput）中每个分区进行数据切割的依据。

## 2.创建操作 ##

### 2.1集合创建操作 ###

RDD的形成可以由内部集合类型来生成，Spark中提供了parallelize和makeRDD两类函数来实现从集合生成RDD，两个函数接口功能类似，不同的是makeRDD还提供了一个可以指定每一个分区preferredLocations参数的实现版本。

    scala> val rdd = sc.makeRDD(1 to 10,3)
    rdd: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[0] at makeRDD at <console>:27

    scala> val collect = Seq((1 to 10,Seq("host1","host3")),(11 to 20,Seq("host2")))
    collect: Seq[(scala.collection.immutable.Range.Inclusive, Seq[String])] = List((Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10),List(host1, host3)), (Range(11, 12, 13, 14, 15, 16, 17, 18, 19, 20),List(host2)))

    scala> val rdd = sc.makeRDD(collect)
    rdd: org.apache.spark.rdd.RDD[scala.collection.immutable.Range.Inclusive] = ParallelCollectionRDD[1] at makeRDD at <console>:29

    scala> rdd.preferredLocations(rdd.partitions(0))
    res1: Seq[String] = List(host1, host3)

    scala> rdd.preferredLocations(rdd.partitions(1))
    res2: Seq[String] = List(host2)


### 2.2存储创建操作 ###

Spark的整个生态系统与Hadoop是完全兼容的，对于Hadoop所支持的文件类型或者数据库类型，Spark也同样支持。hadoopRDD和newhadoopRDD是最为抽象的两个函数，主要包括以下四个参数：

输入格式：指定数据输入的类型，如TextInputFormat;

键类型：指定[K,V]键值对中K的类型;

值类型：指定[K,V]键值对中V的类型;

分区值：指定由外部存储生成的RDD的partition数量的最小值，如果没有指定，系统会使用默认值defaultMinSplits;

兼容旧版本Hadoop API的创建操作，

||文件路径|输入格式|键类型|值类型|分区值|
|--|--|--|--|--|--|
|textFile|path|TextInputFormat|LongWritable|Text|minSplits|
|hadoopFile|path|F|K|V|minSplits|
|hadoopFile|path|F|K|V|DefaultMinSplits|
|sequenceFile|path|SequenceFileInputFormat|K|V|minSplits|
|sequenceFile|path|SequenceFileInputFormat|K|V|DefaultMinSplits|
|objectFile|path|SequenceFileInputFormat|NullWritable|BytesWritable|mminSplits|
|hadoopRDD|n/a|inpurformatClass|keyClass|valueClass|minSplits|

兼容新版本Hadoop API的创建操作，

||文件路径|输入格式|键类型|值类型|分区值|
|--|--|--|--|--|--|
|newAPIHadoopFile|path|F|K|V|n/a|
|newAPIHadoopFile|path|F|K|V|n/a|
|newAPIHadoopRDD|path|F|K|V|n/a|

## 3.转换操作 ##

### 3.1RDD基本转换操作 ###

map：将RDD中类型为T的元素，一对一映射为类型为U的元素。

distinct：返回RDD中所有不一样的元素。

flatMap：将RDD中的每一个元素进行一对多转换。

    scala> val rdd = sc.makeRDD(1 to 5,1)
    rdd: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[2] at makeRDD at <console>:27

    scala> val mapRDD = rdd.map(x => x.toFloat)
    mapRDD: org.apache.spark.rdd.RDD[Float] = MapPartitionsRDD[3] at map at <console>:29

    scala> mapRDD.collect()
    res3: Array[Float] = Array(1.0, 2.0, 3.0, 4.0, 5.0)

    scala> val flatMapRDD = rdd.flatMap(x => (1 to x))
    flatMapRDD: org.apache.spark.rdd.RDD[Int] = MapPartitionsRDD[4] at flatMap at <console>:29

    scala> flatMapRDD.collect()
    res4: Array[Int] = Array(1, 1, 2, 1, 2, 3, 1, 2, 3, 4, 1, 2, 3, 4, 5)

    scala> val distinctRDD = flatMapRDD.distinct()
    distinctRDD: org.apache.spark.rdd.RDD[Int] = MapPartitionsRDD[7] at distinct at <console>:31

    scala> distinctRDD.collect()
    res6: Array[Int] = Array(4, 1, 3, 5, 2)

repartition：repartition只是coalesce接口中shuffle为true的简易实现。

coalesce：主要讨论如何设置shuffle参数，这里分三种情况（假设RDD有N个分区，需要重新划分成M个分区）

1. 如果N < M，一般情况下，N个分区有数据分布不均的状况，利用HashPartitioner函数将数据重新分区为M个，这时需要将shuffle参数设置为true；

2. 如果N > M且N和M差不多(比如说N是1000,M是100)，那么就可以将N个分区中的若干个分区合并成一个新的分区，最终合并成M个分区，这时可以将shuffle参数设置为false(在shuffle为false的情况下，设置M > N，coalesce是不起作用的)，不进行shuffle过程，父RDD和子RDD之间是窄依赖关系；

3. 如果N > M且N和M差距悬殊(比如说N是1000,M是1)，这个时候如果把shuffle参数设置为false，由于父子RDD是窄依赖，它们同处在一个Stage中，就可能会造成Spark程序运行的并行度不够，从而影响性能。比如在M为1时，由于只有1个分区，所以只会有一个任务在运行，为了使coalesce之前的操作有更好的并行度，可以将shuffle参数设置为true；


randomSplit:根据weights权重将一个RDD切分成多个RDD；

glom：将RDD中每一个分区中类型为T的元素转换成数组Array[T]，这样每一个分区就只有一个数组元素；

union：将两个RDD集合中的数据进行合并，返回两个RDD的并集（包含两个RDD中相同的元素，不会去重）；

intersection：返回两个RDD集合的交集，且交集中不会包含相同的元素；

subtract：如果subtract针对的是A和B两个集合，即操作是val result = A.subtract(B)，那么result中将会包含A中出现且不在B中出现的元素；

intersection和subtract一般情况下都会有shuffle的过程；

mapPartitions：与map转换操作类似，只不过映射函数的输入参数由RDD中的每一个元素变成了RDD中每一个分区的迭代器；

mapPartitionsWithIndex：与mapPartitions功能类似，只是输入参数多了一个分区的ID；

zip：将两个RDD组合成Key/Value（键/值）形式的RDD，默认两个RDD的partition数量以及元素数量都相同，否则相同系统将会抛出异常。

zipPartitinos：将多个RDD按照partition组合成为新的RDD，zipPartitinos需要相互组合的RDD具有相同的分区数，但是对于每个分区中的元素没有要求。

zipWithIndex：是将RDD中的元素和这个元素的ID组合成键/值对，需要启动一个Spark作业来计算每一个分区的开始索引号，以便能顺序索引。

zipWithUniqueId：是将RDD中的元素和一个唯一ID组合成键/值对，不需要这样一个额外的作业。

### 3.2键值RDD转换操作 ###

### 3.3RDD依赖关系 ###

## 4.控制操作(control operation) ##

## 5.行动操作(action operation) ##

### 5.1集合标量行动操作 ###

### 5.2存储行动操作 ###
