---
title: 'Angular Master Detail Grid'
description: ''
date: 2018-07-24T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /2018/07/24/angular-master-detail-table/
redirect_to:
- https://www.ngjava.com/angular-master-detail-table/
categories:
  - Angular
tags:
  - angular
 
image: '/assets/img/'

introduction: 'A technical challenge, not a big of interest'
---

# Autoconfigurable Data Grid with detail

In one project I had the opportunity to develop a Data Grid with expandable rows. We used the standard Angular Material as base for the development.

The original [DataTable](https://material.angular.io/components/table/overview) don't support expandable rows, with a bit of research and many trial and error we succeeded to build a pretty good component.

<img src="{{site.baseurl}}/assets/img/uploads/2018/05/grid_twitter.gif" />

With a very ambitious approach we decided to automatically generate the forms of the grid (dates, booleans, selects, texts) using an optional JSON configuration object.

The component don't seem to attract a lot of interest outside of the project. I think that not many projects are based on Angular Material.
Material is not easy to implement compared to other frameworks/guidelines/libraries and don't give a lot of freedom to the developer.

I personally enjoy using it for company solutions but I'm not sure I would choose Material for a public website.

If somebody is blocked trying to solve the same problem here he can find some code fragments. The full code is not open source at the moment (need some work).


Call on the cell that open the template:

``` javascript
onDetailGrid(row: any, data: string, column: AvTableColumnConfig) {
   
       row.selectedTemplate = this.gridTemplate;
       if (typeof row.isExpanded === 'undefined') {
         row.isExpanded = false;
       }
   
       row.isExpanded = !row.isExpanded;
       row.selectedColumn = data;
}
```

Definition of the template in the html:

``` html
<ng-template #panelTemplate let-element>
  <div class="mat-row mat-row-detail" [@detailExpand] style="overflow: hidden">
    <av-table-detail-panel [data]="dataColumns" [item]="element" *ngIf="detailData">
    </av-table-detail-panel>
  </div>
</ng-template>
```

``` html
<ng-template #gridTemplate let-element>
  <div class="mat-row detail-table-container" [@detailExpand] style="overflow: hidden">
    <av-table-detail-table [data]="element[element.selectedColumn]"
                           [configuration]="getDetailConfiguration(element.selectedColumn)"
                           *ngIf="detailData">
    </av-table-detail-table>
  </div>
</ng-template>
```

 