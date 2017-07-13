---
title: 'Angular, Spring Boot, REST, Security and OAuth2 - Part 1 - How to protect your app'
date: 2017-01-19T22:54:44+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=768
permalink: /2017/01/19/angular-spring-boot-and-oauth2-part-1-how-it-works/
dsq_thread_id:
  - "5565739147"
categories:
  - Angular
  - AngularJS
  - OAuth
  - Security
  - Spring Boot
tags:
  - Angular
  - java
  - OAuth
  - REST
  - security
  - Spring Security
---
Project NewWebApp, requirements:

**Architect** : ‚We have to implement <u>REST</u> Services, everything should be <u>stateless</u> and the client is a <u>JS Framework</u>. All the last techs, isn’t it cool? And avoid cookies, they are not safe, they have a bad reputation and there are too many policies against them.‘

**Business analyst**: ‚There is a welcome page, the user has to <u>login </u>to access the <u>local secured resources.</u> I suggest to avoid cookies too, they are bad for the health and they can ruin our marketing campaign.&#8217;

<p dir="auto">
  <strong>You</strong> : ‚ No session?! Rest services?! No cookies?! How I verify the identity of the user? I have to request his identity at every request?‘
</p>

### OAuth 2 can be a solution to your problem

The client has to login with the Authorization Server, this can be connected to your LDAP in the company or with an external service like Google or Amazon. This server generate a Token that is stored by the javascript client. This token has to be sent in every request from the client to your Java application. Your application ask to the server if the token is valid, in case it is affirmative the user has the authorization.
  
Here described the communication:

[<img class="aligncenter" src="{{site.baseurl}}/assets/img/uploads/2017/01/oauth2-1-1.png?resize=644%2C547" align="middle" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2017/01/oauth2-1_full.png)

&nbsp;

### Where to store the token?

The cookies are not safe and they can give a lot of troubles. One solution is to store the token in the <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage" target="_blank">session storage</a> of the browser. In this case when the user will close the browser the session will be closed.

``` javascript

saveMyToken(accessTokenYouReceivedFromTheServer) {
    yourService.$window.sessionStorage.setItem('myToken', accessTokenYouReceivedFromTheServer)
}

```

When you need it you can retrieve the token easily:

``` javascript

getMyStoredToken() {
yourService.$window.sessionStorage.getItem('myToken');
}

```

How to prepare a request with the token received from the server:

``` javascript
sendRequestToTheBackend() {
    return this.$http(method:'GET', url:'http://yourBackend/restResourceURL',
    headers: this.getHeaders()
})}

getHeaders() {
return {
'Authorization': 'Bearer ' + this.getMyStoredToken,
'Accept': 'application/json',
'X-Requested-With': 'XMLHttpRequest'
}
```

What is this 'Bearer' thing? <a href="https://tools.ietf.org/html/rfc6750" target="_blank">Here your answer</a>

How to implement the complete solution? Soon I will update the Angular / Spring Boot tutorial with a simple implementation.