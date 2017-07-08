---
id: 116
title: Connect to HP Quality Center using Java
date: 2011-10-20T15:53:16+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=116
permalink: /2011/10/20/connect-to-hp-quality-center-using-java/
jabber_published:
  - "1319122398"
categories:
  - HP Quality Center
  - java
  - Uncategorized
---
You can access HP Quality Center using java. To do this you have to use com4j to create the wrapper classes to the OTA library.

here some variables that you will use:

<pre class="brush: java; title: ; notranslate" title="">ITDConnection connection = ClassFactory.createTDConnection();
 IBugFactory bugFactory;
 ITDFilter2 filter;
 IList bugList;
</pre>

ex. how to connect to QC:

<pre class="brush: java; title: ; notranslate" title="">private void connectQC(){
    connection.initConnectionEx("http://youradress:84/qcbin");
    connection.connectProjectEx("YOUR_DOMAIN", "YOUR_PROJECT", "UserName", "");

      bugFactory = (IBugFactory) connection.bugFactory().queryInterface(IBugFactory.class);
      // retrieve all the fields
      IList fields = bugFactory.fields();
      filter = bugFactory.filter().queryInterface(ITDFilter2.class);
    }
</pre>

ex. how to retrieve the bug inserted bewtween 2 dates:

<pre class="brush: java; title: ; notranslate" title="">public List getBugLsa(String dateBegin, String dateEnd){
         private ITDFilter2 filter;
         private IList bugList;
         filter.clear();
         String queryString = "&gt;= "+dateBegin+" and &lt;= " + dateEnd;
         System.out.append("query: c + queryString);
         filter.filter("BG_DETECTION_DATE", queryString);
         IList bugList = filter.newList());
         ...
        return result;
   }
</pre>

**Update August 7, 2012**
  
I stopped to use Quality Center at my client working place, for this reason I don&#8217;t have time to investigate hot to improve the connection between Java and HPQC. I will publish some code next week on github and I will post here the link, my program simply connect to QC to create some statistical reports (I used Play, JQuery and excel as view), nothing too complex.
  
The main problems I found to connect my small stats program to QC are related to the version of the JDK (impossible to connect with the 64 bit JDK). I did a prototype with a C# program too and there are definitely less issues in the communication between the components compared to Java. If you have the possibility to use C# it&#8217;s the easiest way, the code it&#8217;s almost identical to the Java code.