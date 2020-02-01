---
id: 523
title: 'Android: consume a REST / HTTPs service using JSON and Threads'
date: 2016-01-27T11:37:35+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=523
permalink: /2016/01/27/android-consume-a-rest-https-service-using-json-and-threads/
categories:
  - java
  - REST
  - tutorial
  - Uncategorized
  - Web Services
tags:
  - Android
  - java
  - JSON
  - REST
  - Thread
---
GitHub: <https://github.com/marco76/stockVoiceTicker>

In this example we show how to implement some Android features:<img class="alignright" src="/wp-content/uploads/2016/android_ticker.png" alt="app_screenshot" width="157" height="279" />

  * Text-to-speech
  * Read a JSON web stream
  * Use a thread to communicate with a website (REST)
  * Timer

The application is very limited and has been created for educational purpose only.

&nbsp;

**1.** **Read a webpage content (REST/HTTPs)**

With Android if you want to read a webpage you have to create a thread. Android doesn&#8217;t allow you to open an URL connection from the Main (thread) because it could be resource intensive and time consuming. If you try to use the main Thread you will get this exception [android.os.NetworkOnMainThreadException](http://developer.android.com/reference/android/os/NetworkOnMainThreadException.html).

In our case we are calling a [Yahoo service using YQL](https://developer.yahoo.com/yql/) with the following URL : [https://query.yahooapis.com/v1/public/yql?q=select%20LastTradePriceOnly%20from%20yahoo.finance.quote%20where%20symbol%20in%20(%22GOOG%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys&callback= ](https://query.yahooapis.com/v1/public/yql?q=select%20LastTradePriceOnly%20from%20yahoo.finance.quote%20where%20symbol%20in%20(%22GOOG%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys&callback=)

And we wait an answer in JSON format similar to this one.

<pre style="color: #000000; word-wrap: break-word; white-space: pre-wrap;">{"query":{"count":1,"created":"2016-01-27T08:42:41Z","lang":"en-US","results":{"quote":{"LastTradePriceOnly":"713.04"}}}}</pre>

In our application we create a Task in the main thread using [ExecutorService](http://developer.android.com/reference/java/util/concurrent/ExecutorService.html) and [Future](http://developer.android.com/reference/java/util/concurrent/Future.html):

<pre class="brush: java; title: ; notranslate" title="">private String readFromWeb() {  
        // THREAD  
        final ExecutorService service;  
        // this Task will contact Yahoo and store the answer (web page) in a String  
        final Future&lt;String&gt; task;    
        String jsonString = null;  
</pre>

The Future task will receive the result of the HTTPS request and it will store it in a String (using task.get()).

<pre class="brush: java; title: ; notranslate" title="">service = Executors.newFixedThreadPool(1);  
task = service.submit(new YahooQuote(YAHOO_URL_PRE + ticker.getText() + YAHOO_URL_POST));  
try {
    jsonString = task.get();  
} catch (final InterruptedException | ExecutionException ex) {  
      ex.printStackTrace();  
  } finally {  
     service.shutdownNow();  
}  
</pre>

The class ([YahooQuote](https://github.com/marco76/stockVoiceTicker/blob/master/app/src/main/java/ch/javaee/voiceStockTicker/YahooQuote.java)) that communicate with Yahoo implements Callable and must override the call() method:

<pre class="brush: java; title: ; notranslate" title="">// Task that return a String
public class YahooQuote implements Callable&lt;String&gt;{
</pre>

To pass the URL to the class we create a new constructor that receive the parameter:

<pre class="brush: java; title: ; notranslate" title="">private final String jsonURL;

    public YahooQuote(String jsonURL) {
        this.jsonURL = jsonURL;
    }
</pre>

The result is stored in the String jsonString.

**2. Parse JSON**

Now we have our json result in a String. We can easily transform the String in a JSON object with the function &#8230; [JSONObject](http://developer.android.com/reference/org/json/JSONObject.html):

Because of the nature of the answer (multiple level JSON objects) we have to create a new JSON object for each level

<a href="{{site.baseurl}}/assets/img/uploads/2016/01/json_multiple.png" rel="attachment wp-att-528"><img class="alignnone size-medium wp-image-528" src="{{site.baseurl}}/assets/img/uploads/2016/01/json_multiple.png?resize=300%2C124" alt="json_multiple" data-recalc-dims="1" /></a>

<pre class="brush: java; title: ; notranslate" title="">private String readPrice(String json) throws JSONException {
        JSONObject jsonObject = new JSONObject(json);
        JSONObject query = jsonObject.getJSONObject("query");
        JSONObject results = query.getJSONObject("results");
        JSONObject quote = results.getJSONObject("quote");
        return quote.getString("LastTradePriceOnly");

    }
</pre>

We get the price result with [getString()](http://developer.android.com/reference/org/json/JSONObject.html#getString(java.lang.String));