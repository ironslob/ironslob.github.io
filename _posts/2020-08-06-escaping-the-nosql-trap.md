---
layout:     post
title:      Escaping the NoSQL trap
date:       2020-08-06 19:07:00
summary:    NoSQL is a wonderful tool, but use it appropriately and responsibly.
categories: databases tech nosql
---

I was first introduced to NoSQL when Solr was thrust upon me as a searchable
document store. It was a fantastic solution to a scaling problem we had.
It allowed us to offload most of our document searching from our relational
database to another service. It was a fantastic combination of tooling which
complemented each other so well.

### We want tech, without the tech

As with most things, somebody took it too far. They realised that the
document store could be used as their primary database. "NoSQL" was born[^1].

Akin to the fad of recent years focused around "no code"[^2] the drive to
produce products without the use of code is an interesting one, if not somewhat
naive.

[^1]: I have nothing to back this claim up, this is just how it seemed to happen to me.

[^2]: Which almost certainly follows on from the concept of NoSQL[^3].

[^3]: Likewise.

### Who are you calling naive?

Many tech startups begin in the same way. A rapidly hacked together proof of concept intended as a short term solution to prove an idea.  We promise ourselves that we'll replace it with a well engineered and architected approach. The re-imagined solution will be clean and free from the hacks. It will help the company to grow along its hockey stick trajectory as we're all led to believe every startup does.

Ask any startup CTO and they'll all tell you the same thing. Plans change.
You begin to find your place in the market. new features are needed, and that
initial proof of concept grows. It becomes your platform, and there isn't time
or resource to do things properly anymore.

This isn't always a bad thing. A good team of engineers will gradually
re-engineer your approach as you improve areas of functionality, add
appropriate testing, automate deployments, etc. You move towards a system which
is no longer proof of concept, and production ready. Sometimes that isn't
possible and you take the [strangler approach](https://martinfowler.com/bliki/StranglerFigApplication.html) which
can take longer, but gets you there in the end.

### What does this have to do with NoSQL?

Your business needs to be up and running with something quick. You don't know
exactly how the business is going to need to pivot to find its space in the
market. You need to iterate quickly. 

Traditional relational databases require prior knowledge about the structure of
the data that you're going to store. When that structure changes you may have
difficulty adapting your platform to use the new structure, or it might take a
lot of time to make the changes[^4].

A relational database has a rigid structure, has relationships, and all of that
needs to be modified when the product changes.

Enter NoSQL. No structure. No changes. No problem?

[^4]: A future blog post will cover the idea of "functional abstraction", a term coined by a colleague to describe a code style adopted at the time which allowed for rapid iteration and re-factoring as you go to resolve issues.  Ideal for startups.

### Data, data, data!

Wait a moment, the use of data is a big thing now. We need to be able to
extract information from the data that we gather from our users so that we can
produce insights that can direct the business. Google Analytics can only take
you so far before being able to understand a bit more about your users, and how
they interact with your platform.

Suddenly the choice for using NoSQL rather than a structured database format
lets you down. While you were quick to get something up and running,
more than likely it's not possible for you to perform even the most basic
analysis of the data you've gathered, or build products on the back of it.

Some of the features that you're likely to want are available by third party
platforms (£££) but many of the insights won't be. For example, tracking a
history of changes for an entity becomes difficult, or analysing search
patterns to find out how people look for products.

### Becoming data driven

As mentioned, there is always the possibility of dropping in third party tools,
though you will end up paying good money for them at a time when the business
perhaps can't afford it. Sometimes they will be better, and you'll use them
anyway.

For other things you have little choice but to move to some sort of data
pipeline to allow you to track user events, and model their behaviour in a way
that allows you to understand how they use your platform, and where they aren't
engaging in a way you'd like. This is costly as you'll need a team of people to
build this, you'll need new skills, and it's an entire product in itself.
Resources you could spend elsewhere.

Whichever method you choose you cannot regain what you've lost - data about how
your customers have been performing before now. You might be able to normalise
the data you have in NoSQL and load them into your choice of analytics tool,
but that relies on your data being well formed and validated, which is rarely
front of mind when diving into a NoSQL solution.

### So when should I use NoSQL?

Historically, relational databases are exceptional at what they do, all the way
up to the point where they hit a scaling limit. Whether it's I/O, memory, CPU,
or something else, the cost of scaling further becomes increasingly expensive.
Sharding, replicas, clustering - whatever your database offers, the complexity
increases exponentially.

Most platforms will introduce some sort of caching layer to alleviate some of
the load, and buy you some time, but this only helps up to a point.

During this scaling process many platforms reach a point in their evolution
where a denormalised representation of their data stored in a NoSQL document
store makes sense. It offers rapid retrieval of dependant data with a single
retrieval, rather than many. This is where NoSQL really comes into its own. By
re-engineering your platform to offload some of your data retrieval onto the
NoSQL document store you can increase the horizontal scalability of your
platform considerably.

### Avoiding the trap

Some businesses are lucky enough that they reach a stage in their growth where
relational database scalability becomes expensive, and at that stage then
turning to a NoSQL solution makes absolute sense. Almost all platforms *will*
reach a stage where they need to perform analytics of the data they have in
their platform to assist in growth, and those that use a NoSQL database as
their primary datastore will hit problems at this point, and spend significant
funds trying to resolve this hole in their platform.

I have yet to see a company that started with a NoSQL database as their primary
datastore not hit painful problems when it comes to all of the things that I've
discussed in this article. There are always countless reasons why it was the
right decision, even in the face of the facts. It's expensive, it's hard to
"fix", and it forces you to grow before you're ready, or in a way that is not
top priority.

Common reasons I've heard.

#### We needed to move fast!

Structure does not slow you down, if done properly. Use an appropriate database
migration tool, and don't be afraid to make changes that need to be made.

#### Relational databases are too slow!

If you have found one of the few non-contrived examples where a NoSQL database
outperforms a PostgreSQL database, hats off to you. I'm sure there are some.

Appropriate use of indexes and appropriate field types is all you need. Don't
be afraid to break the rules around normalisation either, engineers may be
idealists, but the best solutions rarely are.

#### Nobody knows SQL

This is a tough one. ORMs can help you here, as can any one of the million
relational frameworks available for your language of choice.

My advice here would be - learn SQL. Understanding the data you're processing
and how to query it effectively will be invaluable down the line.

#### We have *no idea* what [specific] data is going to look like!

PostgreSQL never fails to impress me, and it has a wonderful
 [datatype for exactly this purpose](https://www.postgresql.org/docs/9.4/datatype-json.html).
It can be queried, it can be indexed, and PostgreSQL is *FAST*.

#### We want to use the newest technology!

Boring technology always trumps the cool kid on the block. This will be my next
article!

### Summary

Set yourself up for success. You owe it to the company you're building to have
a little maturity in your tech, and save yourself a lot of headaches in the
future. Adopt data early, and use it to grow even faster!

I hope your business reaches a scale where you need a real data pipeline, and a
team dedicated to it. A team which can operate autonomously, and run
experiments without months of product development. I also hope that you choose
to go down that path when you need it, and that you don't have it forced upon
you due to technical myopia.

### Footnote

[I'm late to the party](https://www.enterprisedb.com/blog/those-who-forget-past-are-doomed-repeat-it)
