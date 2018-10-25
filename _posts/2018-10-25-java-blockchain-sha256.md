---
title: 'Blockchain: How to generate a SHA-256 hash with Java'
description: ''
date: 2018-10-25T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'blockchain'
color: '#7AAB13'
permalink: /hashmap-inline-initialization/
categories:
  - Java
  - Blockchain
tags:
  - java
  - blockchain
 
image: '/assets/img/'

introduction: 'Blockchain: How to generate a SHA-256 hash with Java'
---

# The hash SHA-256 for Blockchain

Bitcoin and many blockchains use the cryptographic hash function [SHA-256](https://en.wikipedia.org/wiki/SHA-2) to hash the blocks.

On the other side, Ethereum uses his own algorithm : [Ethash](https://github.com/ethereum/wiki/wiki/Ethash) and the informations of this post are not useful for Ethereum.

Here you can find more information about the [hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function).

## Javascript
In Javascript and Typescript you can use the library [crypto-js](https://www.npmjs.com/package/crypto-js) to create a new hash.

## Java
In Java we have the feature (class ;)) included in the `java.security` package: [MessageDigest](https://docs.oracle.com/javase/8/docs/api/java/security/MessageDigest.html)

The following code generate a SHA-256 byte array :

```java
public static byte[] generateSHA256(String textToHash)
    throws NoSuchAlgorithmException {
    
    MessageDigest messageDigest = MessageDigest.getInstance("SHA-256");
    messageDigest.update(textToHash.getBytes(Charset.defaultCharset()));

    return messageDigest.digest();
}
```

To see the result printed correctly we ha to convert the array of bytes in hexadecimals:

```java
private static final String HEXADECIMALS = "0123456789abcdef";

public static String convertByteArrayToHexString(byte[] byteArray) {
        
    final StringBuilder hexText = new StringBuilder(2 * byteArray.length);
        
    for (byte byteElement : byteArray) {
        hexText
        .append(HEXADECIMALS.charAt((byteElement & 0xF0) >> 4))
        .append(HEXADECIMALS.charAt((byteElement & 0x0F)));     
    }
    
    return hexText.toString();
}
```

Example:
The string `test` is converted in `9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08`.

I will add a live demo to [http://javademo.io]().