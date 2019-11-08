---
title: 'Local proxy server to improve frontend development'
description: ''
date: 2019-11-08T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /local-test-proxy-server
categories:
  - Angular, Javascript
tags:
  - Angular, Javascript, Frontend, REST

introduction: 'Mix local and remote REST answers to speed up frontend dev'
---
# Simple proxy server for development
Sources: [https://github.com/marco76/dev-local-proxy-server](https://github.com/marco76/dev-local-proxy-server)

```
┌───┐                                                                     
│ A │                                                                     
│ n │  /customers ┌──────────────────┐              ┌──────────────────┐  
│ g ├────────────▶│   Local proxy    │─────────────▶│  customers.json  │  
│ u │◀────────────┤     server       │◀─────────────┤                  │  
│ l │             │                  │              └──────────────────┘  
│ a │ /products   │                  │ /products   ┌─────────────────────┐
│ r ├────────────▶│                  ├────────────▶│                     │
│   │◀────────────┤                  │◀────────────┤ http://remote-server│
│ / │             └──────────────────┘             │                     │
│   │                                              └─────────────────────┘
│ R │                                                                     
│ e │                                                                     
│ a │                                                                     
│ c │                                                                     
│ t │                                                                     
│   │                                                                     
└───┘                                                                           
```

## Goal
This local server can be used to simulate _some_ REST answers without calling directly the remote server.

This solution is useful during the development if we need some services from a remote server (e.g. login) but we want
 to test the application with local data because we are commuting/playing with different use cases etc.. 

## How it works
This local servers is a nodejs application. It acts as proxy for each request, if the request is not in the list of the 'simulated answers' this one is proxied to the target server.
The JSON simulated answers are stored in files with the name '[url].json'.

E.g. http://localhost:3000/customers will look for the file './json/customers.json'.
The active urls have to be activated in server.js changing the `LOCAL_ANSWERS_URL`, e.g. `const LOCAL_ANSWERS_URL = ['/customers']`;

If the path is not present in `LOCAL_ANSWERS_URL` the request will be sent to the remote server, e.g.:
`http://mytestserver.xyz/customers` and the answer proxied to the client.

### Configuration
The nodejs server is configured to answer requests sent to port 3000 and it looks for the server at the port 8080.
The client app has to send all the requests to port 3000.

### Start
`node server.js`


``` javascript
const http = require('http');
const fs = require('fs');

// address of the server that will answer to the requests
const PROXIED_SERVER_HOSTNAME = 'localhost';
const PROXIED_SERVER_PORT = 8080;

// this server port
const SERVER_PROXY_PORT = 3000;

// these requests are not sent to the remote server but served by the local nodejs
const LOCAL_ANSWERS_URL = ['/hello', '/test'];

http.createServer((request, response) => {
    const {method, url, headers} = request;

console.log('[server] got request for url:', url, method);

if (LOCAL_ANSWERS_URL.indexOf(url) > -1 && method === 'GET') {
    console.log('simulating url:', url, method);
    setHeaders(response);

    // remove '/' at the beginning of the file
    // url: /hello -> file: hello.json
    let filename = url.substring(1);

    fs.readFile("./json/" + filename + ".json", (err, data) => {
        if (err) throw err;

    response.write(data);
    response.end();

});
} else {
    proxyRequest(request, response);
}

}).listen(SERVER_PROXY_PORT);

function setHeaders(response) {
    response.setHeader('content-type', 'application/json');
    response.setHeader('access-control-expose-headers', 'errorCode')
    response.setHeader('access-control-allow-credentials', 'true');
    response.setHeader('vary', 'accept-encoding,origin,access-control-request-headers,access-control-request-method,accept-encoding');
    response.setHeader("Access-Control-Allow-Origin", "*");
    response.setHeader("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    response.setHeader('date', new Date());
    response.setHeader('Connection', 'keep-alive');
}

function proxyRequest(request, response) {
    const {method, url, headers} = request;
    console.log('proxying:', url);

    const httpRequestOptions = {
        hostname: PROXIED_SERVER_HOSTNAME,
        port: PROXIED_SERVER_PORT,
        path: url,
        method: method,
        headers: headers
    };

    const proxy = http.request(httpRequestOptions, function (res) {
        response.writeHead(res.statusCode, res.headers);
        res.pipe(response, {
            end: true
        });
    });

    request.pipe(proxy, {
        end: true
    });
}
```