---
layout:     post
title:      Versioning an API (part 1)
date:       2020-04-03 13:57:00
summary:    Versioning an API is straight forward, and the right approach will help lend itself to a well engineered implementation.
categories: api tech
---

There are many different ways to version an API, and the approach that you
choose may result in a considerably cleaner and easier to maintain
implementation for your teams. First, a quick run down of the most common
approaches to versioning.

### Base of the request URI

A common approach due to its incredible simplicity, this generally involves
including the requested version number in the URI, examples being:

- `https://api.mydomain.com/v1/entity`
- `https://mydomain.com/api/v2/entity`
- `https://v1.api.mydomain.com/entity`

For any API which is likely to require versioning I would strongly advise
against this approach. The tendency here is to avoid API changes except for
major releases, and then moving the entire API incrementally forward in one
step. This very much goes against the agility most engineering teams require
when working on products.

The only real benefit to using an approach similar to this is to isolate major
versions of the API on different hostnames, and leave them running for legacy
purposes. Very few people will ever need this, and can achieve better results
with different approaches.

### Query parameters

Second to the above I have seen an approach before that uses a parameter in the
request URL to specify the API version. Predictably this results in something
akin to `https://api.mydomain.com/api/entity?version=2`.

The main downside here is from a client implementation perspective. Any
abstraction of searching (for example) will likely map query options to
parameters. Requiring the client to also include the API version in this list
increases the complexity of implementation.

### Why not use one of the above?

Generally speaking there are no rights and wrongs here, but only facts and
opinions. A few points about the approaches above to discuss:

#### Pros & cons

- Both are simple to implement from the server side, but the effort required to
  "bump" the version of the API suggests that only major versions of the API
  will ever be used, rather than minor releases including extra functionality
  which wouldn't warrant a major release.
- Large releases for major versions often result in major code duplication, a
  reluctance to fix bugs in previous releases, and a slow cadence as releases
  come extra 6 months. Bug fixes rarely get back-ported in such environments.
- Physically isolating major versions will give scalability benefits, although
  most people won't have the budget to go down this road as would require large
  teams, and duplicating infrastructures

## Ok, so what else?

There's another approach which can be adopted easily enough by client and
server which offers several key advantages over the above:

- Rapid iteration
- Regular API releases
- Code re-use
- Flexibility
- Client driven
- Easy to implement

### A brief history of GET

When a User-Agent (e.g. browser, mobile app, etc.) requests an entity from a
HTTP server it sends headers along with the request that looks similar to the
below.

```
GET / HTTP/1.1
Host: www.inphota.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:74.0) Gecko/20100101 Firefox/74.0
Accept: */*
Accept-Language: en-GB,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
```

And this tells the server a lot of useful information about what the client
understands. The particularly interesting header here is `Accept`.

Similarly, the server will respond with a corresponding "I hear you accept
application/json, and that's what I've providing", e.g.

```
HTTP/1.1 200 OK 
Content-Type: application/json; charset=utf-8
Content-Length: 11211
Last-Modified: Sat, 04 Apr 2020 10:57:57 GMT
Cache-Control: private, max-age=0, proxy-revalidate, no-store, no-cache, must-revalidate
Date: Sat, 04 Apr 2020 10:58:53 GMT
Connection: Keep-Alive
```

In this way the client has told the server what it understands, and the server
has told it explicitly what it's provided.

### Ok, so what?

These headers are used by the client to inform the server of the content it's
providing, and the content that it's expecting. Therefore it naturally follows
that this header can - and *should* - be used to also inform the server of the
**version** of the entity model that it's providing and expecting.

But wait, there's more! The flexibility of using HTTP headers for this process
gives us a unique advantage over the URI based approaches above: flexible
versioning.

Instead of using an incrementing integer to represent the major version being
used, if we instead switch to using a date then we can easily do several
things:

- Immediately tell when the API entity version was released - and tell when a
  client is out of date
- Easily instruct the server "I'm providing/expecting the entity version that
  was live on date X"
- Release minor updates and changes to entities without changing behaviour for
  existing clients
- A clean server-side approach to versioning the API which avoids duplication
  of code and keeps the logic in one place

### What does it look like?

For our example we're going to request details about coffee roasts available
from a service `api-for-we-roast-coffee.com`. We know several things about the API:

1. The latest version of this made up API as of writing is 20200401
1. The coffee roasts endpoint is `/roasts`
1. We want the data serialized as JSON

#### GET

To get an entity through `curl`, we would do something like this:

```shell
$ curl -v -H "Accept: application/json; version=20200401" \
    https://api-for-we-roast-coffee.com/roasts
> GET /roasts HTTP/1.1
> Host: api-for-we-roast-coffee.com
> User-Agent: curl/7.65.3
> Accept: application/json; version=20200401

< HTTP/1.1 200 OK
< Content-Type: application/json; version=20200401

[
    {
        "name": "inphota brew",
        "level": "medium",
        "strength": 9
    }
]

...
```

And the server responds with the version of the entity that we know how to
handle.

#### POST, PUT, etc.

Similarly, if we wanted to issue a POST, PUT, or something else which requires
an entity to be sent to the server, we would use the header `Content-Type` to
inform the server "here's the data you need for the action, it's in version X".
Example:

```shell
$ curl -v -H "Content-Type: application/json; version=20200401" \
    -d '{"name":"night time brew", "level": "dark", "strength": 2}' \
    https://api-for-we-roast-coffee.com/roasts
> POST /roasts HTTP/2
> Host: api-for-we-roast-coffee.com
> User-Agent: curl/7.65.3
> Accept: application/json; version=20200401
> Content-Type: application/x-yaml; version=20200401
> Content-Length: 47

< HTTP/1.1 201 CREATED
```

Here we're doing even more!

1. We're telling the server that the data we're providing is JSON serialized,
    and it's entity version 20200401
1. We're also - rather cheekily - asking for the response to be in YAML
    (assuming we serialize to YAML), in version 20200401. Note: it's standard
    for a POST to return 201 and no content.

## Stuff for my next post

I hope this post has been educational, though I've really only gotten started on the benefits of an approach like this. Common questions include:

- What if somebody doesn't specify a version?
- What if the version doesn't exist?
- How does this make API implementation easier?
- How do you expect the client to juggle all these versions?
- Why does this mean the server team can do more releases?
- What's with all the coffee?

I'll answer all of these and hopefully provide some useful suggestions for your API.

# UPDATE 09/04/2020

I've had several people raise the question about whether to use vendor specific MIME types instead of the pre-defined ones, such as `application/json`. For examples:

- `application/vnd.mattwilson+json`
- `application/vnd.mattwilson.20200401.json`
- `application/vnd.mattwilson.json; version=20200401`

I suspect the answer here lies in the context of the API that you're implementing, and the intracacies of the clients. Whichever works best for you!
