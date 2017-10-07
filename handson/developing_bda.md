# Developing Big Data applications

This hands-on session will require a Scala IDE (I personally prefer IntelliJ Idea).

# Bootstrapping an sbt project

Clone SparkTemplate repository:
```
git clone https://github.com/earthquakesan/SparkTemplate
```

## Tasks

1. Install [sbt](http://www.scala-sbt.org/release/docs/Installing-sbt-on-Linux.html)
2. Bootstrap new project
3. Write a simple test case
4. Write wordcount application
5. Pack wordcount in spark submit
6. Upload files to HDFS and run wordcount for them

# Importing example project
Import [Twitter Trends](https://github.com/earthquakesan/TwitterTrends) sbt project into your preferable IDE.
IntelliJ Idea can import git repositories as projects.
You can also just clone it to your workspace and run from command line:
```
git clone https://github.com/earthquakesan/TwitterTrends
```

## Tasks

1. Get Twitter API key
2. Run application with your keys (using docker-compose)
3. Import application into IDE and run components separately

# Assignments

Write an application, which reads access and error apache logs (use sample files) and output the following information:

1. Number of requests per day
2. Number of requests per IP address over all the log duration
3. Amount of failed requests

Write an application to make a text analysis (use sample files --> Alice in Wonderland). It should output the following information:
1. Most used words in the book
  a. Make a stop list from the most used words such as "the", "a", "or" etc.
  b. Output only words not in the stop list.
2. Build a letter statistical distribution (i.e. letter count)
3. Visualize distribution as a histogram using com.quantifind.charts.Highcharts._ (see TwitterTrends application).

All the applications should be covered with local unit tests, configurable via environmental variable and be possible to deploy using Docker.
