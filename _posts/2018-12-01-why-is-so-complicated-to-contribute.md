---
title: 'How can I help, TomEE? It''s not so easy to contribute!'
description: ''
date: 2018-12-01T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'javaee'
color: '#7AAB13'
permalink: /tomee-contribute-is-not-easy/
categories:
  - Java EE
  - TomEE
tags:
  - Java EE
 
image: '/assets/img/'

introduction: 'Why is so complicated to contribute in an open source project?'
---

# The goal of this post
*The goal of this post is to show how a 'novice' (me) that want to help an open source project in his spare time could be scared and refrained by the entry steps required to start.*

There is no better moment to write some notes than when you try to do something for the first time, and you see many difficulties that the 'experts' don't see anymore because everything is taken for granted.

I invite any reader to contribute to his favourite Java project actively or share the difficulties to achieve this with the project maintainers.
 
# Tomee contribution

As consultant this December I have a bit more of time available until January 2019 (new client and project involving Java EE :-)). It's a great occasion to contribute to an open source project.

The tweet of @Tomitribe could not come at a better moment, they are looking for contributors starting in December.

<blockquote class="twitter-tweet" data-lang="de"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/TomEE?src=hash&amp;ref_src=twsrc%5Etfw">#TomEE</a> for the Holidays! For the month of December <a href="https://twitter.com/tomitribe?ref_src=twsrc%5Etfw">@Tomitribe</a> is putting a spotlight on new contributors <br>to <a href="https://twitter.com/ApacheTomEE?ref_src=twsrc%5Etfw">@ApacheTomEE</a>! Your Time to Shine &gt; Learn more <a href="https://t.co/T9FrrdLg4s">https://t.co/T9FrrdLg4s</a></p>&mdash; Tomitribe (@tomitribe) <a href="https://twitter.com/tomitribe/status/1068521371427172357?ref_src=twsrc%5Etfw">30. November 2018</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 



As a Java EE / Spring consultant I have great respect for TomEE even if I didn't have the opportunity to use their server in any project until now. Even better my new project starting in 2019 involves the use of TomEE (as a dev environment).

Why don't give a try? Here my experience about the first steps of a wannabe helper with this project.

# Is the project still active? 

Tomitribe developers are very active during the conferences and are super talented, no doubts that lot is going on but...

_Twitter: [@ApacheTomEE](http://twitter.com/@ApacheTomEE)_

The twitter account seems inactive since July 2017. Somebody could think that the project is lost in the recent transitions between Java EE 7 -> 8 -> Jakarta EE / Microprofile.

Website: _[http://tomee.apache.org](http://tomee.apache.org)_

There is no news or update's date, I could still have some doubts about the activity level. The webpage is referring to the twitter account inactive since July 2017 :(

Is it worth to participate to a project without vital signs?

All the communication in the social media is done via [Tomitribe](https://www.tomitribe.com), that is not referenced in the TomEE website.

# **Which version should I download?**

I have multiple personal projects running WildFly and Payara, I could give TomEE a try. Which version should I download? A brief description is absent:

[http://tomee.apache.org/download-ng.html](http://tomee.apache.org/download-ng.html)

TomEE plume, TomEE plus, TomEE webprofile, TomEE microprofile, OpenEJB Standalone, TomEE Plume Webapp, TomEE Plus Webapp, TomEE Webapp, TomEE Microprofile Webapp.

I guess that TomEE 8 is the implementation of Java EE 8, but it's not explicitly written. If I want to download the full Java EE implementation what should I download?

What's the difference between 'Plume' and 'Plus', what's the 'Webapp' for?

# Download the sources and build it
The sources are hosted in the apache repository:
[https://git-wip-us.apache.org/repos/asf?p=tomee.git](https://git-wip-us.apache.org/repos/asf?p=tomee.git)

The link is 'hidden' in the footer of the TomEE webpage, I'm missing a big GitHub logo that invites to 'fork me on GitHub'.

I don't have much experience with the apache repository but I see that there are recent activities and that is reassuring:
[https://git-wip-us.apache.org/repos/asf?p=tomee.git;a=summary](https://git-wip-us.apache.org/repos/asf?p=tomee.git;a=summary)

I'm more used to GitHub, GitLab and BitBucked, the apache git seems more 'old fashion' and definitely not sexy, but it's not a blocker.

At least, the sources are mirrored in GitHub, [https://github.com/apache/tomee](https://github.com/apache/tomee)
(but can I send a pull request from the GitHub mirror to the  Apache Git?).

Unfortunately, the mirror contains only the code source and not the issues/discussions.

# Issues
It took me a while to find them: 
[https://issues.apache.org/jira/projects/TOMEE/summary](https://issues.apache.org/jira/projects/TOMEE/summary)

The main page of the project shows the following project description: 
`All-Apache Java EE 6 Web Profile stack based on Tomcat`

Update needed!
This is another sign that could show that the project is stuck in the past.

I succeed to find the list of the issues, and I like the first I see:
['Unable to deploy this war on TomEE Plus using Java 11'](https://issues.apache.org/jira/browse/TOMEE-2264)

The compatibility of Java 11 and Java EE is one of my concerns at the moment.

Spring Boot is already compatible, Open Liberty is doing some progress, Payara is not compatible, WildFly has a workaround.
I fear that a delay in the Java EE / Jakarta EE compatibility with Java 11 could undermine his adoption future adoption.

The fact that there is some work to solve this particular issue is reassuring and gives me motivation to contribute.

# Build the sources

The documentation's page explains clearly how to build the sources: 
[https://github.com/apache/tomee](https://github.com/apache/tomee)

`git clone https://git-wip-us.apache.org/repos/asf/tomee.git tomee-master`

`mvn clean install`

... boomm!
 
Errors in the tests, Maven cannot build correctly.

In most of my clients' environments if a test fails the merge request is refused by Jenkins. This situation is merely unthinkable in many professional environments. Scary ... did I do something wrong?

I check the continuous build linked at the bottom of the page:
    [https://ci.apache.org/builders/tomee-trunk-ubuntu](https://ci.apache.org/builders/tomee-trunk-ubuntu)

The browser shows:

`No Such Resource`

`No such child resource.`

Something is missing.

I've more luck with the [Continuous Integration of the version 1.7](https://ci.apache.org/builders/tomee-1.7.x-ubuntu) and I can see some results. More than 50% of the results are `failure`. The version 1.7 should be the stable one, I guess but ...

At the end with the suggested **fast build** I can build the sources and start to play with the server.

# Mailing list

The twitter of Tomitribe [invites us](https://www.tomitribe.com/blog/tomee-for-the-holidays/) to participate to a TomEE mailing list and ask "How can I help?". I like to look at a mailing list archive before to subscribe it.
To find the history of the Tomee mailing lists I had to use related a search engine because I didn't find any direct link from the website.

Here what I found:
[http://openejb.apache.org/mailing-lists.html](http://openejb.apache.org/mailing-lists.html)

```
TomEE Developers Online Archives/Forums:

Search	Post	Depth	Archive URL

		2002	http://openejb.markmail.org/search/?q=type:development
		2002	http://marc.info/?l=openejb-development
		2006	http://mail-archives.apache.org/mod_mbox/tomee-dev/
		2006	http://n4.nabble.com/OpenEJB-Dev-f982480.html
		2007	http://www.mail-archive.com/dev@tomee.apache.org
```

The one I was looking for is the last one:

[http://www.mail-archive.com/dev@tomee.apache.org](http://www.mail-archive.com/dev@tomee.apache.org)

# And now?

The goal of this post is to show how some potential contributors could be scared to participate in an open source project because the entry steps are not intuitive and 'for experts only'.

> I think that TomEE is a project that deserves to be helped even more today that the Java EE / Jakarta EE community is in the middle of a significant transformation in his technological and business environment.

I'd like to participate but why it is so complicated and confusing (at least for me)?

# How can I help?

How can we help other developers to contribute to Java and open source projects?

I think that the field has to be prepared when we call the community to help us we need:
- Tutorials
- Step by step guides
- 'dont' be afraid to commit'
- Accessible infrastructure and web resources
- What kind of contributors are you? Dev? Design? Doc?

Can I help?

