## Concepts de base

### Introduction

Objectifs 

### Concept 1

Les points à aborder

1. Point 1
2. Point 2

### File to be downloaded

**Hadoop Tar ball:** Access the Hortonworks Sandbox and know the version of Hadoop used in it.  [Download](http://www.apache.org/dyn/closer.cgi/hadoop/common/ "Download a Hadoop Release") the tar.gz file of same version as the one present in the Hortonworks Sandbox.   
**Note**: Extract the downloaded Hadoop tar.gz file and ensure that it contains the following files:

    a. share/hadoop/common/hadoop-common-*.jar
    b. share/hadoop/mapreduce/hadoop-mapreduce-client-core-*.jar
    c. share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-*.jar

### Explanation of the Use Case
A fictional use case is presented here, in order to enable you to easily understand the functionality and power of Hadoop MapReduce, without finding it overwhelming or boring.

####General Elections in Utopia
In the democratic country of Utopia, the General Elections (Polls) for the position of President was conducted some days ago.  About five million Utopian Citizens participated in the elections with full enthusiasm and cast their votes to the candidate whom they think deserves the position.  Because of large population, the elections were conducted in 1000 polling booths, geographically separated, so that a citizen can vote from his nearest booth.

There were five contestants for the post.  The names of the candidates are as follows:  
1. Jaja  
2. Jiji  
3. Jojo  
4. Juju  
5. Jinjin  

You are the Chief Election Commissioner, the person responsible for fair conduct of the entire election process.  Now that the polls are over, half of your worries are over.  But, just a half!  The remaining half is to ensure that the vote-counting happens without any error.  You are responsible for announcing the next president of the country.

As the Chief Election Commissioner, you have been given the power to use any resource of the country judiciously, for the purpose of counting votes.  As you have been planning, from the beginning, to use the computational resources of the country for this purpose, you chose Hadoop MapReduce to do the dirty job of vote counting for you.

Your pre-planning skills have helped you in taking each vote as an electronic vote, where every vote cast in every polling booth is written into the booth's text (.txt) file as one line.  This one line will be the name of the candidate who received the vote.

For example, if the first vote in the polling booth 5 goes to Jaja, the `booth_5.txt` file would look like the one below:

```
Jaja
```
If the second vote goes to Jojo, the file would look like the one below:

```
Jaja
Jojo
```
...and so on!

You have to announce the results by this evening, before 5 PM.  It is already 10 AM.  As it happens to be, you are a good Java Programmer. Now, roll up your sleeves and follow the instructions to count the votes and announce the results by 5 PM.

### Instructions
####Eclipse Project Setup

####Code the Mapper Class
In the eclipse project created above, create a new Java Class file with name `VoteCountMapper` and write the following code in it.  This will be the Mapper Class.

```java
package com.vivekGanesan;

import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

public class VoteCountMapper extends Mapper<Object, Text, Text, IntWritable> {
	
	private final static IntWritable one = new IntWritable(1);
	
	@Override
	public void map(Object key, Text value, Context output) throws IOException,
			InterruptedException {
		
		//If more than one word is present, split using white space.
		String[] words = value.toString().split(" ");
		//Only the first word is the candidate name
		output.write(new Text(words[0]), one);
	}
}

```

####View Vote Count Results
The final, consolidated vote count for each candidate can be found in a file present in the directory `/user/hue/VoteCountOutput`.  View this file using [File Browser](http://localhost:8000/filebrowser/) to know the number of votes each candidate got.

![Job Results](images/tutorial-09/job_results.png "Job Results")

Assuming that you spent approximately an hour, including the setup and running of MapReduce job, you could be ready with the election results by 11 AM.  Then what? Party until 5 PM :)

