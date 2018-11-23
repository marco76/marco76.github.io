---
id: 628
title: Change the JRE / JDK in Eclipse on Mac OS X and access the sources
date: 2016-04-20T14:19:40+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=628
permalink: /2016/04/20/change-the-jre-jdk-in-eclipse-on-mac-os-x-and-access-the-sources/
categories:
  - Eclipse
  - java
  - tutorial
tags:
  - eclipse
  - ide
  - java
  - jdk
  - tutorial
---
Sometimes it happen that we have to change or add the JDK/JRE version used by eclipse and/or we want to access the correct source code of the JDK.

Here the step by step procedure under OS X:
  
1. Open File -> Preferences
  
[<img class="alignnone size-full wp-image-629" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_13-53-18.png?resize=205%2C223" alt="2016-04-20_13-53-18" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_13-53-18.png)
  
2. Search for the Installed JRE (currently the 1.8.0_73) and click &#8216;Add&#8230;&#8217;

[<img class="alignnone size-full wp-image-631" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_11-57-15-1.png?resize=1134%2C178" alt="2016-04-20_11-57-15" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_11-57-15-1.png)3. Select the JRE Home

If you installed the SDK using the installer it should be in:

/Library/Java/JavaVirtualMachines/jdk1.[version].jdk/Contents/Home/[<img class="alignnone size-full wp-image-632" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_11-58-59.png?resize=751%2C155" alt="2016-04-20_11-58-59" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_11-58-59.png)

4. To access the sources select rt in the System Libraries and click Source Attachment &#8230;[<img class="alignnone size-full wp-image-633" src="{{site.baseurl}}/assets/img/uploads/2016/04/2016-04-20_11-58-59_2.png?resize=751%2C100" alt="2016-04-20_11-58-59_2" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2016/04/2016-04-20_11-58-59_2.png)

5. Choose the src.zip file from the directory containing the JDK

[<img class="alignnone size-full wp-image-634" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_11-59-27.png?resize=594%2C208" alt="2016-04-20_11-59-27" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_11-59-27.png)

6. Confirm and set the new JRE as the default one clicking on the checkbox near the name [<img class="alignnone size-full wp-image-635" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_12-00-23_2.png?resize=710%2C174" alt="2016-04-20_12-00-23_2" data-recalc-dims="1" />](https://i0.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_12-00-23_2.png)

&nbsp;

The change is effective only for the <span style="text-decoration: underline;">new projects</span>, if you want to change the JDK/JRE for an existing project:

7. Click with the right button on the project and choose &#8216;Properties&#8217;. Search for &#8216;Java Build Path&#8217; and select the JRE under Libraries. Click te &#8216;Edit&#8217; button.
  
[<img class="alignnone size-full wp-image-636" src="{{site.baseurl}}/assets/img/uploads/2016/04/2016-04-20_12-01-12.png?resize=747%2C144" alt="2016-04-20_12-01-12" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2016/04/2016-04-20_12-01-12.png)

8. Choose the new JRE

[<img class="alignnone size-full wp-image-637" src="{{site.baseurl}}/assets/img/uploads/2016/04/2016-04-20_12-01-33.png?resize=611%2C216" alt="2016-04-20_12-01-33" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2016/04/2016-04-20_12-01-33.png)

9. (Optional) Restart Eclipse

Without a restarting if you try to call the JRE sources Eclipse will throw an error

[<img class="alignnone size-full wp-image-638" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_12-02-29.png?resize=897%2C79" alt="2016-04-20_12-02-29" data-recalc-dims="1" />](https://i1.wp.com/javaee.ch/wp-content/uploads/2016/04/2016-04-20_12-02-29.png)

&nbsp;