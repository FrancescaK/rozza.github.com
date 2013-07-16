---
layout: post
title: "Using MongoDB to find the most popular pub names!"
date: 2013-06-12 14:50
published: false
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

Two things immediately sprang to mind: **weather** and **beer**.
<!--more-->

Being a fan of Al Murray - the pub landlord and
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
parse, convert to GeoJSON, load to MongoDB and clean up the data.  The resulting
document looks like:

<pre class="line-numbers language-javascript">
<code>{
  "_id" : 451152,
  "amenity" : "pub",
  "name" : "The Dignity",
  "addr:housenumber" : "363",
  "addr:street" : "Regents Park Road",
  "addr:city" : "London",
  "addr:postcode" : "N3 1DH",
  "toilets" : "yes",
  "toilets:access" : "customers",
  "location" : {
      "type" : "Point",
      "coordinates" : [-0.1945732, 51.6008172]
  }
}
</code></pre>

See the [osm2mongo.py](https://github.com/rozza/pubnames) if you want to know
more.

## Top pub names

<p class="center">
  <img src="/images/2013/pubs_wordle.png"<br>
</p>

It turns out finding the most popular pub names is simple with the aggregation
framework.  Just group by the name and then sum up all the occurrences of that
name.  To get the top five most popular pub names we sort by the summed value
and then limit to 5:

<div class="row-fluid">
  <div class="span6">
<pre class="line-numbers language-javascript">
<code>db.pubs.aggregate([
  {"$group":
     {"_id": "$name",
      "value": {"$sum": 1}
     }
  },
  {"$sort": {"value": -1}},
  {"$limit": 5}
]);
</code></pre>
  </div>
  <div class="span6">
For the whole of the UK this returns:

<ol>
<li>The Red Lion</li>
<li>The Royal Oak</li>
<li>The Crown</li>
<li>The White Hart</li>
<li>The White Horse</li>
</ol>
  </div>
</div>

## Top pub names near you

At MongoDB London I did the top pub names near the conference, showing off some
of the geo functionality that is available in MongoDB 2.4.  Here I'm find all
pubs within ~2miles of the venue:

<pre class="line-numbers language-javascript">
<code>db.pubs.aggregate([
    { "$match" : { "location":
                 { "$within":
                   { "$centerSphere": [[-0.12, 51.516], 2 / 3959] }}}
    },
    { "$group" :
       { "_id" : "$name",
         "value" : { "$sum" : 1 } }
    },
    { "$sort" : { "value" : -1 } },
    { "$limit" : 5 }
  ]);
</code></pre>


# What about where I live?

I decided to make it a bit more dynamic and playing with d3.js and leaflet.js,
so I hacked together a website that will create a wordle for pub names near you!

* two caveats:
1. I only got the **UK** pub names from open street map and
2. Its on free hosting so may take some time to load!



## Cheers

<p class="center">
  <img src="/images/2013/almurray.jpg" title="Original image: http://www.flickr.com/photos/bradfordtheatres/3063899946" /> <br>
  <small> Source: <a href="http://www.flickr.com/photos/bradfordtheatres/3063899946"/>http://www.flickr.com/photos/bradfordtheatres/3063899946</a></small>
</p>

