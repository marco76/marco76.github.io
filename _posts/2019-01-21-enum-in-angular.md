---
title: 'Enums in Angular templates'
description: ''
date: 2019-01-21T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /enums-angular
categories:
  - Angular, TypeScript, JavaScript
tags:
  - Angular, TypeScript, JavaScript
 
image: '/assets/img/'

introduction: 'How to use enum in the html template'
---

The TypeScript `enum` can be used directly in your class, but it has to be assigned to a local variable to be used in the template.
 
Declaration example:

```typescript
import { Component } from '@angular/core';

enum LanguageType {Java = 1, 'JavaScript' = 2, "TypeScript" = 3}

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {
  languageTypeDeclaredInComponent = LanguageType;

constructor() {
  // this works
  console.log("Language Java", LanguageType.Java);
  // this works
  console.log("Language JavaScript", this.languageTypeDeclaredInComponent.JavaScript)
  }
}
```

The template can work only with your _local variable_ and not the enum self.
The template can access only objects exposed by the controller or the component. 

```html
<p>
OK, Enum for Java: <i>languageTypeDeclaredInComponent.Java</i> 
</p>
<p>
OK, Validity: <i>languageTypeDeclaredInComponent.Java === 1</i>
</p>
<p>
KO, This doesn't work : <i>LanguageType.Java</i>
</p>
```

If you use directly the enum in the template the browser will show the following exception:

`ERROR
Error: Cannot read property [...] of undefined`

## Transpilation

`enum` doesn't exist in JavaScript. During the transpilation it's converted in an array:

```typescript
enum ExampleType { Java = 1, 'JavaScript' = 2, TypeScript = 3 };
```

```javascript
var ExampleType;
(function (ExampleType) {
    ExampleType[ExampleType["Java"] = 1] = "Java";
    ExampleType[ExampleType["JavaScript"] = 2] = "JavaScript";
    ExampleType[ExampleType["TypeScript"] = 3] = "TypeScript";
})(ExampleType || (ExampleType = {}));
;
```

## Example

[https://angularenum.stackblitz.io](https://angularenum.stackblitz.io)

## References

[TypeScript reference](https://www.typescriptlang.org/docs/handbook/enums.html)

[Angular :  GitHub](https://github.com/angular/angular/issues/2885)

[enum in deep](https://basarat.gitbooks.io/typescript/docs/enums.html)

