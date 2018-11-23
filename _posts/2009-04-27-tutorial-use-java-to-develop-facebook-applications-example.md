---
id: 73
title: 'Tutorial: use Java to develop Facebook applications'
date: 2009-04-27T11:27:08+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=73
permalink: /2009/04/27/tutorial-use-java-to-develop-facebook-applications-example/
categories:
  - Facebook
  - java
  - Spring
---
Create a FB application is funny and easy with Java. There are some open source libraries that can help you to achieve your goal. You find these libraires here :
  
<http://wiki.developers.facebook.com/index.php/Java>
  
I used the facebook-java-api to create this tutorial. It’s a good library but unfortunately it miss documentation (in the traditional OSS old style 😉 )
  
<http://code.google.com/p/facebook-java-api/>
  
I used SpringMVC as framework, it’s very easy to pass values to web pages with Spring.
  
I created 2 pages , one for the login (index.htm) and a second page (results.htm) that shows the results :
  
Code of the first page :

<pre class="brush: java; title: ; notranslate" title="">IFacebookRestClient fbClient = new FacebookJsonRestClient(API_KEY, SECRET_CODE)

FacebookWebappHelper fh = new FacebookWebappHelper(request, response, API_KEY, SECRET_CODE, fbClient);
        fh.requireLogin("");
        
        if (!fh.isLogin())
            return null;
</pre>

The goal of this page is to login the user if is still not logged in. Is in your application configuration on Facebook that you can decide where to send the user after the succesful login.

The second page contains this code :

<pre class="brush: java; title: ; notranslate" title="">protected ModelAndView handleRequestInternal(
            HttpServletRequest request,
            HttpServletResponse response) throws Exception {
  String token = ServletRequestUtils.getStringParameter(request, "auth_token");
  IFacebookRestClient fbClient = new FacebookJsonRestClient(API_KEY, SECRET_CODE);
  fbClient.auth_getSession(token);

  // get your Facebook id
  Long id = fbClient.users_getLoggedInUser();
  
  // query Facebook database, ask for all friends single and give me their uid and their complete name
  JSONArray sqlResult = (JSONArray) fbClient.fql_query("SELECT uid, name FROM user WHERE  relationship_status='single' AND uid IN (SELECT uid2 FROM friend WHERE uid1 = " + id.toString() + ")");

  // write the result in a List
List users = new ArrayList();
        for (int i = 0; i &lt; sqlResults.length(); i++) {
            User usr = new User();
            usr.setUid(Long.valueOf(sqlResults.getJSONObject(i).getString("uid")));
            usr.setName(sqlResults.getJSONObject(i).getString("name"));
            users.add(usr);
        }

// send the result to the webpage
ModelAndView mav = new ModelAndView();
mav.addObject("users", users);
return mav;
}
}
&#91;/sourcecode&#93;

This code fragment return the list of my friends single and I put this friends in an ArrayList of User (class included in facebook-java-api).
Show the results in a jsp page is very easy :

&#91;sourcecode language='html'&#93;
&lt;table border="1"&gt;
              &lt;tr&gt;&lt;td&gt;First name&lt;/td&gt;&lt;td&gt;Name&lt;/td&gt;&lt;/tr&gt;      
              &lt;tr&gt;&lt;td&gt;${user.uid}&lt;/td&gt; &lt;td&gt;${user.name}&lt;/td&gt;&lt;/tr&gt;
&lt;/table&gt;
</pre>