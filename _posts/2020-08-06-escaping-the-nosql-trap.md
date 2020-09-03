---
layout:     post
title:      Escaping the NoSQL trap
date:       2020-08-06 19:07:00
summary:    NoSQL is a wonderful tool, but use it appropriately and responsibly.
categories: databases tech nosql
---

I was first introduced to NoSQL when Solr was thrust upon me as a searchable
document store. It was a fantastic solution to a scaling problem we had seen,
and allowed us to offload a lot of our document searching from our relational
database to another service. What a fantastic combination of tooling which
complemented each other so well.

### We want tech, without the tech

As with most things somebody took it too far when they realised that the
document store could be used as their primary database, and the concept of
"NoSQL" was born[^0] with the introduction of technology such as MongoDB.

Akin to the fad of recent years focused around "no code"[^0] the drive to
produce products without the use of code is an interesting one, if not somewhat
naive.

[^0]: I have nothing to back this claim up, this is just how it seemed to
happen to me.

[^0]: Which almost certainly follows on from the concept of NoSQL[^0].

[^0]: Likewise.

### Who are you calling naive?

Many tech startups begin in the same way, a hastily hacked together proof of
concept which is intended as a short term solution to prove that a problem
exists and can be solved. The intention is always to replace this proof of
concept with a well engineered and architected approach which will allow the
company to grow and scale along its hockey stick trajectory as we're all led to
believe every startup does.

Ask any startup CTO and they'll all tech you the same thing. Plans change as
you begin to find your place in the market, new features are needed, and that
initial proof of concept grows, is built upon, and there isn't time or resource
to do things properly anymore.

This isn't always a bad thing. With a good team of engineers you can gradually
re-engineer your approach as you improve areas of functionality, add
appropriate testing, automate deployments, etc. You move towards a system which
is no longer proof of concept, and production ready. Sometimes that isn't
possible and you take the <strangler approach|https://martinfowler.com/bliki/StranglerFigApplication.html> which
takes longer, but in theory you have the same result.

### What does this have to do with NoSQL?

Your business needs to be up and running with something quick. You don't know
exactly how the business is going to need to pivot to find its space in the
market. You need to iterate quickly. 

Traditional relational databases require prior knowledge about the structure of
the data that you're going to store. When that structure changes you may have
difficulty adapting your platform to use the new structure, or it might take a
lot of time to make the changes[^0].

A relational database has a rigid structure, has relationships, and all of that
needs to be modified when the product changes.

Enter NoSQL. No structure. No changes. No problem?

[^0]: A future blog post will cover the idea of "functional abstraction", a
term coined by a colleague to describe a code style adopted at the time which
allowed for rapid iteration and re-factoring as you go to resolve issues.
Ideal for startups.

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
build this, you'll need new skills, and it's an entire product and team in
itself. When the business reaches a certain scale you're likely to want a
"real" data pipeline to allow a separate team to run their analytics
autonomously, and experiments without reliance on product changes, but it's a
significant cost and your choices are few. Not an ideal situation to be in -
particularly one that's avoidable.

Whichever method you choose you cannot regain what you've lost - data about how
your customers have been performing before now. You might be able to normalise
the data you have in NoSQL and load them into your choice of analytics tool,
but that relies on your data being well formed and validated, which is rarely
front of mind when diving into a NoSQL solution.

### So when should I use NoSQL?

Historically relational databases are exceptional at what they do, all the way
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
platform massively.

### Avoiding the trap

Some businesses are lucky enough that they reach a stage in their growth where
relational database scalability becomes expensive, and at that stage then
turning to a NoSQL solution makes absolute sense. Almost all platforms *will*
reach a stage where they need to perform analytics of the data they have in
their platform to assist in growth, and those that use a NoSQL database as
their primary datastore will hit problems at this point, and spend significant
funds trying to resolve this hole in their platform.

### Footnote

I'm late to the party - https://www.enterprisedb.com/blog/those-who-forget-past-are-doomed-repeat-it
