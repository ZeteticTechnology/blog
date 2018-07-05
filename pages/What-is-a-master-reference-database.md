---
layout: post
title: What is a Master Reference Database
published: true
---

## What is a master reference database and why build one?

In short, it is a database containing a dataset of master reference data.


## What is master reference data?

Master reference data is data that many users of related datasets would use to make those datasets more usable. Additionally, data contained in a master reference database can be used as a set of joining pieces of data that allow data to be compared across multiple datasets. 



## But firstly, what is data? What is information?

For the purposes of these articles, data is a series of related values. Typically, these values are organised into sets of records. A record is a set of related values, that have some sort of structure.



For example, a simple stock trade notification record:


> 2018-06-05T18:47:05+00:00 AAPL 654 54


To make sense of the above values, we need to know what the fields represent, and the ordering. In practice, we would need some sort of key, or manual of how to read the values. Otherwise, we can't say for certainty what values are the trade size and price.

So, to make unambigeous sense of this information, we need a key. 

| Position | Meaning |
|----------|-------|
| 1 | Trade DateTime |
| 2 | Stock Ticker |
| 3 | Price |
| 4 | Trade size |

We can now combine the key with the data record to get:


| Field Name | Field Value |
|------------|------------:|
| DateTime | 2018-06-05T18:47:05+00:00 |
| StockTicker | AAPL |
| Price | 654 |
| Trade Size | 54 |

In this example, we can describe the record as having fields, each with a Field Name, and a Value. One can think of a record therefore as a list of key / value pairs.


Note that the ordering of the key / value pairs in the above example doesn't make any difference. This is because the field names are unique. This is good practice. An example of bad practise would be:

| Field Name | Field Value |
|------------|------------:|
| DateTime | 2018-06-05T18:47:05+00:00 |
| StockTicker | AAPL |
| Value 2 | 654 |
| Value 1 | 54 |




This is a good example of the difference between data and information. Without the key, the stock trade record is just data, just a bunch of values. Its not very useful on its own. When we add the key, it becomes more useful, and we can extract some information from the data. We might be able to use it to calculate the profit or loss from a trade of that stock that we did earlier. 


An interesting thing to note here is that to extract information from data, one typically needs to combine it with more data. This is primarily to achieve two effects:


1. Makes the data itself more specific or precise. In the above example, adding the key. Now we can be more precise about what the values are.

or

2. Places the data in a wider context (makes the context more specific or precise). For example, we may want to know what the name of the company is, if we don't recognise the ticker.


It is typically the job of a master reference database to supply these extra values. A master reference database might supply a record that states:


| Field Name | Field Value |
|------------|------------:|
| Stock Ticker | lshd |
| Company Name | sdhskdh |


Now we can combine the two records by joining on the stock ticker to form:


| Field Name | Field Value |
|------------|------------:|
| DateTime | 2018-06-05T18:47:05+00:00 |
| StockTicker | AAPL |
| Value 2 | 654 |
| Value 1 | 54 |
| Company Name | Apple Inc |


This combined record is now more usable.

There are two interesting parallels worth mentioning at this point.

This idea that there is data and context, can be found in two other conceptually similar things in this world:

## Lets talk about jargon

Jargon:

Dict defnition

/ˈdʒɑːɡ(ə)n/

noun

    special words or expressions used by a profession or group that are difficult for others to understand.
    "legal jargon"

People use jargon all the time. Some arenas of human communication use it more intensely than others, but typically those areas that do, usually are trying to communicate complicated concepts under time pressure. Examples include: Emergency dispatchers and first responders, trading floors, emergency rooms.

Jargon is used for many reasons, but critically, it can be used to speed up communication, by omitting pre-agreed context. 


### Data compression

In computer systems, there are constraints on the availability of data storage (disks, ram etc), and the available time to process data (cpu cycles). As such it doesn't usually make sense to store the name of every field for every value on every record. What we might typically see is a tabular format table of multiple records. For example:


| DateTime | StockTicker | Price | Amount |
|----------|-------------|-------|--------|
| 2018-06-05T18:47:05+00:00 | AAPL | 654 | 1000 |
| 2018-06-05T18:47:05+00:00 | AAPL | 654 | 4500 |
| 2018-06-05T18:47:05+00:00 | AAPL | 654 | 1500 |
| 2018-06-05T18:47:05+00:00 | AAPL | 654 | 3000 |
| 2018-06-05T18:47:05+00:00 | AAPL | 654 | 200 |
| 2018-06-05T18:47:05+00:00 | AAPL | 654 | 100 |

Now each line represents a record, and the structure implies that the field names hold the same ordinal position for each line. The first line acts as a key for the field names. This is a table format.

This is much more space efficient, and is intuitive for humans to read. Of course, many more compression schemes exist in the field of computer science (many of which are not human readable). But less examine the information aspects on the above table. If we lift any one record out from the complete list, and do not retain the key, the record loses information. Its not clear what each value means.

In general we can see that to process, store and use data, we need to interpret and/or apply some structure, and in most cases, add extra data in order to extract contained information.


A master reference database is a special case where we can gather and extract a lot of this extra contextual data and then use it in many places, typically to make other datasets more useful.

## A Master

We refer to it as a Master reference database is it is common practice to work to make one reference dataset authoritative in the scope of ones endeavours. For example in one enterprise, one probably only wants one final source of truth for reference data values. Therefore we call it the **master**. 


Note that the master can contain structures and records that allow other datasets to disagree with each other, but they should not disagree with the master.
