# What is spark-kompactor
This is a tool written in pyspark to compact small files underline for Hive tables on HDFS. This helps to reduce the workload burden on Name Node of a Hadoop cluster. More details of why many small files on Hadoop causing problem can be found from [here](https://vanducng.dev/2020/12/05/Compact-multiple-small-files-on-HDFS/).

# How it works
The tool works for only table having partitions and can run on scheduled for maintanance purpose. This will help to prevent conflict writting on latest partition as most of our jobs are running in incremental manner. Below steps showing its compacting operation:

1. Scan partitions on provided table
1. Count number of files and total size for each partition
1. Checkout data of each partition, repartition it (compacting) based on default block size.
1. Overwrite partition with repartitioned data 

> To prevent the conflict writing, within spark configuration, the `spark.hadoop.hive.exec.stagingdir` is configured to write to other directory instead of default one.

# How to run it
As this is a Spark application, simply run below spark-submit command with your input table name. Bear in mind that, your account should be accessible and have DML privilege to that table.

```bash
spark-submit --master yarn \
--deploy-mode client --driver-memory 2g \
--executor-memory 2g --executor-cores 2 \
--files logging.ini kompactor.py \
--table_name schema_name.table_name
```

## References
* [On Spark, Hive, and Small Files: An In-Depth Look at Spark Partitioning Strategies](https://medium.com/airbnb-engineering/on-spark-hive-and-small-files-an-in-depth-look-at-spark-partitioning-strategies-a9a364f908)
* [Building Partitions For Processing Data Files in Apache Spark](https://medium.com/swlh/building-partitions-for-processing-data-files-in-apache-spark-2ca40209c9b7)
* [Compaction / Merge of parquet files](https://kontext.tech/column/spark/296/data-partitioning-in-spark-pyspark-in-depth-walkthrough)
* [Why does the repartition() method increase file size on disk?](https://stackoverflow.com/questions/54218006/why-does-the-repartition-method-increase-file-size-on-disk)
