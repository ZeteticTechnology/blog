---
layout: post
title: What is a DataSet?
published: true
---

I have mentioned the word dataset a few times. Lets define that more specifically.

We'll give an example. Lets say we have some stock ticker to company names records


| Stock Ticker | Company Name |
|------------|------------:|
| AAPL | Apple Inc |
| IBM | International Business Machines Corporation |
| MSFT | Microsoft Corporation |
etc



We might also have a table of stocks to addresses:

| Stock Ticker | Company Address |
|------------|------------:|
| AAPL | Cupertino |
| IBM | 1 New Orchard Rd; Armonk, New York 10504 |
| MSFT | Readmond, Washington |
etc




We can group these two tables and call them the **stocks dataset**. 

In practise, datasets would most likely contain many tables. In a database, we can have many datasets, but quite often we'll have only one. We'll rarely span one dataset across many physical databases, but its possible. 

In fact, if you have data in an enterprise, you could say that your entire enterprise inventory forms a single dataset. But that might be stretching things. 

I've kept the terms separate because a database is a physical implementation, and a dataset is a conceptual grouping. Indeed it is possible to have a dataset be physically implemented as a set of CSV files in a folder on disk.
