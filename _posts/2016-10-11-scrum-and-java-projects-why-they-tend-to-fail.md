---
id: 709
title: Scrum and Java projects. Why they tend to fail?
date: 2016-10-11T08:28:00+00:00
author: Marco Molteni
excerpt: |
  Why we think that scrum is the solution to every project?
  Why we don’t develop a methodology specific to our technology stack?
layout: post
main-class: 'other'
published: false
guid: http://javaee.ch/?p=709
permalink: /2016/10/11/scrum-and-java-projects-why-they-tend-to-fail/
image: /wp-content/uploads/2016/10/1475832328_featured-1-e1476167480550.jpeg
categories:
  - agile development
  - java
  - Project management
  - scrum
tags:
  - java
  - project management
  - scrum
---
<p dir="auto">
  As consultant I saw a lot of client passionate about scrum:
</p>

> <p dir="auto">
>   ‚Scrum works very well!‘ ‚Scrum sucks!‘ ‚We partially (?!) adopt Scrum‘ and so on.
> </p>

<p dir="auto">
  Many people think that scrum is a methodology but in reality is only a framework.<br /> Spring is a framework too. It sucks? It’s productive? Well it depends how you use it, how you know it. The same is true for every framework.
</p>

<h2 dir="auto">
  When the motivation goes away Scrum is counterproductive
</h2>

<p dir="auto">
  I have seen teams that were simply not motivated to use it or simply bored after few weeks. The 15 (or more) minutes of daily meeting was a simple routine that required a simple answer for the greatest happiness of all the participants :
</p>

> <p dir="auto">
>   ‚I worked on that feature. No blocking issues.‘
> </p>

<p dir="auto">
  During those meeting I saw participants continuing to work at their laptop, eating, watching TV series (!), chatting with the neighbour, reading the news on the smartphone … but everybody was certainly multitasking and listening with attention the other talking.
</p>

<h2 dir="ltr">
  The notion of (almost) done
</h2>

<p dir="ltr">
  What about the notion of done? &#8216;The task is done!‘ (Developer 1) ‚Cool! Update excel, if we have time somebody will do a peer review‘ (Developer 2). Result: the task was more or less ‚done&#8217; and the peer review will be realized by the client on their validation system or, probably, on their production environment.
</p>

## The issues in a Java project?

<p dir="ltr">
  The more frequent issues in my experience are:
</p>

#### Communication between developers

<p dir="ltr">
  most of the wonderful talent in IT are not the best in communication, scrum try to help but the human nature is hard to change.
</p>

#### Specifications and users

<p dir="ltr">
  they change all the time, they are incomplete most of the time, they are often impossible to implement. Agile methodologies try to help. The notion of product owner helps a lot but the artifacts will continue to change. The users are becoming more and more client oriented consultants and are losing their specific business skills. The developer need a lot more business information. Every exception has to be managed, in development we often spend more time implementing solutions to problems that the user will never see. Invite a user in a technical meeting … he will get a new perspective on his own activity. The analysts? If you are lucky they were skilled developers, otherwise … good luck!
</p>

#### Time and budget

<p dir="ltr">
  a sales guy sold a project based on an estimation of the total cost of the project based on a draft of business concept. A developer (that is not currently involved in the project) initially estimated the effort at 100 (‚it’s easy‘), the project manager added a risk margin of 50% (‚150 should be ok‘).<br /> As we know the initial estimation is in 90% of the projects completely wrong. This issue is not solved by the common project frameworks that assume endless time or feature stop at the end of the time. The pure agile is good for internal and continuous development. When you face clients that pay for your solution endless budget and feature stop are simply not realistic.
</p>

#### Technical decisions

<p dir="ltr">
  today there are hundreds of frameworks, every year a new trend impact the new development projects for few years. Those frameworks survive only a couple of years before to ‚disappear‘ but their cost is immense for many years : Flex, Grails, GWT, ExtJS, JSF, AngularJS (1.x) are a few example. The result is that the projects today are a toxic mix of old and new technologies glued together.
</p>

#### Technical skills

<p dir="ltr">
  many developers know the syntax (Java, .Net, JS) and they are correctly qualified as ‚developers‘. The problem is that to build quality the syntax is not enough. You need to know the grammar and the rhetoric too. Is this enough? Not yet, ask 5 author to write a chapter of the same book. The result is inconsistency, repetition and so on. The architecture and the lead are very important in this context.
</p>

#### All developers equals? The team is not the sum

<p dir="ltr">
  some (agile) theories say that the developers will find the best solution themselves in an agile context. Is it true? Not according to my experience. The alpha guys will try to impose their view, arguing if necessary. Without a lead everybody will fill his tasks without caring how the others are working. A structure in the code has to be implemented and somebody has to take the lead on this. The structure can be challenged to improve the quality and productivity. To improve the maintanability everybody in the team has to be able to read the code of the others.
</p>

#### Individualities

<p dir="ltr">
  often the management think that all the developers are the same, same skills, same productivity. ‚We are late? We add 5 new developer for 2 weeks and the project is finished‘. The new Fordism applied to the development. In reality the situation is more similar to a football (or soccer or any other sport) team. Some people are excellent defenders but unskilled strikers. Soft skills, technical skills, personal constraints are important factors that can define the success or the failure of a project. If you have Messi and Ronaldo in your team you are lucky. If Messi will play as goalkeeper and Ronaldo as central defender … well … disaster announced.
</p>

## Solutions?

<p dir="ltr">
  &#8216;Ok, thanks! We already know it. Do you have any practical solution?‘
</p>

<p dir="ltr">
  Well scrum is a generic framework that can be used in a Java project as well in a house building project or prepare a new recipe.<br /> A complete solution require time to be elaborated. There are some techniques that I can suggest to improve the work for/in a team. My question is the following:
</p>

> <p dir="ltr">
>   To increase the quality of our Java (or language X) project why we don’t use a Java-oriented (or language X) dedicated framework?
> </p>

<p dir="ltr">
  You can leave any comment about your direct experience.
</p>