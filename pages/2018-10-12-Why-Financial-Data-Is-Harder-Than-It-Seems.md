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

We'll a made up company called Widgets Inc. They create a stock line and issue 1 million individual shares. These are worth 100 USD each. Things go well, and the company makes good money. 