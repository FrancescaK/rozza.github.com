---
layout: post
title: "Lessons learnt building mongoengine"
date: 2012-11-22 10:00
published: false
comments: true
categories:
- mongodb
- tech
- conference
- pycon ireland
- pycon uk
---

Recently, I attended both [Pycon UK](http://www.pyconuk.org/) and [Pycon Ireland](http://python.ie/pycon/2012/) to talk about the lessons I have learnt while maintaining [mongoengine](http://mongoengine.org).  The conferences were both excellent and surprisingly different. Pycon UK had quite an *"unconference"* feel, with some exciting sprint rooms - I wish I had more time as by all reports the [educational jam](https://github.com/ntoll/pyconuk-education-sprint-2012) was inspirational.  Pycon Ireland in contrast felt more slick with booths from DemonWare, Amazon and Facebook.  If you can, I'd advise going to both conferences as they really complement each other.

It's great to go to Python conferences and see ever increasing numbers of developers adopting [MongoDB](http://mongodb.org), its such a good fit for python!  Also, I can't tell you how _great_ it is to have people come up and thank you and [10gen](http://10gen.com) for creating such a great product.  Databases aren't exactly sexy technology but as a 10gen engineer one of our core aims is to build upon the power and ease of use of mongoDB.

Back to the matter at hand, here's a quick overview of the talk.  Please check out the [slides](#slides) for all the detail I glazed over here and for the extra keen you can even [watch the video](#video) as well!

## Lessons learnt building mongoengine TL;DR

1. Dive in! <br>
  If a project is on [Github](http://github.com/mongoengine/mongoengine) it's a license to dive in.  Don't be afraid, but do be a good citizen and feedback changes to the root repository so others can benefit.

2. Metaclasses <br>
  Turns out they aren't magical! In fact how we use them in mongoengine allows us to change the class and add metadata at declaration time. This really useful trick makes creating instances of classes quick and easy.

3. Straying from the path<br>
  Mongoengine uses a [Django](https://www.djangoproject.com/) style query syntax.  This is bad in that its different from the native [pymongo](http://api.mongodb.org/python/current/) syntax.  But its also good as this adds some sweet syntactic sugar.

4. Not all ideas are good<br>
  In mongoDB the native save operation overwrites the whole document.  This catches new users out.  So in mongonengine save tracks changes to the document and uses [atomic updates](http://www.mongodb.org/display/DOCS/Atomic+Operations).  The jury is out whether or not this is a good thing™.

5. Managing a community <br>
  I've never done it before and its not always easy but it has been worth it.  My top tip is to check out the [github network graph](https://github.com/MongoEngine/mongoengine/network) and ask committers to forks to add pull requests back upstream, failing that add tickets for useful features you've spotted downstream.  This has had an added benefit of bringing more people into the core project.

6. Don't be afraid to ask for help<br>
  Cue me asking for help on any aspect of mongoengine, be it the core, the website or framework support!  Please get in contact :)

## <a id="slides"></a> Slides

<script async class="speakerdeck-embed" data-id="507adf49f29efa00020740df" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

<a id="video"></a>
The video is available on the [PyCon Ireland channel](http://www.youtube.com/watch?v=N-S_yuhZtAw).
