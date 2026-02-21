# Hadoop MapReduce WordCount Implementation

This project demonstrates a complete implementation of the WordCount program using Java, Apache Maven, Hadoop 3.2.1, and Docker. The objective was to build a MapReduce application, package it into a JAR file, deploy it to a Hadoop cluster running inside Docker containers, and execute the job using HDFS for distributed data processing.

---

## Technologies Used

- Java (OpenJDK 25)
- Apache Maven 3.9.12
- Hadoop 3.2.1
- Docker & Docker Compose
- HDFS (Hadoop Distributed File System)

---

## Project Structure

```
WordCount-MapReduce-Hadoop/
│
├── output/                         # MapReduce output (exported from HDFS)
│   ├── part-r-00000
│   └── _SUCCESS
│
├── shared-folder/input/data/       # Input file
├── src/main/java/com/example/      # Mapper, Reducer, Controller
├── docker-compose.yml
├── pom.xml
└── README.md
```

---

# How to Run the Project

## 1. Start the Hadoop Cluster

From the project root (where `docker-compose.yml` is located):

```bash
docker compose up -d
docker ps
```

This starts the NameNode, DataNodes, ResourceManager, NodeManagers, and HistoryServer.

---

## 2. Build the Project (Maven)

From the project root:

```bash
mvn clean package
```

This generates the JAR file:

```
target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar
```

---

## 3. Copy the JAR into the Hadoop Container

```bash
docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/
```

---

## 4. Copy the Input File into the Container

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/
```

---

## 5. Execute the MapReduce Job

Open a shell inside the ResourceManager container:

```bash
docker exec -it resourcemanager bash
cd /opt/hadoop-3.2.1/share/hadoop/
```

Create input directory in HDFS:

```bash
hdfs dfs -mkdir -p /input
hdfs dfs -put -f input.txt /input/
```

Remove old output (if rerunning):

```bash
hdfs dfs -rm -r -f /output
```

Run the MapReduce job:

```bash
hadoop jar WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input /output
```

---

## 6. View Output in HDFS

```bash
hdfs dfs -cat /output/part-r-00000
```

The `/output` directory is created automatically by Hadoop after the job completes successfully.

---

# MapReduce Output

After execution, Hadoop generates:

```
/output/part-r-00000
```

The resulting word count output is:

```
dataset 1
your 1
own 1
input 1
Create 1
```

For grading purposes, the output directory has been exported from HDFS and included in this repository under:

```
output/part-r-00000
```

This folder serves as proof of successful execution.

---

# Notes

- The `_SUCCESS` file is automatically created by Hadoop to indicate successful job completion.
- If the job is rerun, the `/output` directory must be removed first using:
  
  ```bash
  hdfs dfs -rm -r -f /output
  ```

- The project uses Docker to simulate a distributed Hadoop environment locally.

---

# Conclusion

This assignment demonstrates practical experience with:

- Building a Java MapReduce application
- Managing dependencies with Maven
- Running Hadoop in Docker containers
- Using HDFS for distributed storage
- Executing and validating MapReduce jobs
- Exporting distributed output for repository verification

The application successfully processes input data and produces the correct word frequency counts.
