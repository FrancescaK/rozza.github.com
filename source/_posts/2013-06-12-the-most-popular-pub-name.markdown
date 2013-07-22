---
layout: post
title: "Using MongoDB to find the most popular pub names!"
date: 2013-07-16
published: false
comments: true
categories:
- mongodb
- aggregation
- demo
- tech
- geo
---

Earlier in the year I gave a talk at [MongoDB London](http://www.10gen.com/events/mongodb-london-2013)
about the different aggregation options with MongoDB.  The topic recently came up again
in conversation at a user group, so I thought it warranted a blog post. 

## Gathering ideas for the talk

I wanted to give a more interesting aggregation talk than the standard
"counting words in text", and as the [aggregation framework](http://docs.mongodb.org/manual/core/aggregation/) gained shiny [2dsphere geo](http://docs.mongodb.org/manual/core/2dsphere/) support in
[2.4](http://docs.mongodb.org/manual/release-notes/2.4/#new-geospatial-indexes-with-geojson-and-improved-spherical-geometry), I figured I'd use that. I just needed a topic...

### What are us Brits focused on?

Two things immediately sprang to mind: **weather** and **beer**.
<!--more-->

I opted to focus on something close to my heart: **beer** :)
But what to aggregate about beer? Then I remembered an old pub quiz favourite...

**What is the most popular pub name in the UK?**

I know there is some great open data, including a wealth of information
on pubs available from the awesome [open street map](http://www.openstreetmap.org/)
project.  I just need to get at it and happily the [Overpass-api](http://www.overpass-api.de)
provides a simple "xapi" interface for OSM data.
All I needed was anything tagged with `amenity=pub` within in the
bounds of the UK and with their xapi interface this is as simple as a wget:
`http://www.overpass-api.de/api/xapi?*[amenity=pub][bbox=-10.5,49.78,1.78,59]`

Once I had an osm file I used the [imposm python library](http://imposm.org/) to
parse the xml and then convert it to following GeoJSON format:

<pre>
<code class="javascript">{
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

Then it was a case of simply inserting it as a document into MongoDB. I
quickly noticed that the data needed a little cleaning, as I was seeing duplicate
pub names, for example: "The Red Lion" and "Red Lion".  Because I wanted to make
a wordle I normalised all the pub names.

If you want to know more about the importing process, the full loading code is
available on github: [osm2mongo.py](https://github.com/rozza/pubnames/blob/master/osm2mongo.py)

## Top pub names

It turns out finding the most popular pub names is very simple with the
aggregation framework.  Just group by the name and then sum up all the occurrences.
To get the top five most popular pub names we sort by the summed value and then
limit to 5:

<div class="row-fluid">
  <div class="span6">
<pre>
<code class="javascript">db.pubs.aggregate([
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

<p class="center">
  <img src="/images/2013/pubs_wordle.png"<br>
</p>

## Top pub names near you

At MongoDB London I thought that was too easy, so filtered to find the top pub
names near the conference and showing off some of the geo functionality that became
available in MongoDB 2.4.  To limit the result set match and ensure the
location is within a 2 mile radius by using `$centreSphere`. Just provide the
coordinates `[ <long>, <lat> ]` and a radius of roughly 2 miles
(3959 is approximately the radius of the earth, so divide it by 2):
<pre>
<code class="javascript">db.pubs.aggregate([
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

## What about where I live?

At the conference I looked the most popular pub name near the conference.  Thats
great if you happen to live in the centre of London but what about everyone else
in the UK? So for this blog post I decided to update the demo code and make it
dynamic based on where you live.

See: [pubnames.rosslawley.co.uk](http://pubnames.rosslawley.co.uk)

Apologies for those outside the UK - the demo app doesn't have data for the
whole world - its surely possible to do, I just lacked the patience to download
it all!

## Cheers

<p class="center">
  <img src="/images/2013/almurray.jpg" title="Original image: http://www.flickr.com/photos/bradfordtheatres/3063899946" /> <br>
  <small> Source: <a href="http://www.flickr.com/photos/bradfordtheatres/3063899946"/>http://www.flickr.com/photos/bradfordtheatres/3063899946</a></small>
</p>

All the code is available in my repo on [github](https://github.com/rozza/pubnames)
including the bson file of the pubs and the wordle code - so fork it and start
playing with MongoDB's great geo features!
