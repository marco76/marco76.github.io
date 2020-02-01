---
id: 985
title: 'Angular and JavaScript: export your data to CSV using Typescript'
date: 2017-03-05T00:45:29+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
guid: http://marco.dev/?p=985
permalink: /2017/03/05/angular-and-javascript-export-your-data-to-csv-using-typescript/
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

<img src="{{site.baseurl}}/assets/img/uploads/2017/03/excel.png" />

Npm: <https://www.npmjs.com/package/@molteni/export-csv>

GitHub: <https://github.com/marco76/export-csv>


Goal: easily export arrays and JSON data from your Angular or JavaScript application to Excel / CSV

Very often in our professional web application we visualise information from a database in tables. A common requirement is to export this data to an excel file.

Usually this requires to generate the file on the backend (Java etc.) and send it to the client.

If all the data is already present on the client we can simply use a Typescript function.

### Install the npm package

```typescript
npm i @molteni/export-csv --save
```

In your application you can import the package with the following declaration:

```typescript
import ExportToCSV from "@molteni/export-csv";
```

and call the export for only some columns e.g.:

```typescript
var exporter = new ExportToCSV();
exporter.exportColumnsToCSV(this.blogArticles, "filename", ["title", "link"]);
```

or for all the columns, e.g.:

```typescript
var exporter = new ExportToCSV();
exporter.exportColumnsToCSV(this.blogArticles, "filename")
```

#### Code snippet with Angular and UTF-8
<img src="{{site.baseurl}}/assets/img/uploads/excel_small.jpg" />

```typescript
exportToCSV = new ExportToCSV();
  objectToExport = [{column : 'äàü£™ , ®, ©,'}, {column: 'second row'}];
  download() {
    this.exportToCSV.exportColumnsToCSV(this.objectToExport, 'filename.csv', ['column']);
  }
```

### What it can do

Here the content of the definition file with the methods available.

```typescript
export declare class ExportToCSV {
    constructor();
    exportAllToCSV(JSONListItemsToPublish: any, fileName: string): void;
    exportColumnsToCSV(JSONListItemsToPublish: any, fileName: string, columns: string[]): void;
    static downloadFile(filename: string, data: string, format: string): void;
    static download(filename: string, data: any): void;
}
```

### How it works

The method reads the object passed and puts them in an array. The array is stored in a blob in the navigator ([msSaveBlob](https://msdn.microsoft.com/en-us/library/windows/apps/hh441122.aspx)):

```typescript
let blob = new Blob([data], {type: format});
window.navigator.msSaveBlob(blob, filename);`
```

To download a temporary link is created and the click on the link simulated:

```typescript
let elem = window.document.createElement('a');
elem.href = window.URL.createObjectURL(blob);
elem.download = filename;
document.body.appendChild(elem);
elem.click();
```

### Excel and UTF-8
Excel has issues importing UTF-8 CSV files. You can read more here: [StackOverflow](https://stackoverflow.com/questions/155097/microsoft-excel-mangles-diacritics-in-csv-files).

Our implementation is compatible with Excel and the UTF-8 characters should correctly represented in Excel.