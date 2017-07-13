---
id: 976
title: 'Tutorial : How to deploy your Java app to the web with AWS Elastic Beanstalk in 5 minutes'
date: 2017-03-01T01:16:27+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=976
permalink: /2017/03/01/how-to-deploy-your-java-app-in-the-web-with-aws-elastic-beanstalk-in-5-minutes/
image: /wp-content/uploads/2017/03/beanstalk-100x30.png
categories:
  - AWS
  - Demo Application
  - java
  - Spring Boot
  - tutorial
tags:
  - aws
  - beanstalk
  - devops
  - spring boot
---
To deploy your Spring Boot app in the web it really takes only 5 minutes thanks to AWS Elastic Beanstalk.

For the deploy we can choose multiple sources: code, docker image or deployable jar/war. In this example we shows the deploy through a war.

### Ingredients

  1. a Spring bootable web application (Web Server embedded). For this tutorial I use the demo app : <https://github.com/marco76/SpringAngular2TypeScript>
  2. configure the server.port of your Spring application to 5000. BeanStalk install ngix that redirect the public port 80 of the server to the internal port 5000. You can change application.properties or set a system variable in AWS.
  3. an account with AWS (the deployment of application in AWS is not free).

### Step by step procedure

  1. In the AWS services select &#8216;Elastic Bean&#8217;.
  
    <img class="alignnone size-full wp-image-971" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2017/03/1.png?resize=355%2C200" data-recalc-dims="1" />
  2. Click on &#8216;Create new application&#8217;.
  
    <img class="alignnone wp-image-957" src="{{site.baseurl}}/assets/img/uploads/2017/03/2.png?resize=209%2C67" data-recalc-dims="1" />
  3. Choose a name and write a description to your application.
  
    <img class="alignnone wp-image-958" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/03/3.png?resize=310%2C191" data-recalc-dims="1" />
  4. This will create your environment, you need to generate a new application. Click on &#8216;create one now&#8217;.
  
    <img class="alignnone wp-image-966" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/03/4.png?resize=428%2C77" data-recalc-dims="1" />
  5. Choose &#8216;Web server environment&#8217;.
  
    <img class="alignnone wp-image-969" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/03/5.png?resize=330%2C216" data-recalc-dims="1" />
  6. Select Java in the &#8216;Preconfigured platform&#8217;.
  
    <img class="alignnone wp-image-975" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2017/03/6.png?resize=355%2C213" data-recalc-dims="1" />
  7. Don&#8217;t change the option in &#8216;Application code&#8217;. AWS will install a default application.
  
    <img class="alignnone wp-image-961" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/03/7.png?resize=345%2C170" data-recalc-dims="1" />
  8. At this point you can &#8216;Create environment&#8217; to create your server instance.
  
    <img class="alignnone wp-image-967" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2017/03/10.png?resize=355%2C46" data-recalc-dims="1" />
  9. (Optional) You can choose to customize the options of the server configuration (an EC2 instance is created). In our example we don&#8217;t change anything, the configuration suggested by Amazon is enough).
  
    <img class="alignnone wp-image-962" src="{{site.baseurl}}/assets/img/uploads/2017/03/8.png?resize=333%2C268" data-recalc-dims="1" />
 10. After the confirmation the server instance is created and the demo application deployed.
  
    <img class="alignnone wp-image-964" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2017/03/9.png?resize=372%2C71" data-recalc-dims="1" /><img class="alignnone wp-image-968" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/03/11.png?resize=382%2C200" data-recalc-dims="1" /><img class="alignnone size-full wp-image-970" src="{{site.baseurl}}/assets/img/uploads/2017/03/12.png?resize=400%2C132" data-recalc-dims="1" />
 11. Near the name of your application there is the clickable URL that allows you to open the app.
  
    <img class="alignnone size-full wp-image-965" src="{{site.baseurl}}/assets/img/uploads/2017/03/13.png?resize=955%2C43" data-recalc-dims="1" />
 12. Here the sample application installed by AWS.
  
    <img class="alignnone size-full wp-image-972" src="{{site.baseurl}}/assets/img/uploads/2017/03/14.png?resize=400%2C250" data-recalc-dims="1" />
 13. To deploy your application you have to click on &#8216;Upload and Deploy&#8217; in your dashboard.
  
    <img class="alignnone size-full wp-image-963" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/03/15.png?resize=220%2C121" data-recalc-dims="1" />
 14. Upload the war that you want to deploy and give a version name to this deploy, e.g. : &#8216;My First app version 1.0&#8217;. All the deployments are archived in S3 and you can easily redeploy a previous version.
  
    <img class="alignnone size-full wp-image-959" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2017/03/19.png?resize=350%2C188" data-recalc-dims="1" />
 15. When the deployment is terminated you can click on the URL link and see your application deployed!
  
    <img class="alignnone size-full wp-image-960" src="{{site.baseurl}}/assets/img/uploads/2017/03/17.png?resize=500%2C297" data-recalc-dims="1" />

Congratulations! 🙂
  
<img class="alignnone size-full wp-image-974" src="{{site.baseurl}}/assets/img/uploads/2017/03/18.png?resize=500%2C203" data-recalc-dims="1" />

### How to delete your environment

At this point you have many interesting options, p.e. you can copy your environment, save the configuration or, my choice, to delete the servers.

This server has been created only for this tutorial, now I can delete it choosing &#8216;Terminate environment&#8217;, after confirmation AWS will destroy the instance created and all the related objects.

<img class="alignnone size-full wp-image-973" src="{{site.baseurl}}/assets/img/uploads/2017/03/20.png?resize=200%2C262" data-recalc-dims="1" />