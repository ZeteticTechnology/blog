## Financial data - how hard can it be?

I've worked with financial data for years, and a recurring theme is that it is harder than it initially appears. Once you start to scratch the surface, it quickly becomes apparent that financial data is far, far from simple. 

So I thought it might be useful to dig into this and explain why. Let's start with an example.

## A stock price history


| Date | Price |
|------------|------------:|
| 2018-06-05 | 256 |
| 2018-06-06 | 258 |
| 2018-06-07 | 270 |

etc

What could be complicated about that?

Well, lets examine what these values actually mean.

Firstly, we need to understand what a stock actually is. Then we need to understand what a price is, and how a price for a stock might actually originate, and then finally we have to understand the date part.

What is a stock?


Essentially, a  stock is some sort of fractional ownership of a company. There are plenty of more details here, but for the purposes of this dicscussion, the stock we care about in the context of the series of prices, is a specific security. 

Stock securities are issued by a specific financial entity. So, if you own a share / stock, then you own a small part of that company, in a limited form. 

However, companies can issue multiple stock lines. They can have different rules. Not only that, but the same company can at a time of their choosing, re-issue a stock line, again with different rules. Let's look at an example - the stock split.

We'll a made up company called Widgets Inc. They create a stock line and issue 1 million individual shares. These are worth 100 USD each. Things go well, and the company makes good money. The market believes that each share is now worth 10000 USD. 

The price of 10000 USD means that the smallest piece of the company anyone can buy is 10000 USD, and that is kind of a large number, given that many investors could be individuals. So, to resove this issue, the company issues a new stock line, with a target price of 1000 USD. To this, they retire the existing stock line (essentially by buying all the issueed stocks back) and issue the new line (they pay for the old shares with the new shares). This is called a stock split. 

In this case, the stock is "split" 10 to 1. That is to say, that 10 new shares in new stock line are issued for each 1 share in the old stock line.

This does not change the total value of all the shares, only the total number of shares.

This does have an effect on the "share price of the company", ie the actual number that is used by the market, in this case from 10000 USD tyo 1000 USD. When you call a stock broker to buy some shares in Widget Inc, you ask him or her for the price. They state "1000 USD" when previously they would say "10000 USD".

If you were therefore to obtain a history of stock prices for this company, you would see a discontinuity in prices around the time of the stock split. 

| Date | Price |
|------------|------------:|
| 2018-06-05 | 10001 |
| 2018-06-05 | 10004 |
| 2018-06-06 | 1002 |
| 2018-06-07 | 1003 |

If you were to run an algorithm over this series and it didn't know that there was a stock split, it might assume that the value of the company had dropped by a factor of ~10. This is not true.

To counteract this, we might _adjust_ the prices older than the point of the stock split, by a factor of 10.

| Date | Raw Price | Adjustment factor | Ajusted Price |
|------------|------------|------------|------------:|
| 2018-06-05 | 10001 | 0.1 | 1000.1 |
| 2018-06-05 | 10004 | 0.1 | 1000.4 |
| 2018-06-06 | 1002 | 1 | 1002 |
| 2018-06-07 | 1003 | 1 | 1003 |

Hopefully it is obvious what the above table is showing, but just to be clear:

We have created the concept of a "Raw Price". This is defined as the price that was in the market _at the time_.

We have created the concept of an "Adjustment Factor". That we use to align Raw prices to be comparable to the ones we see today.

We have created the concept of a "Adjusted Price". This is the price series that we could use in an algorithm that doesn't know about such things like stock splits. We define the Adjusted price to be equal to the Raw Price multiplied by the Adjustment Factor.

We have also populated the adjustment factor column with values that represent the stock split. We have added values of 1 to the period after the stock split, and 0.1 before.

Now we can see the Adjusted Price is much more in line with the current price seen in the market.

We can also see that the adjustement process works going backwards in time. We start with prices that are comparable to what we see in the market today, and then go backwards in time to adjust older prices to make them comparable to today. It is possible to do this process forwards in time, but that has its own issues that I will not go into in this article.

Now it gets a bit more tricky.

Companies can create other companies. A good example is that of a spin off. Lets say that Widgets Inc creates a brand new company from their cool stuff division. This company we can call CoolStuff Inc. CoolStuff Inc creates a stock line. It price at the moment is 500 USD.

Lets say that the company came into being on the 3rd of July 2018.

| Date | Price |
|------------|------------:|
| 2018-07-04 | 500 |
| 2018-07-05 | 503 |
| 2018-07-06 | 506 |
| 2018-07-07 | 499 |

Seems ok so far. But what if you wanted to run an algorthim over the series, but there isn't enough data points. You would want to extend back the series with some sort of proxy.

We can do this by taking the value of Widgets Inc and then applying some sort of adjustment. This makes some sense. After CoolStuff was once a part of Widgets Inc. We might be able to make some sort of assesment that CoolStuff was around 20% of the value of Widgets Inc. 

We can take a similar approach as above:


| Date | Raw Price (CoolStuff Inc) | Raw Price (Widgets Inc) | Adjustment factor | Ajusted Price |
|------------|------------|------------|------------|------------:|
| 2018-06-05 | missing | 1005 | 0.2 | 201 |
| 2018-06-05 | missing | 1010 | 0.2 | 202 |
| 2018-06-06 | 204 | not used | 1 | 204 |
| 2018-06-07 | 205 | not used | 1 | 205 |

Where the values of for CoolStuff Inc are not available, we have used the values for Widgets Inc, and applied an ajustement factor of 0.2.

This approach allows us to create historic valuations at points in for a company that did not exist. There are many other approaches to dealing with this problem, but this is a simple approach that is in use in many enterprises.


Prices:

What is a price?

Many of us are familiar with prices, for consumer products etc. We also understand that prices for the sam thing can change over time (petrol was chaper in the past), and we understand that the price of something can be cheaper when offered in different places and by different people. The price for one apple varies from city to city, retailer to retailer, month to month. 

In our time series example, we cover the time varying aspect of the price by modelling it as a series. However, we didn't specify where and how the prices originated. 

We can therefore ask where did the prices originate, and how do they get determined. For most stocks, this is on an exchange. 

An exchange is a market place, either in physical form, or electronic where people / companies can meet to exchange items of value. Specifically in oupr case, a stock market is where people can buy and sell stocks. For an exchange to happen, both parties have to agree on the terms if the deal. Ie, which stock, how much of it and for what price per stock.

The exchange can facilitate, and record these values. In that sense, exchanges record all the prices of a given stock that were executed. We can take those prices an then use that get a sense for what the price is accross the market _on aggregate_. We might take the average of all the stock prices over a given day. This isn't the price that we might actually get if we tried to buy or sell the stock ourselves. Indeed this average price might in practise never actaully have ever been one that was executed. 

However, many people need to know how much a given stock is worth, even if they ar not going to trade it. They might need to value an existing amount of stock that some owns. Therefore there is a need for prices. The exchange might offer a service where it prices the stock by using information in the market.

So, to really know what our prices series represents, we need to know where the prices came from. Were they the average of the prices seen in the market place? Were they compiled from executed prices? Were they compiled from quoted prices only? Maybe they were just made up by someone?

Times:

All we showed 