---
id: 33
title: Give to your client a dedicated J2EE server in ASP mode
date: 2009-03-09T15:58:54+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=33
permalink: /2009/03/09/give-to-your-client-a-dedicated-j2ee-server-in-asp-mode/
categories:
  - java
---
In my last project we had to sell ASP solutions to client. The software works on our servers and the clients have to connect remotely.
  
The projet has been launced by a small non-IT company, mantain a Server Park for them is not possible. For this reason we opted for dedicated server managed by exernal service provider.

The problem of this solution is that one server is very expensive and the approach to create multiple instances of Tomcat, mysql, &#8230; is a big risk in case of problem of one instance and it&#8217;s complicated in the configuration phase.

The solution adopted is to install VMWare Server (you can do the same with Virtuozzo, XEN, &#8230;) and install an Ubuntu VM for each client. Ubuntu has a server edition conceived for the VMs ([Ubuntu JEOS](http://www.ubuntu.com/products/whatisubuntu/serveredition/jeos)). It needs only 128MB ram and 200 MB HD. We used Tomcat 6 (thanks Spring!!!) for this product (max 200MB for instance). 512 MB dedicated for each VM is largely sufficient.

Each client has a different ip port to connect to the appication. Configuring the nat.conf of VMWare on the server we can easily redirect the client request to his &#8216;dedicated server&#8217;. On a typical server with 4GB of ram we can install 8 VMs reducing the costs for us and the client.

A big advantage is the possibility to create locally the VM and transfer it to the &#8216;production&#8217; when it&#8217;s ready copying the files. The maintenance is easier because when we have to upgrade the version we can redirect the port to a different VM with the updated product. This last options is not implemented yet because we preferred to maintain the DB server in the VM with tomcat. In the future (after benchmarks) it could be interesting to install a &#8216;service&#8217; VM with the DB and access it directly from the VMs.