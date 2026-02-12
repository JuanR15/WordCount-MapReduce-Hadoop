# Hadoop MapReduce WordCount Implementation

This project demonstrates a complete implementation of the WordCount program using Java, Apache Maven, Hadoop 3.2.1, and Docker. The objective was to build a MapReduce application, package it into a JAR file, deploy it to a Hadoop cluster running inside Docker containers, and execute the job using HDFS for distributed data processing.

The Hadoop cluster was deployed using Docker containers, including a NameNode, multiple DataNodes, ResourceManager, NodeManagers, and a HistoryServer. Java (OpenJDK 25) and Apache Maven 3.9.12 were installed locally to build the project. Maven was configured using environment variables, and the project was successfully compiled using:

mvn clean package

This generated the executable JAR file:

WordCountUsingHadoop-0.0.1-SNAPSHOT.jar

The JAR file was then copied into the ResourceManager container using:

docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/

An input directory was created in HDFS:

hdfs dfs -mkdir -p /input

The input file was uploaded into HDFS:

hdfs dfs -put input.txt /input/

The MapReduce job was executed using:

hadoop jar WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input /output

After execution, the output was retrieved using:

hdfs dfs -cat /output/part-r-00000

The program successfully processed the input file and produced the following word counts:

dataset 1  
your 1  
own 1  
input 1  
Create 1  

This confirms that the Mapper correctly tokenized the input text and emitted key-value pairs, and the Reducer correctly aggregated and summed the word frequencies.

During development, several technical challenges were encountered. Maven was initially not recognized due to incorrect PATH configuration. Java was not properly installed in the system environment variables. A ClassNotFoundException occurred due to using the incorrect fully-qualified class name when executing the Hadoop job. Additionally, navigating Docker containers and interacting with HDFS required troubleshooting and validation of file locations. These issues were resolved by properly configuring environment variables, verifying the contents of the compiled JAR file, and using the correct class path when running the job.

