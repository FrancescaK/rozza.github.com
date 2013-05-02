---
layout: post
title: "MongoEngine 0.8 RC1 Released"
date: 2013-05-01 11:00
comments: true
published: true
categories:
- mongodb
- mongoengine
- tech
- nosql
- python
---

**Notice: ** 0.8.0RC2 has been released fixing an obscure queryset cursor cloning issue.


I'm really pleased to annouce the release candidate for [MongoEngine 0.8](https://pypi.python.org/pypi/mongoengine/0.8.0RC1)!
Its been a *long process* due to work and life commitments but the latest version
of MongoEngine is ready for testing and feedback.

Why a release candidate?
------------------------

There have been massive changes in the internals, requiring thought and
**testing** before upgrading and releasing to the wild - so please
read the [upgrade](http://docs.mongoengine.org/en/latest/upgrade.html#to-0-8)
docs carefully!

The changes are worth it and make using [MongoEngine](http://mongoengine.org)
even better.

Please test 0.8.0RC1 on your test systems and staging systems and any feedback
please email the [user group](https://groups.google.com/group/mongoengine-users)
or if you'd prefer to message me directly you can via
[https://github.com/rozza](https://github.com/rozza)

Whats changed?
--------------

There have been loads of fixes, improvements and changes in 0.8.
The headliners are:

<!--more-->

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

And much, much more! See the full [changelog](http://docs.mongoengine.org/en/latest/changelog.html#changes-in-0-8-0)
for all changes or check [github](https://github.com/MongoEngine/mongoengine/) to see the
[84 tickets](https://github.com/MongoEngine/mongoengine/issues?milestone=2&page=1&state=closed)
that have been fixed as part of this release.

Try it now!
-----------

Please test it, try it out and report any issues back!
All being well 0.8.0RC1 will be released as 0.8.0 in a week.

<small>* Want to get involved in MongoEngine? We're looking for help so please ping me!</small>

