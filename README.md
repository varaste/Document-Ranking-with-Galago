# Document-Ranking-with-Galago

In this project, the goal is to get familiar with the evaluation criteria and scoring functions for documents. A scoring function assigns a score to the document according to the degree of relevance of a document to the query, so that the documents are ranked and displayed based on their score. Finally, the resulting ranking is compared with the grand truth and the efficiency of the recovery function is reported.
The text search tool used in this exercise is Galago

## Objectives

- Indexing of all documents

- Applying and familiarizing with available retrieval functions

- Using evaluation criteria and calculating the effectiveness of evaluation functions

## Pre Requirements

At first, it is necessary to provide environment for the project as follows:

- Download and install version 20.4 of Ubuntu operating system

- Java installation

- Maven Installation 

- Galago Installation 

Dataset used in this project consists of three parts:

### Corpus

Collections of news articles are in TREC format. Each document contains several fields:

- **OCNO**: ID of each document

- **Head**: The title of the document

- **Text**: The text of the document

### Queries

This file contains questionnaires


### Relevance Judgment
This file contains relevant judgments. In the final stage to evaluate retrieval functions, the obtained results are compared with these judgments.

## Create an index 

In any language, words will have different appearances according to the role they play in sentences. But all of them are made from the same root. Therefore, in many methods, we must first find the root of the words. There are different algorithms for stemming, Porter's algorithm is one of the famous algorithms in the English language. According to a series of regular rules (for example, removing the letter s at the end of plural words), this algorithm can obtain the roots of words with good accuracy.

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
