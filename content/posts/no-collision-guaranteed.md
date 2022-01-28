---
title: "No Collision Guarantee"
date: 2022-01-28T17:49:17+05:30
draft: false
tags:
  - "Problem-Solving"
  - "System-Design"
---

# Designing No Collision Guarantee System.

For my url shortening system I wanted no collisions in the shortened url.

For example:

if you pass in a url called `https://longurl.com`, the backend will send you a short url that should be unique, the structure of the url would look something like `https://shorturl.com/[slug]`

The slug is our main component, it should always be unqiue. The blog is all about ensuring the uniqueness.

Some things to note:

- The length of the slug depends on how many entries will there be?
- So for 5 digit alphanumeric slug (also called Base-62):
  26 + 26 + 10 => 62
  62^5 non-repeating values are possible.

What choices do we have to ensure uniqueness of the slug?

- Random slug

  -> The problem here is that for a request, there **can** be two same random characters (collisions).

- Using MD5

  -> Collisions are possible here as well (although very rare, but we want no collision guarantee).

- Using a <u>**Counter**</u>

  -> Counters are the only thing that can guarantee no collisions. This is true if we only use a single database, which is bad for scaling. For multiple distributed database, we again have a problem!

# The problem with counter

For a no collision guarantee URL shortening system (single database), our unique id i.e our slug will be a counter.

For example:

- our first slug will be: 10000 => (2bI)<sub>base62</sub>
- second: 10001
- third: 10002 and so on.

Now the problem is when we want to horizaontally scale and talk with multiple databases.

The different approaches with multi database counter will look like:

- One counter for all databases

  -> This will ensure atomicity, but will slow down response time

- Different counter for different databases

  -> This approach will be faster, but the problem is how will we ensure that there are no collisions between two different counters?

## Problem with counter synchronization

The problem with counter synchronization is that there can be two counters with same value, this can be solved by using Apache zookeeper.

What zookeeper would do is assign different counter ranges to different databases or servers.

For example:

- DB<sub>1</sub> is assigned counter range from 10000 to 20000
- DB<sub>2</sub> is assigned counter range from 20001 to 30000
- DB<sub>3</sub> is assigned counter range from 30001 to 40000
- DB<sub>4</sub> is assigned counter range from 40001 to 50000 and so on.

And zookeeper will also keep track of the counter usage so that it if a database exceeds the range, it is assigned a new range.

In conclusion, for our distributed no collision guaranteed url shortening service, we can use counters to guarantee no collisions and use apache zookeeper to synchronize the counters across databases.
