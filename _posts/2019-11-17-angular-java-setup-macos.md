---
title: 'Angular and Java Setup'
description: 'Install Java and Angular on MacOS'
date: 2019-11-17T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'java'
color: '#7AAB13'
permalink: /java-angular-setup
categories:
  - Java, Angular
tags:
  - Java, Angular

introduction: 'Quick and easy install Java and Angular'
---

# Setup of Angular and Java on MacOS

I take the opportunity to setup the dev environment for a new MacBook to write this post.
I try to reduce the steps using external 'standard' tools.

## Angular installation
1. Install Homebrew [https://brew.sh](https://brew.sh)
Homebrew will install the MacOs `Command Line Tools`if not present yet.
2. `brew install angular-cli`

This should install `node`, `npm` and `angular cli` on your system.

## Java - install multiple JDK
For Java I need multiple JDK versions installed and easily switch betweeen them.
For this job there is an excellent tool: [https://sdkman.io/install](https://sdkman.io/install).
In alterative you can try: https://github.com/shyiko/jabba or https://www.jenv.be

1. install SDKMAN: `curl -s "https://get.sdkman.io" | bash`
2. To use it without opening a new terminal: `source "/Users/marco/.sdkman/bin/sdkman-init.sh"`
3. You can see the list of the available Java versions:
 `sdk list java`
or install the last stable version of the SDK:
`sdk install java`

example:

``` cmd
> sdk install java

Downloading: java 11.0.5.hs-adpt

In progress...

######################################################################### 100.0%

Repackaging Java 11.0.5.hs-adpt...

Done repackaging...
Cleaning up residual files...

Installing: java 11.0.5.hs-adpt
Done installing!


Setting java 11.0.5.hs-adpt as default.
```

4. I like to test the last build available (partial result of `sdk list java`):
``` cmd
 Java.net |  | 14.ea.22 | open |  | 14.ea.22-open       
          |  | 13.0.1   | open |  | 13.0.1-open         
          |  | 12.0.2   | open |  | 12.0.2-open
```

with `sdk install java 14.ea.22-open` I can play with the preview of java 14

```cmd

Installing: java 14.ea.22-open
Done installing!

Do you want java 14.ea.22-open to be set as default? (Y/n): Y

Setting java 14.ea.22-open as default.

```

To use a version installed in the current terminal:
`sdk use java 11.0.5.hs-adpt`

To dchange the default:
`sdk default java 11.0.5.hs-adpt`

You can find the commands list here: [https://sdkman.io/usage](https://sdkman.io/usage)

## Maven
brew and SDKMAN allow you to easily install other useful tools for your development.
e.g. Maven (homebrew: `brew install maven`, SDKMAN : `sdk install maven`
