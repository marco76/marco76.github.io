---
id: 759
title: A simple Node.js REST server simulator for Testing
date: 2016-10-19T09:52:00+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
guid: http://ngjava.com/?p=96
permalink: /2016/10/19/a-simple-node-js-rest-server-simulator-for-testing/
categories:
  - AngularJS
  - Demo Application
  - JSON
  - Node.js
  - REST
  - Uncategorized
---
#### Problem

<p dir="auto">
  You are developing a client application (AngularJS) and you need to receive the data from a REST API service.<br /> Often you don’t have the access to the remote service or it doesn’t have the required data for your tests (dinamically generated data).
</p>

[<img class="aligncenter" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/10/node_mini_problem_1.png?resize=600%2C173" align="middle" data-recalc-dims="1" />](https://i1.wp.com/javaee.ch/wp-content/uploads/2016/10/node_mini_problem_1_full.png)

A direct access to a test .json file is not possible from the client because of security restrictions of the browser (the browser should not be able to play with the filesystem).

#### Solution &#8211; Concept

<p dir="ltr">
  A client app can request (GET) a pre-defined page to a local server and receive the JSON file for the test.<br /> The server can be implemented in Node.js and it serves static files content as response to http requests.
</p>

<img class="aligncenter" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/10/node_mini_server_3.png?resize=600%2C356" align="middle" data-recalc-dims="1" />

&nbsp;

#### Solution &#8211; Implementation

<p dir="ltr">
  The code and the installation procedure are here: <a href="https://github.com/marco76/node_rest_server/" target="_blank">https://github.com/marco76/node_rest_server/</a>
</p>

<p dir="ltr">
  The structure of the code is very simple:
</p>

[<img class="aligncenter" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/10/node_mini_server_structure_1.png?resize=300%2C191" align="middle" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/10/node_mini_server_structure_1_full.png)

<p dir="ltr">
  Here an example of response:
</p>

[<img class="aligncenter" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/10/node_mini_server_response_1.png?resize=600%2C210" align="middle" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/10/node_mini_server_response_1_full.png)

&nbsp;

#### Solution &#8211; Details

<p dir="ltr">
  The file server.js create a new http server and waits for http requests.<br /> It instantiates a <em>loaderModule</em> that contain the <em>class</em> that will retrieve the JSON data.
</p>

<pre class="brush: jscript; title: ; notranslate" title="">// import the function from the module
    var loaderModule = require('./ResponseLoader.js');
    // create an instance of the prototype
    var loader = new loaderModule("loader");

    function requestService(request, response){
        // url == filename without extension 
        var url = request.url;
        
        // home page called
        if (url=="/"){
            url = "/index";
        }
        
        // call the method that load the static page 
        loader.load(url);
        
        // prepare the http response
        response.statusCode = 200;
        response.setHeader('Content-Type', 'application/json');
        // add the JSON content
        response.end(loader.getJson());
    }</pre>

<p dir="ltr">
  The second important Javascript file is the function that receives the request to load a file and retrieves the content:
</p>

<pre class="brush: jscript; title: ; notranslate" title="">function ResponseLoader (name){

    // the content of the file is stored in this variable
    var json;

    // method that load the file on the base of the URL
    ResponseLoader.prototype.load = function(url){
        console.log("requested file: " + url);
        // url: hello -&gt; file: [current directory]/json/hello.json
        fs.readFile( __dirname + '/json' +url+'.json', function (err, data) {
            if (err) {
                console.log('error');
                throw err;
            }
            // the content of the file is assigned to the variable
            json = data;

            console.log(name + " : " +data.toString());
        })
    };

    // give me the json content
    ResponseLoader.prototype.getJson = function (){
        console.log(name + " : return json");

        return json;
    };
</pre>

<p dir="ltr">
  The features are very basic but they can be easily extended. The code is modularized using the ‚module‘ feature of node.js.<br /> If you come from Java / .NET : the import of modules is not standard in JavaScript until ECMAScript 6.
</p>