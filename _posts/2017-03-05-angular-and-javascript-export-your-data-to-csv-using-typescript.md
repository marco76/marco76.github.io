---
id: 985
title: 'Angular and JavaScript: export your data to CSV using Typescript'
date: 2017-03-05T00:45:29+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=985
permalink: /2017/03/05/angular-and-javascript-export-your-data-to-csv-using-typescript/
image: /wp-content/uploads/2017/03/excel-100x37.png
categories:
  - Angular
  - Angular 2
  - AngularJS
  - java
  - javascript
  - TypeScript
tags:
  - csv
  - excel
  - export
  - TypeScript
---
Npm: <https://www.npmjs.com/package/@molteni/export-csv>

GitHub: <https://github.com/marco76/export-csv>

Limitations: This method doesn&#8217;t work on Safari.

Online example: <http://angular.cafe>

Goal: easily export the table&#8217;s from your Angular or JavaScript application to Excel / CSV

Very often in our professional web application we visualise information from a database in tables. A common requirement is to export this data to an excel file.

Usually this requires to generate the file on the backend (Java etc.) and send it to the client.

If all the data is already present on the client we can simply use a Typescript function.

### Install the npm package

<pre class="brush: bash; title: ; notranslate" title="">npm i @molteni/export-csv --save</pre>

In your application you can import the package with the following declaration:

<pre class="brush: jscript; title: ; notranslate" title="">import ExportToCSV from "@molteni/export-csv";</pre>

and call the export for only some columns e.g.:

<pre class="brush: jscript; title: ; notranslate" title="">var exporter = new ExportToCSV();
exporter.exportColumnsToCSV(this.blogArticles, "filename", ["title", "link"]);
</pre>

or for all the columns, e.g.:

<pre class="brush: jscript; title: ; notranslate" title="">var exporter = new ExportToCSV();
exporter.exportColumnsToCSV(this.blogArticles, "filename")
</pre>

### What it can do

Here the content of the definition file:

<pre class="brush: jscript; title: ; notranslate" title="">export declare class ExportToCSV {
    constructor();
    exportAllToCSV(JSONListItemsToPublish: any, fileName: string): void;
    exportColumnsToCSV(JSONListItemsToPublish: any, fileName: string, columns: string[]): void;
    static downloadFile(filename: string, data: string, format: string): void;
    static download(filename: string, data: any): void;
}
</pre>

### How it works

The method reads the object passed and puts them in an array. The array is stored in a blob in the navigator ([msSaveBlob](https://msdn.microsoft.com/en-us/library/windows/apps/hh441122.aspx)):

<pre class="brush: jscript; title: ; notranslate" title="">let blob = new Blob([data], {type: format});
window.navigator.msSaveBlob(blob, filename);`
</pre>

To download a &#8216;temporary&#8217; link is created and the click on the link simulated:

<pre class="brush: jscript; title: ; notranslate" title="">let elem = window.document.createElement('a');
elem.href = window.URL.createObjectURL(blob);
elem.download = filename;
document.body.appendChild(elem);
elem.click();
</pre>