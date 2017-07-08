---
id: 608
title: 'Java Server Sent Event &#8211; Automatically update web pages'
date: 2016-04-17T17:06:32+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=608
permalink: /2016/04/17/java-server-sent-event-automatically-update-web-pages/
categories:
  - Glassfish
  - java
  - Java EE
  - JSON
  - REST
  - tutorial
  - Web Services
---
Code source here: <https://github.com/marco76/javaSSE>
  
How a client (ex. web browser) can get updates from a server? Here some options:

## _1. Polling_

The client regularly request to the server new data using a simple request/response via HTTP.

[<img class="alignnone wp-image-611 size-full" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_1_2016-04-17_11-15-19-e1460905354536.png?resize=314%2C124" alt="sse_1_2016-04-17_11-15-19" data-recalc-dims="1" />](https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_1_2016-04-17_11-15-19.png)

## 2. _Websocket_

Java EE 7 and Spring 4 implemented the [WebSocket](https://en.wikipedia.org/wiki/WebSocket) protocol that allows a bi-directional communication between a client and a server using TCP. The HTTP is used only for the handshake. WebSockets are the appropriate solution for application that need frequent exchange of small chunks of data at high speed (ex. trading, videogames, &#8230;).

[<img class="alignnone wp-image-610 size-full" src="https://i2.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_2_2016-04-17_11-15-29-e1460905439922.png?resize=343%2C131" alt="sse_2_2016-04-17_11-15-29" data-recalc-dims="1" />](https://i2.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_2_2016-04-17_11-15-29.png)

## 3. _Server-side events_

HTML5 allows an unidirectional communication similar to the publish/subscribe model in JMS. The protocol used is HTTP and is described in the [W3C documentation](https://www.w3.org/TR/eventsource/) (note than [IE is not compatible](http://www.w3schools.com/html/html5_serversentevents.asp)).

[<img class="alignnone wp-image-609 size-full" src="https://i2.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_3_2016-04-17_11-15-43-e1460905505391.png?resize=350%2C149" alt="sse_3_2016-04-17_11-15-43" data-recalc-dims="1" />](https://i2.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_3_2016-04-17_11-15-43.png)

The events can be broadcasted multiple clients:

[<img class="alignnone wp-image-612 size-full" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_4_2016-04-17_14-09-19-e1460905566225.png?resize=295%2C155" alt="sse_4_2016-04-17_14-09-19" data-recalc-dims="1" />](https://i2.wp.com/javaee.ch/wp-content/uploads/2016/04/sse_4_2016-04-17_14-09-19.png)

This solution is appropriate for application that require to be notified about new events (newsfeed, twitter-like, stock market etc.)
  
In Java the support for SSE is not yet a standard (it should be in Java EE 8). You can implement it using a Servlet or use some of the libraries that already support it : Jersey and Spring.

The following example uses Jersey in a Java EE environment (Weblogic).

## Example

In this example we simulate the &#8216;live&#8217; visualisation of a simple running competition. The result time (and the time of appearance) is randomly defined.

The class that produces the result is a standard REST java class. The resource must produce a SERVER\_SENT\_EVENTS type. The method return an [EventOutput](https://jersey.java.net/apidocs/2.8/jersey/org/glassfish/jersey/media/sse/EventOutput.html) class. This object maintain the connection with the client and send the results with the [write()](https://jersey.java.net/apidocs/2.8/jersey/org/glassfish/jersey/server/ChunkedOutput.html#write(T)) method

<pre class="brush: java; title: ; notranslate" title="">@Path("competition")
public class SseEventOutput {

	static Logger logger = Logger.getLogger("HelloSse.class");

	// EventOutput is created when the client open this resource
	// the method return the object EventOutput to the client (http)
	// until when the EventOutput is not closed it can send data to the client using write()
	
	EventOutput eventOutput = new EventOutput();
	
	@Path("results")
	@GET
	@Produces(SseFeature.SERVER_SENT_EVENTS)
	public EventOutput getServerSentEvents() {
</pre>

In the next fragment of code we show when the new data is sent to the client browser using the write method:

<pre class="brush: java; title: ; notranslate" title="">// prepare the OutboundEvent to send to the client browser
OutboundEvent event = buildOutboundEvent(runner);
						
logger.log(Level.INFO, "writing the result for the participant in position: " + position);

// the event is sent to the client
// the EventOutput is already returned to the client but until when is not closed it can send messages to the client
eventOutput.write(event);
												// waiting for the next runner
position++;
</pre>

The data to send is created and stored in the [OutboundEvent](https://jersey.java.net/apidocs/2.9/jersey/org/glassfish/jersey/media/sse/OutboundEvent.html) object. In the object is defined in which format the data will be sent (JSON).

<pre class="brush: java; title: ; notranslate" title="">private OutboundEvent buildOutboundEvent(final Runner runner){
    OutboundEvent.Builder eventBuilder = new OutboundEvent.Builder();
    eventBuilder.name(LocalTime.now() + " - runner at the finish ... ");
		
    // the runner object will be converted to JSON
    eventBuilder.data(Runner.class, runner);
    eventBuilder.mediaType(MediaType.APPLICATION_JSON_TYPE);
	    
return eventBuilder.build();
}
</pre>

Here the result of the execution:
  
[<img class="alignnone wp-image-618 size-full" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-07-34_chrome.png?resize=440%2C173" alt="2016-04-17_16-07-34_chrome" data-recalc-dims="1" />](https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-07-34_chrome.png)
  
and the log:
  
[<img class="alignnone wp-image-619 size-full" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-07-58_log.png?resize=630%2C159" alt="2016-04-17_16-07-58_log" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-07-58_log.png)
  
While the connection was open Chrome showed the &#8216;working in progress&#8217; icon:
  
[<img class="alignnone wp-image-617 size-full" src="https://i2.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-06-05_chrome.png?resize=125%2C27" alt="2016-04-17_16-06-05_chrome" data-recalc-dims="1" />](https://i2.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-06-05_chrome.png)

## Broadcast

In the class SSEBroadcast you can find an example of how the broadcast implementation works.

In the image you can see the result of sending 3 messages using post (green terminal) to the resource /api/hello\_sse\_broadcast/send.
  
The black terminal and chrome opened the page /api/hello\_sse\_broadcast/listen and they received the messages (chrome connected later and received only 2 messages).

[<img class="alignnone size-full wp-image-620" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-25-22_clients.png?resize=571%2C520" alt="2016-04-17_16-25-22_clients" data-recalc-dims="1" />](https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-17_16-25-22_clients.png)

The implementation of the broadcast require a Singleton:

<pre class="brush: java; title: ; notranslate" title="">@Path("hello_sse_broadcast")
@Singleton // singleton, one producer and multiple listener
public class SseBroadcast {	
	
	SseBroadcaster broadcaster = new SseBroadcaster();
</pre>

We implemented one resource that return an EventOutput object. This object allows the communication between the server and the client. This resource is called by the client that subscribe the notifications/events :

<pre class="brush: java; title: ; notranslate" title="">/**
	 * When a client connect to this resource it opens a communication channel.
	 * @return
	 */
	@Path("listen")
	@GET
	@Produces(SseFeature.SERVER_SENT_EVENTS)
	public EventOutput listenData() {
		 final EventOutput eventOutput = new EventOutput();
	        this.broadcaster.add(eventOutput);
	       
	        return eventOutput;
	}
</pre>

Another resource receive the POST messages and create an event to send to the subscribers:

<pre class="brush: java; title: ; notranslate" title="">/**
	 * For each call to this method some data is broadcasted to the listeners
	 * @return
	 */
	
	@POST
	@Path("send")
	public String sendData(@FormParam(value = "text") String text) {
		OutboundEvent.Builder builder = new OutboundEvent.Builder();
		builder.comment("optional comment: " + text);
		builder.data(Calendar.getInstance().getTime().toString());
		builder.mediaType(MediaType.APPLICATION_JSON_TYPE);
		
		broadcaster.broadcast(builder.build());
		
		return "date sent";

	}
}
</pre>

Interesting references:
  
<http://streamdata.io/blog/push-sse-vs-websockets/>