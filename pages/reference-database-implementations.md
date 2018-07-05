---
layout: post
title: How should a Master Reference Database be implemented?
published: true
---

Lets look at some implementation aspects when creating a master reference database.

Firstly, we'll need a data store.

### Files

Its perfectly feasible to implement a master reference database as a single file or a series of files in folders. I think the question you should ask yourself is do you really want to do that? Yes, many frameworks exist out there to persist and read data to and from disk into the programming language of your choice, but at some point, you'll probably want to do more that. Perhaps you find a bad value somewhere in that file, and you want to fix it. You'll probably need to write code to do that. The question is, what kind of code do you want that to be? Maybe at some point, the dataset becomes large, or complex in structure. You might find that difficult to organise. What about backups? Replication, multi-user access, compression? Could get messy!

### Databases

Databases have been around for decades, and many have very advanced and robust implementations. They implement all the features above, at the added cost of a standalone process that is typically hosted on a dedicated machine. Most databases also support a type of application programming interface (API) called SQL that is very well adapted to working with data.

My recommendation is to use a database. PostgreSQL is a free open source database system that scales well and competes very favouably with the likes of Microsoft SQL Server and Oracle etc.


## Types of database

### Relational SQL database

There are many definitions out there that describe relational databases in detail. I wont copy them out here. However I will state why I recommend using a relational database as the physical mechanism for creating a master reference database.  

Master reference data is all about linking and providing context. This is well suited to the relational approach for storing records. It is straight forward to create relational structures that can encapsulate relationships between tables, records etc. 

The implementations of relational databases are well understood, reliable, and in some cases free to use. Tooling is good. Integration with other IT systems and programming languages is extremely good. In short, they are a mature technology and they work well.

TODO: Note about ACID compliance
 
### NoSQL Databases

In the last 15 years or so there have been great strides in the areas of high performance systems for data storage and querying, NoSQL, Hadoop, Couch DB etc, but typically for master reference databases the trade offs for the increased performance of these implementations are usually not worth it. Few high performance retain ACID compliance and that is a key feature of making sure that master reference data is complete and accurate.

### Columnar store database 

However, for time series data the picture is different. There are now available time series dataset for ingestion that contain billion and trillions of records. These records usually do not have a complex structure and moreover with the correct schema usually never need to be updated or deleted. Using a traditional relational store implementation will have trouble scaling to such large volumes. At this point, columnar store databases can be especially suited.


## Data formats

There are many ways of representing data. 

- Key value pairs
- Tabular
- Row format and column format
- Matrix
- Object representations
- Graphs


In my personal experience, that unless you have a requirement to store graphs or matrix data, tabular formats are the easiest to work with storing and delivering master reference data. This is because they are simple to understand, store and consume. When implementing a master reference data system, you'll have enough issues with content to really want to do anything fancy with storage formats. 


## Data processing components

You'll probably want to build some components that load data in bulk format from sources, or maybe a GUI to allow users to interact with your datastore. To do that you could buy something off the shelf, or write your own. 

To build code that easily interacts with a database, you'll have a lot of choice. If you are an expert on a particular programming language, then probably use that. You'll be productive quickly. However, if you want to try something new, or really have a free hand in deciding what to use, then some programming languages do stand out for their flexibility and library support.

My personal recommendation is to use Python. 

Python has database connectivity with many databases via pyodbc. 

Python has pandas, a dataframe library that makes many data operations trivially easy. For example, filtering, joins, interpolation, inputation, etc. Pandas is heavily used in the data science and AI fields. 

Python has an easy to use REPL environment, which is something java only recently added. This makes prototyping data processing steps much easier that the usual write, compile, break point routine that many java programmers have had to live with for so long.

Pretty much everything in the Python eco-system is free and open source.

Python does have a less than optimal threading model though. for some large scale batch operations, and data services, this can be a limitation. However, there now exists open source frameworks that have interfaces to Python that handle large scale batch processing (eg Apache Spark), or high speed data ingestion (Apache kafka). 

My advice is don't try to architect and implement distributed computing frameworks yourself if there exists an open source framework that does it for you. These kinds of solution are hard to get right!


If you implement many batch loaders of data, you'll probably find that many of them have similar requirements for data processing, but also infrastructure considerations like, how to connect to the database, how to bulk load etc. Its obviously useful to develop patterns for these infrastructure operations so that you don't re-invent the wheel. Of course you can have many stand-alone loaders running the same code components that you reuse in your code base, but consider also that it can be worth your time to use a framework for such things.
