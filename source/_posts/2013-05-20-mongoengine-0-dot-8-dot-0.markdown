---
layout: post
title: "mongoengine 0.8.0"
date: 2013-05-20 12:30
comments: true
published: true
categories:
- mongodb
- mongoengine
- tech
- nosql
- python
---

I'm really pleased to annouce the release of
[MongoEngine 0.8.0](https://pypi.python.org/pypi/mongoengine/0.8.0).
Its been a *long process* due to work and life commitments but the latest version
of MongoEngine is here.

Whats changed?
--------------

There have been loads of fixes, improvements and changes in 0.8.
The headliners are:

  * Minimum requirements are python 2.6+ and pymongo 2.5+
  * Inheritance now off by default
  * Inherited documents store `_cls` not `_types` (preventing auto creation of
  	multikey indexes)
  * Querysets are immutable and now return clones
  * New Geo Fields supporting mongodb 2.4 new "2dSphere" indexes
  * New context managers for switching collections or databases on the fly as
  	well as turning off dereferencing
  * Django support improved (now supports Django 1.5.1, groups and permissions)
  * Performance improvements, back to the same performance as 0.4

And much, much more!
<!--more-->
See the full [changelog](http://docs.mongoengine.org/en/latest/changelog.html#changes-in-0-8-0)
for all changes or check [github](https://github.com/MongoEngine/mongoengine/) to see the
[96 tickets](https://github.com/MongoEngine/mongoengine/issues?milestone=2&page=1&state=closed)
that have been fixed as part of this release.

Whats next?
-----------

There are a number of ideas on [github](https://github.com/MongoEngine/mongoengine/issues?milestone=6) for 0.9.  I'm looking for people to help with the administration and coding
of MongoEngine, so please get involved and help drive MongoEngine forward!

