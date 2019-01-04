---
title: 'Create a list of objects from a file using lambda expressions'
description: ''
date: 2019-01-04T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'java'
permalink: /java-read-file-list-lambda/
categories:
  - Java
tags:
  - java
 
image: '/assets/img/'

introduction: 'Read the file, create and sort the object list'
---
## The lambda expression

```java
public List<Conference> readInputStream(InputStream inputStream) throws IOException {

    // target list
    List conferenceModelCollection;
    try (
          // open the url stream, wrap it an a few "readers"
        BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream))) {
        // read the file, line by line
        conferenceModelCollection = reader.lines()
        // skip the first 4 lines - contain metadata
        .skip(4)
            // take the line and convert into an object
            .map(this::convertToConference)
            // sort the lines by date
            .sorted((f1, f2) -> f1.getBegin().compareTo(f2.getEnd()))
            // add to the existing collection
            .collect(Collectors.toList());
    }

    return conferenceModelCollection;
}
```


## Example of the converter

```java
private Conference convertToConference(String dataLine) {
    String[] data = dataLine.split("\\|");
    int dataPosition = 1;

    Conference conference = new Conference();

    conference.setName(data[dataPosition++]);
    conference.setWebsite(data[dataPosition++]);
    conference.setBegin(LocalDate.parse(data[dataPosition++], DATE_FORMATTER));
   ...

    return conference;
}
```

## Read the file

```java
private List<Conference> readCalendarDataFromUrl() throws IOException {
    URL url = new URL(urlString);

    return readInputStream(url.openStream());
}
```
