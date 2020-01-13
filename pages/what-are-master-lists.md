---
layout: post
title: What are Master Lists?
published: true
---

Something that a master reference database can contain is master lists. A master list is a set of records that enumerate a set of known and desired things. Lets continue using the example of stocks.


We can create a list of stocks that we care about as a set of records containing their tickers:


Stocks Table

| Stock Ticker | Company Name |
|------------|------------|
| AAPL | Apple Inc |
| IBM | International Business Machines Corporation |
| MSFT | Microsoft Corporation |
etc





We can place in this list all the stocks that we know about. We can then call this the master stocks list.

Crucially, we want entries of this list to be unique. Having duplicates could cause issues when trying to do some types of analysis using the list. For example joining the master list of stocks with trading profit and loss items. We could end up double counting profits for the same stock.

This list can then be queried. Eg: 

"Give me all the stocks whose ticker begins with 'A' "

In the scope of a master reference database, we want the "stocks" list to contain all a master list of stocks and contains the definitions of the stocks for a given scope (usually the enterprise).


Master lists are extremely important as they often form the basis for many queries. One might query, for all the stocks that don't have prices for yesterday. If one didn't have a master list, one would most likely have to create a list of stocks of interest from the set of the prices from the day before. This is time consuming and prone to errors. Some stocks might have been suspended and have no price for the day before. Building master lists is a key component of building a reference database.


To do this we need to ascertain the scope (what stocks do we need / want), and the sources (where can we get lists of stocks). Then, we'll hav to define what is the minimum information required of a stock to place it in the master list. 

## What stocks do we want in the master list?

This may seem like a simple question but its very important. Its likely that the intended scope of the master list is something that business users will define. For example "All stocks traded in the US with a market capitalisation of more than 10m US dollars". 

## Where are we going to get these values from?

In the above example, obtaining the list of stocks of interest will probably involve querying some external resource, for example a financial data provider like Bloomberg or Thomson Reuters. Often, we will need combine multiple sources of lists into one. 

Exactly how you query these other data sources is beyond the scope of this article, but you will probably end up querying, yes, another master list. This master list is updated more frequently and accurately than yours, you hope.

## Ok, we got some stocks, lets store them!

Wait a moment. You now have to make sure that you have enough information to be able to store them in your master list. You might therefore reject those entries that don't have sufficient information. Exactly what information that is depends on your requirements, but its instructive to consider what the minimum could be.

## Minimum data required to store a stock

Well this specific question is a sore topic. Its not actually that useful to consider that 

This then throws up the next problem of ensuring that we don't end up with duplicates. This is a significant challenge in many areas. We'll need to have a think about natural keys to do this.
