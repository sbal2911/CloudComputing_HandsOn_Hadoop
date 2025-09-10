
# Hadoop MapReduce Word Count Project - HandsOn

This repository showcases the fundamentals of running and processing MapReduce programs using a basic counting functionality on number of words. This project is all about reading a text file, implementing Mapper and Reducer logic and, in turn generating a count of number of times, a particular word appears in the text file. The input text and their corresponding output are included for reference.

## Approach and implementation
1. Mapper Logic: The mapper uses a StringTokenizer to divide the input text into independent tokens. The logic includes a while loop that is leveraged to run through each token and designate it as a key-value pair. To avoid processing any irrelevant data, tokens less than 3 characters are eliminated.

2. Reducer Logic: The reducer combines the key-value pairs created from the Mapper logic. For each repeatitive word, it adds +1 to the counter and generates a complete list of words along with their counts.

## Setup and Execution

Please follow the next set of 9 instructions to properly implement the whole project with desired outputs:

### 1. **Initialize the Hadoop Cluster**

Start the Hadoop cluster using Docker Compose:

```bash
docker compose up -d
```

### 2. **Develop the required Code**

Use Maven to clean and package the code:

```bash
mvn clean package
```

### 3. **Deploy the JAR File to Hadoop**

Copy the packaged JAR file into the Hadoop ResourceManager container:

```bash
docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 4. **Transfer the Dataset to Docker Container**

Move the input dataset into the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Access the ResourceManager Container**

Connect to the container shell:

```bash
docker exec -it resourcemanager /bin/bash
```

Find the way to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Configure HDFS**

Create a directory for the input data in HDFS :

```bash
hadoop fs -mkdir -p /input/data
```

Upload the dataset to HDFS:

```bash
hadoop fs -put ./input.txt /input/data
```

### 7. **Run the MapReduce Logic**

Execute the word count logic: In this case, if you find an error with the folder name stating the folder already exists, feel free to change the name of the destination folder for your smooth usage.

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1
```

### 8. **Observe the Output**

View the result of the MapReduce job stored in HDFS:

```bash
hadoop fs -cat /output1/*
```

### 9. **Export results to Local Systemn**

To save a copy of the output from HDFS to your local machine:

1. Use the command for copying results from HDFS into the container:
   
    ```bash
    hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. Exit the container and transfer results to your host OS:
   
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/
    ```
    
## Challenges Faced and Solutions

**Challenges:**  Everytime I tried to run the MapReduce logic as explained in step 7 above, my processing use to stop even though resource manager was up and running, the job abruptly stopped and exited from the containers.

**Solution:** I tried increasing the Memory limit in the advanced resources section of the Docker setting and the program executed successfully with below output.

## Provided Input: 
 ```bash
Imagine was you removal raising gravity. Unsatiable understood or expression dissimilar so sufficient. 
Its party every heard and event gay. Advice he indeed things adieus in number so uneasy. 
To many four fact in he fail. My hung it quit next do of. It fifteen charmed by private savings it mr. Favourable cultivated alteration entreaties yet met sympathize. 
Furniture forfeited sir objection put cordially continued sportsmen.
   ```

## Generated output: 
 ```bash
adieus	1
continued	1
objection	1
party	1
mr.	1
gay.	1
fail.	1
quit	1
of.	1
indeed	1
four	1
Imagine	1
savings	1
cordially	1
fifteen	1
many	1
Favourable	1
sportsmen.	1
next	1
number	1
and	1
hung	1
expression	1
met	1
sir	1
gravity.	1
removal	1
private	1
sympathize.	1
every	1
event	1
things	1
yet	1
dissimilar	1
cultivated	1
heard	1
was	1
Advice	1
fact	1
Furniture	1
understood	1
entreaties	1
uneasy.	1
raising	1
Its	1
alteration	1
put	1
Unsatiable	1
forfeited	1
charmed	1
you	1
sufficient.	1
   ```
