---
layout: post
title: "Using MongoDB to find the most popular pub names!"
date: 2013-06-12 14:50
published: true
comments: true
categories:
- mongodb
- aggregation
- demo
- tech
---

Earlier in the year I gave a talk at [MongoDB London](http://www.10gen.com/events/mongodb-london-2013)
about the differing aggregation options for MongoDB.  The talk was well recieved
and I promised to post more about it, so when recently it came up again in
conversation at a usergroup, so I thought it was high time I should make good
on that promise!

## Gathering ideas for the talk

I wanted to give a more interesting aggregation talk than the bog standard "counting
words in text" and as the [aggregation framework](http://docs.mongodb.org/manual/core/aggregation/) gained shiny [2dsphere geo](http://docs.mongodb.org/manual/core/2dsphere/) support in
[MongoDB 2.4](http://docs.mongodb.org/manual/release-notes/2.4/#new-geospatial-indexes-with-geojson-and-improved-spherical-geometry), I figured I'd use that, next stop get some data to aggregate!

### What are us Brits focused on?

Two things immediately sprang to mind - **weather** and **beer**.
<!--more-->
Taking inspiration from Al Murray <small>- the pub landlord</small> and
knowing that I could get some great open data including a wealth of information
on pubs from [open street map](http://www.openstreetmap.org/), I opted to focus
on something close to my heart: **beer**.  But what to aggregate?
Then I remembered an old pub quiz favourite...

## What is the most popular pub name?

At the conference I did the most popular pub name near the conference.  Thats
great if you happen to live in the centre of London but what about everyone else
in the UK? For this blog post I decided to update it and make it a bit more
dynamic and in the demo app provide a map and allow you to get the most popular
pub names for the bounds of the map.  Apologies for those outside the UK - the
demo app doesn't have data for the whole world - its surely possible to do.

## Making sense of OSM data

The [Overpass-api](http://www.overpass-api.de) provides a xapi interface for
osm data.  All I needed was anything tagged with `amenity=pub` within in the
bounds of the UK.  Once I had an osm file I used the imposm python library to
parse, convert to GeoJSON, load to MongoDB and clean up the data.  See the
[osm2mongo.py](https://github.com/rozza/pubnames) if you want to know more.

## Top five pub names

It turns out finding the most popular pub name is simple:

<pre><code>
   db.pubs.aggregate([
    {"$group":
       {"_id": "$name",
        "value": {"$sum": 1}
       }
    },
    {"$sort": {"value": -1}},
    {"$limit": 5}
]);
</code></pre>

For the whole of the UK this returns:

1. The Red Lion
2. The Royal Oak
3. The Crown
4. The White Hart
5. The White Horse

