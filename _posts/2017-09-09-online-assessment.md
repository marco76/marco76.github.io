---
title: 'The online automatic technical assessment! Seriously?'
description: 'Are we artisans of the code or simple part of an assembly line?'
date: 2017-09-09T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'Dev life'
color: '#FF8300'
permalink: /online-assessment/
categories:
  - Developer life
tags:
  - Developer life
 
image: '/assets/img/'

introduction: 'Are we artisans of the code or simple part of an assembly line?'
---
# Thank you for applying, you are invited for an online technical assessment...

Many companies ar using automated online tools for the technical assessment.

On one side I can find a reason for this. I had to evaluate CV during my career and I love the candidates that with 2 years of experience are ‘Expert’ in Java, Spring, PHP, AngularJS, React, Oracle, SQLServer at the same time. I would invite them for an interview only to ask them ‘expert questions’ (e.g. what’s the difference between _findOne()_ and _getOne()_ in Spring Data).

The easy solution that many companies found to filter the hundreds of candidatures they receive is, in my opinion, very wrong. They use automated tools that ask questions about algorithm and automatically classify the developers in an excel style table to _'find easily and objectively the best candidates'_ as they advertise.

The process is the following:

- You send your CV
- <strike>An HR call you asking some basic questions like: salary, availability, why to work for us.</strike>(Soon they will get rid of the HR!)
- You receive an invitation to participate to an algorithm test
- If you succeed to be between the top candidates you can continue the recruitment process

### The HR and the company life
Why bother the HR to call you for a short introduction?

- 'why you want to come to work for us? (are you crazy or desperate?)'
- 'what is your salary expectation?' ... 'Sorry is too much for us' or 'Well we can do better'.
- 'would you like to work with this technology too?'
- 'these will be your projects are you still interested?'

Maybe you could have stupid questions like:

- do you have a company restaurant? parking place?
- is it possible to work partially (80%)? there is the possibility of 1 day of home office?
- any training plan for the employees?
- how many holidays per year?
- do I have to travel for this job?

Honestly who cares? Often these companies have so low evaluation on Glassdoor/Kununu that you can figure out the answer yourself.

### We don't look for people ...

What is very clear is that this kind of companies are not interested in your person but only in your skills. Because what they want is not an employee that contribute to the company but a consultant that cost as much as an employee to put in an assembly line.

What I love in my _consulting_ activity is that the roles are clear:

- I give them my skills and I don't bother about their management/internal politics.
- They give me my salary and they don't bother about my person.
- The project, conditions and tasks are clearly explained at the very beginning.
- If one of the party lied the relationship is immediately terminated.

If you apply for a permanent position and your first contact is an online algorithm  and not a line manager or somebody from HR well ... good luck ;)

## The technical tool

The questions of the algorithm test are like:
‘for a given text find if all the parentheses and braces are correctly open and closed e.g. _\[\[\(\{\}\)\{\(\)\}\]\(\)\]\)_'

You have to write a code parser, other questions involve b-tree structures and other Algorithm 101 exercices.

What is funny is that those questions are asked for all the development profiles (fullstack, frontend).
As fullstack consultant I regularly have to talk to the end user, find the best graphical interface for them, write the code using JS frameworks, Java frameworks, model the database, review everything with the stakeholders of the architecture, plan the testing with the QA, deploy in some cloud platform.

> I have a personal assistant, my IDE that takes care of the parentheses and braces, it remembers me the methods of every class in Java and suggest me improvements ;)

You can learn every keyword / feature of every tool that you use during the day, but maybe you can create more value learning how to quickly find a solution that solves your problem.

To solve the algorithms proposed by these tests it should be possible to simply use some existing and trusted libraries (from apache or eclipse foundation) as during the real job usually you use frameworks and you don't develop a website simply using the JDK.

Anyway, every company has the right to use whatever tool they prefer to choice their own employees/consultants.
> They have the right to ask you to prepare a coffee (extra points for one decaf) for all the team in a time box of 3 minutes during the interview.
> Sure, this involve a lot of problem solving on your side but … 
> you really want to work for them?


## Tool review

I had the opportunity to play with a couple of these tools (‘the best one, as they write in every forum’) testing the tools without the stress/time limit of a real candidate :

**I thought it was Java!** Their online ide/compiler is not really working. The exact same code working in a standard Java IDE was rejected by the tool.

**Stress management?!** During the tests the platform stopped and I had to restart few times, is it to test the stress management?

**This is a piece of paper code on it! IDE are for cheaters!** The tool is worst than notepad, with the bad pagination the code is not easy readable.
Code completion? What’s this?

I love my IDE at work, it's my assistant. I ask during the interviews which tools the company uses, in fact I avoid some of them passionately.

In these online test if you use a common IDE and you copy the answer in the text area the tools will like you as a cheater.

**Your app doesn’t work but we don’t tell you why!** This should be a test how you manage users’ tickets: ‘Description of the problem: The application doesn’t work’. The evaluation uses unit test cases to tell if your algorithm is working or not. You don’t see all the tests. Your code works with the given use cases and unit tests. You submit the answer and the platform says that your code is wrong. Why? Who knows? Who cares?

**A real developer don't use a debugger**: forget to have a debugger, many of the true developers still use punched cards. The debugger is for weak devs and should be eliminated by the IDE.

**Quality? Why? We cannot measure it!** The evaluation is based on unit tests, write 300 lines of bad code and you will be good. Maybe somebody will comment: ‘The guy wrote 300 lines of code in 2 minutes, cool!’

**Who cares the maintenance costs?** Time is an important criterion for the evaluation 300 lines of bad code in 5 minutes are better than a refactored solution of 5 lines in 2 hours. You will be classified by the time and points.
> The message is ‘Be quick …  the assembly line is running fast!’

**You are a compiler not a developer!** The questions are for compilers a compiler is the only one to judge the solution.

**Remember the college / Uni ?** These tools have as commercial target Universities. If you apply for the first job you will have more chances than many ‘senior’ developers.

**We are watching you!** Everything you do with this tools is recorded, be prepared to work for big brother!

**Why use existing libraries when you can rewrite from scratch?** You have to solve problems involving Arrays and Strings. Can you use Java Collections classes? No! Lambdas? Not supported! Regex? Not supported! Apache Commons? What is this?!
> It’s like to be a developer in the ’90 using a beta of the JDK 1.0.

**The best skill of a goalkeeper, score penalties!** As in football/soccer/hockey the goalkeeper is evaluated on his skills to score penalties, every developer of the company is evaluated on the base of the same algorithm.

**Organize a coding party!** To avoid cheating these platform don’t allow you to copy the code you tested in your favourite IDE but allows you to invite friends at home to solve the algorithms. The recommended composition of the team is : a developer, a mathematician and university student/professor.

**Build a wood boat and go to the moon!** The problem are so irrealistic that nowhere on the web you will find the answer, they even repay you if you find the same question on the web. What is ridicoulous is that I found the answers to the questions on the ‘sister’ website of the company that sells those tests.

**Only the bad developers use StackOverflow to look for solutions!** Most of the programmers that I know and (some are very good) use web forums (like SO) to quickly find a solution to an existing problem (or decipher a misterious error code). According to these tools this is bad and they ask you absurd questions to avoid that some rational person published a solution on the web.

## Alternatives?

### 1. Show me something and explain it to me

One of the best approach I found during my career is used by some innovative companies (e.g. Appian).

For the technical assessment you have 3 days to develop a project and present it to your future team.

> The concept is based on the principle ‘the best idea wins’ the candidate has full freedom to find the best solution with a given framework of development.

Some candidates without a strong technical background created great applications without any notion of b-tree structures.

### 2. Do you exist on the internet ?

Do you have a GitHub repository? Do you participate in some open source projects?
This approach is used by some companies that ask for the github account. Not all the developers have the time to do it after a full day of work and a family at home.

For me this is a very valuable nice to have.

## How I test these companies ?

When I receive the online assessment invitation I answer something like:

_'as alternative I propose that you send to me : a subject of your choice, your favourite technology stack (e.g. java, angular, mongodb), your favourite cloud._

_In a couple of days you will find my solution deployed online at the adress www.something.com using the technologies you choose and the code on GitHub. If you like my solution it you can invite me for the technical grill about my code (quality, tests, etc.) or any other technical subject of your choice'_

> I am a software artisan (not a code blue collar) and I love to show it :)

> I'm very happy to invest few hours of my time to find a cool and innovative company!

### ... their answer

The answers are similar to quotes from 1984 of George Orwell and they refere to 'procedures and standardisation', 'classification of people', 'company policy', 'objective criteria' and so on.

For me is clear they don't want innovative people, no colors between the blue collars. Many would like to work in this kind of companies, I don't.

> In some companies 'the best idea wins' and the employees are challenged to improve the existing status quo of the company.

This helps the immune system of a company to stay strong.

> In other companies 'somebody will think for you, don't bother about this, follow the orders and conform to them until your deprecation'.

When I get this answer they fail my test and I feel lucky to be an external consultant! :D

## How to succeed in an online assessment test

You really want to work for one of this companies?

- I suggest you follow the MOOC of Coursera on data structures and algorithms. It should allow you to easily beat senior developers.
- I recommend to study how Regex works, you could easily solve this quizzes quickly with only 1 line without fighting with the IDE. The risk is that regex is not supported by these tools.
- The coding party with friends and free beers is a good and enjoyable option too. You can show your social and management skills. Some devs in real life when they don't know how to solve a problem they will ask on SO and it will cost less to the company that 1 week of try and error.

## Reading suggestions

[How I ended up conducting the most successful technical interviews with a single question](http://www.nicolasbize.com/blog/how-i-ended-up-conducting-the-most-successful-technical-interviews-with-a-single-question/)

[Why I Don't Talk to Google Recruiters](http://www.yegor256.com/2017/02/21/say-no-to-google-recruiters.html)