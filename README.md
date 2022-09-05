# Document-Ranking-with-Galago

In this project, the goal is to get familiar with document evaluation criteria and scoring functions. A scoring function assigns a score to the document according to the degree of relevance of a document to the query so that the documents are ranked and displayed based on their score. Finally, the resulting ranking is compared with the grand truth, and the efficiency of the recovery function is reported. The text search tool used in this exercise is Galago.

## Objectives

- Indexing of all documents
- Applying and familiarizing with available retrieval functions
- Using evaluation criteria and calculating the effectiveness of evaluation functions

## Prerequisite

At first, it is necessary to provide an environment for the project as follows:
 
- Download and install version 20.4 of the Ubuntu operating system
- Java installation
- Maven Installation
- Galago Installation

First, Connect to the internet.

second install prerequisite,

1- Java

How to install java on ubuntu:

	a.sudo apt-get update && apt-get upgrade
	b.sudo apt-get install default-jdk
	c.java -version : 
		if (the output is like below, it's in the correct path.): then go to: 2- Maven
			output:
			openjdk version "11.0.4" 2019-07-16
			OpenJDK Runtime Environment (build 11.0.4+11-post-Ubuntu-1ubuntu218.04.3)
			OpenJDK 64-Bit Server VM (build 11.0.4+11-post-Ubuntu-1ubuntu218.04.3, mixed mode, sharing)
			
		else:
			d.sudo update-alternatives --config java : this give you the jdk path 

				output :
					/lib/jvm/java-11-openjdk-amd64/bin/java

			e.sudo gedit /etc/environment

			f.if (not exist JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64" at the end of the file)
						g.add this line to end of file : 
						h2.source /etc/environment
						e3.echo $JAVA_HOME 

							output in this step:
								 "/usr/lib/jvm/java-11-openjdk-amd64"

2- Maven

How to install Maven on Ubuntu:


	Notice:the path below is where you have put the downloaded files, like: /home/USER_NAME/Desktop/Galago/apache-maven-3.6.2-bin.tar.gz 
	a.cp /home/USER_NAME/Desktop/Galago/apache-maven-3.6.2-bin.tar.gz /tmp
	b.sudo tar xf /tmp/apache-maven-*.tar.gz -C /opt
	c.mv /opt/apache-maven-3.6.0 /opt/maven
	d.sudo gedit /etc/profile.d/maven.sh
	e.paste the following configuration in file:
		export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
		export M2_HOME=/opt/maven
		export MAVEN_HOME=/opt/maven
		export PATH=${M2_HOME}/bin:${PATH}

	f.sudo chmod +x /etc/profile.d/maven.sh
	g.source /etc/profile.d/maven.sh
	h.mvn -v
	i.output in this line:

		Apache Maven 3.6.2 (40f52333136460af0dc0d7232c0dc0bcf0d9e117; 2019-08-27T19:36:16+04:30)
		Maven home: /opt/maven
		Java version: 11.0.4, vendor: Ubuntu, runtime: /usr/lib/jvm/java-11-openjdk-amd64
		Default locale: en_US, platform encoding: UTF-8
		OS name: "linux", version: "4.15.0-65-generic", arch: "amd64", family: "unix"		

3- Galago

	a.go to galago source folder : cd galago-3.16/
	b.enter this command: mvn -DskipTests=true install (notice: it takes 30 minutes)
		if (the installation brought up the error about dependency-check-maven 3.3.4):
			e1.open pom.xml: gedit pom.xml 
			e2.find the plugin making the error: (Ctrl+F) 3.3.4
			e3.comment the plugin: <!--   --> 
			e4.go to b.
		else:
			
			successful output would be something like this:
				[INFO] ------------------------------------------------------------------------
				[INFO] Reactor Summary for galago 3.16:
				[INFO] 
				[INFO] galago ............................................. SUCCESS [ 42.601 s]
				[INFO] utility ............................................ SUCCESS [ 11.403 s]
				[INFO] tupleflow-typebuilder .............................. SUCCESS [  7.729 s]
				[INFO] tupleflow .......................................... SUCCESS [  4.891 s]
				[INFO] eval ............................................... SUCCESS [  5.547 s]
				[INFO] snowball-stemmers .................................. SUCCESS [  3.615 s]
				[INFO] krovetz-stemmer .................................... SUCCESS [  3.285 s]
				[INFO] core ............................................... SUCCESS [ 43.162 s]
				[INFO] contrib ............................................ SUCCESS [ 25.643 s]
				[INFO] ------------------------------------------------------------------------
				[INFO] BUILD SUCCESS
				[INFO] ------------------------------------------------------------------------
				[INFO] Total time:  02:28 min
				[INFO] Finished at: 2019-10-05T16:54:28+03:30
				[INFO] ------------------------------------------------------------------------
	
	c.enter this command: chmod +x core/target/appassembler/bin/galago
	
	d.enter this command: core/target/appassembler/bin/galago
	
	   output of this step:
	   
		Type 'galago help <command>' to get more help about any command.

		    Popular commands:
		       build
		       search
		       batch-search

		    All commands:
		       batch-search
		       build
		       build-entity-corpus
		       build-special
		       build-topdocs
		       build-window
		       build-word-dates
		       chain-jobs
		       doc
		       doc-id
		       doc-name
		       doccount
		       dump-connection
		       dump-corpus
		       dump-index
		       dump-index-manifest
		       dump-key-value
		       dump-keys
		       dump-modifier
		       dump-name-length
		       dump-term-stats
		       eval
		       harvest-links
		       help
		       make-corpus
		       merge-index
		       overwrite-manifest
		       pagerank
		       search
		       stats
		       stemmer-conflation
		       subcollection
		       transform
		       xcount


	e. (optional) you can set an alias for 'galago' in your system to point out to '/core/target/appassembler/bin/galago'


Dataset used in this project consists of three parts:

### Corpus

Collections of news articles are in TREC format. Each document contains several fields:

- **OCNO**: ID of each document

- **Head**: The title of the document

- **Text**: The text of the document

### Queries

This file contains questionnaires.


### Relevance Judgment
This file contains relevant judgments. In the final stage, to evaluate retrieval functions, the obtained results are compared with these judgments.

## Create an index 

In any language, words will have different appearances according to the role they play in sentences. But all of them are made from the same root. Therefore, in many methods, we must first find the root of the words. There are different algorithms for stemming; Porter's algorithm is one of the famous algorithms in the English language. According to a series of regular rules (for example, removing the letter s at the end of plural words), this algorithm can obtain the roots of words with good accuracy.

After installing and setting up Galago, we used the Porter Stemmer method to find the roots of words with the help of Galago commands.

```ruby
"stemmer" : ["porter"]
```
  
Tokenization converts text into its constituent tokens. We do this with the following commands:

```ruby
"tokenizer" : {
    "fields" : ["text","head"],
    "formats" : {
      "text"    : "string",
      "head"    : "string"
    }
  } 
```

![Tokenization](https://github.com/varaste/Document-Ranking-with-Galago/blob/main/assets/Arya%20Varaste-Tokenization.png)



Finally, we extracted the file format, which was initially unformatted with 7zip software from the Windows operating system and reached the .txt file. This type of file is suitable for documents and texts that we need to be able to see different parts of it separately.

![trectext](https://github.com/varaste/Document-Ranking-with-Galago/blob/main/assets/Arya%20Varaste-%20trectext%20.png)

We put the commands related to the settings mentioned above in the indexSettings.json file and run the following command in the terminal:

```
Galago/galago-3.16/core/target/appassembler/bin/galago build  /home/arya/Desktop/CA1-Resources/indexSettings.json
```

After about an hour of executing the Build command, the indexing process was completed successfully.

```
Done Indexing.
  - 0.92 Hours
  - 55.18 Minutes
  - 3310.79 Seconds
Documents Indexed: 163912.
```

With the BM25 method, we first perform the retrieval with the default values of b and k on the query set 101 to 150 with the value of 100 for the number of Requested and obtain the values of Recall, MAP, nDCG and P@5. In the following, we change the values of b and k by trial and error first with long steps and note the values every time, and if we see an improvement compared to the default state, we try close values with smaller steps to reach the optimal value.

By searching in scientific sources, the suitable range for the b parameter was 0.3 to 1, and the suitable range for the k parameter was 0.5 to 2.5; based on this, we tested the values in these areas as well as a margin from above and below these areas, and the parameters Optimum for questionnaires 101 to 150 for parameters b and k were seen as 0.4 and 2.6 respectively.

The results obtained from these values were greater than the results obtained from the default values, which indicates that the optimization of these parameters was successful in obtaining better results.
