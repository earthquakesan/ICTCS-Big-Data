# Spark operations

Parallelization --> Transformations --> Actions

In general, Spark operations can be divided into three categories: parallelization (loading data into RDDs), transformations (map, reduce, filter etc.) and actions (collecting and saving results).

This document contains only part of available operations. For the full list refer to the [Spark Programming Guide](http://spark.apache.org/docs/2.1.1/programming-guide.html).

Recommended reading: [Mohammed Guller "Big Data Analytics with Spark"](https://www.amazon.com/Big-Data-Analytics-Spark-Practitioners/dp/1484209656)

## Loading Data

`parallelize`:
```
val data = Array(1, 2, 3, 4, 5)
val distData = sc.parallelize(data)
```

`textFile`:
```
scala> val distFile = sc.textFile("data.txt")
distFile: org.apache.spark.rdd.RDD[String] = data.txt MapPartitionsRDD[10] at textFile at <console>:26
```

`wholeTextFiles`:
```
val solrDocs = sc.wholeTextFiles("/opt/solr/docs/*/*")
```

## Transformations

`flatMap`, `map`, `reduceByKey`:
```
package core

import org.apache.spark.{SparkConf, SparkContext}

object WordCount {
  def main(args: Array[String]): Unit = {
    val config = new SparkConf().setMaster("local[4]").setAppName("My Application")
    val sc = new SparkContext(config)
    val input = args(0)
    val output = args(1)

    val lines = sc.textFile(input)
    val wordCount = lines
      // flatMap produces an Array, and not an Array of Arrays
      .flatMap(line => line.split(" "))
      // represent each element as a tuple
      .map(word => (word, 1))
      // count all elements
      .reduceByKey(_ + _)
    wordCount.saveAsTextFile(output)
  }
}
```

`map`, `filter`, `reduce`:
```
val solrDocs = sc.wholeTextFiles("/home/ivan/.virtualenvs/eccenca/vendor/solr/solr-5.3.0/docs/*/*")
val countLessThan100 = solrDocs
  .map({case (name, contents) => contents.length})
  .filter(l => l < 100)
  .map(_ => 1)
  .reduce(_ + _)
println(countLessThan100)
```

`cartesian`:
```
val numbers = sc.parallelize(List(1, 2, 3, 4))
val alphabets = sc.parallelize(List("a", "b", "c", "d"))
val cartesianProduct = numbers.cartesian(alphabets)
cartesianProduct.foreach(println)
(1,a)
(1,b)
(1,c)
(1,d)
(2,a)
...
(4,d)
```

`zip`:
```
val numbers = sc.parallelize(List(1, 2, 3, 4))
val alphabets = sc.parallelize(List("a", "b", "c", "d"))
val zippedPairs = numbers.zip(alphabets)
zippedPairs.foreach(println)
(1,a)
(2,b)
(3,c)
(4,d)
```

`zipWithIndex`:
```
val numbers = sc.parallelize(List(1, 2, 3, 4))
val zipWithIndex = numbers.zipWithIndex()
zipWithIndex.foreach(println)
(1,0)
(2,1)
(3,2)
(4,3)
```

`sortBy`:
```
val customerCSVString =
  """
    |John Doe,25,m,04159
    |John Doe2,35,m,04169
    |Jane Doe,55,f,04107
    |John Doe4,15,m,04159
    |Jane Doe5,10,f,04107
    |John Doe6,56,m,04169
    |Jane Doe7,78,f,04159
    |John Doe8,55,m,04107
  """.stripMargin

case class Customer(name: String, age: Int, gender: String, zip: String)
val customersCSV = sc.parallelize(customerCSVString.split("\n")).filter( s => s.split(",").length > 1)
val customers = customersCSV map {
  l => {
    val a = l.split(",")
    Customer(a(0), a(1).toInt, a(2), a(3))
  }
}

val groupByZip = customers.sortBy( c => c.age, true)
println(groupByZip.collect().mkString("\n"))

Customer(Jane Doe5,10,f,04107)
Customer(John Doe4,15,m,04159)
Customer(John Doe,25,m,04159)
Customer(John Doe2,35,m,04169)
Customer(Jane Doe,55,f,04107)
Customer(John Doe8,55,m,04107)
Customer(John Doe6,56,m,04169)
Customer(Jane Doe7,78,f,04159)
```

## Actions

`collect`:
```
val rdd = sc.parallelize((1 to 10000).toList)
val filteredRdd = rdd filter { x => (x % 1000) == 0 }
val filterResult = filteredRdd.collect
```

`count`:
```
val rdd = sc.parallelize((1 to 10000).toList)
val total = rdd.count
```

`first`:
```
val rdd = sc.parallelize(List(10, 5, 3, 1))
val firstElement = rdd.first
```
