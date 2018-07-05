---
layout: post
title: Thoughts on Reference Data
published: true
---

## What is the objective of these articles?

Mainly to get this stuff out of head and on to the page. But seriously, I intend via these articles to lay out a methodology for how to build a master reference data system, based on concepts and principles. I do this so that others may be inspired to do the same, and perhaps avoid a few pitfalls along the way.


## My background:

I've been working financial companies since 2001. I joined a systematic trading team in 2004 and held many positions until 2011, when my CTO asked me to head up the data effort for the systematic trading division. My then CTO was a bit of dreamer, and wanted to go down the whole domain / logical / physical modelling route. After a year of messing around with UML diagrams and having some pretty fun white board discussions, the tech team were fed up. We had created something so conceptually advanced, that no-one really understood it, and the entire project fell apart. No applications in the enterprise really used the systems we had built in earnest. It was a big disappointment, but it was a big learning curve for me. My CTO was then asked to leave... 


Wen the new guy arrived, he was a no-nonsense kind of chap, and had clearly heard that data was "a major issue". He tasked me with starting again. Thankful for a reprieve, I started in earnest to design a brand new securities master reference database, a new time series database, an internal data API, hire a new team, and decommission all the existing legacy applications - no pressure!

My new CTO then proceeded to fire all the guys on the technical team he thought were crap. One guy was canned after 1 day! Needless to say, I was keen to create something simple, and not a Business Analyst's wet dream. Something stripped down and practical. However, because I had been part of the systematic trading team for ~8 years at that point, I knew a lot of the edge cases that could trip us up, if the solution was too simple. However, it had to look simple to the new guy...

As such, some design decisions were in hindsight not optimal, because they had to be justifiable in that work/project environment. You might think that you could refactor stuff later, and for some things that is true. But when your system contains data that ends up being the lookup keys used by almost every application in your enterprise, refactors can be painful and scary things.

But, in the end we did make something that worked pretty well. I, along with the team I led, created a solid reference and time series platform. Is it perfect? No. Is it good? Yeah, pretty good.

So that's how I came to spend so much time exploring this problem space. It's certainly been the most challenging technical work I ever did. when I hired developers from other teams and fields of work, they always remarked on how tricky this area is. Many of them eventually migrated back to "non-data development" in search of an easier life!


## Has this been done before?

Yes, but having done some basic research, it seems that there isn't much out there on the internet in this domain. I suspect that's for a few reasons:


- The BAs have it all wrapped up tight. If you work in a large organisation (like a Bank), chances are you have a BA sitting near you. These guys like to hod on to a critical part of the discussion when it comes to building data platforms. Ie, the features required, the scope and the domain model. They can be quite aggressive about this. Because, if they don't have the final say on this, then you can quite reasonably ask, what are they there for? Obviously that's facetious, but its still true that a good deal of data engineers build data systems to specifications given to them, that they aren't expect to fully understand. That is not a good thing!

- The intellectual property. If you have a good data model, its intrinsically represents how your organisation views itself, the world around it and how its inner processes work. You can't publish that on the internet without being fired or sued.

- Even if you had an open source database / loaders / API implementation which anyone can download and install, its still needs to be loaded with data. In some cases, that loading operation can be an extremely manual and time intensive process. In many cases it requires expert knowledge, and even then, not all teams would agree what the true data state should be. Its a fact of life that financial data doesn't always have a final authoritative value that all users can agree on.

- The above factors make reference data extremely valuable, and those firms that commit to creating it, are not disposoed to giving their hard work to competitors for free.




All the above means that many users of reference data find themselves in the situation where they need to buid something that serves their needs, but then have all the challenges presented to them of things like digital identification schemes that they have prior experience of. As such, there's a lot of private, half baked solutions out there!



So, I've decided to showcase some of my thoughts here. I hope to also create a fully fledged open source project on github that anyone can use. So, these articles will serve to illustrate the design concepts of that project.
