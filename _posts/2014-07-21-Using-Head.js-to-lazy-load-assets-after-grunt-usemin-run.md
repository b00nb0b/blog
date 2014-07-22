---
layout: post
title: Using Head.js to lazy load assets after grunt usemin run
comments: true
---

Recently i had to improve the loading behavior of a mobile optimized webpage and used the [PageSpeed Insights - Tool](https://developers.google.com/speed/pagespeed/insights/) from Google to analyze my page. It told me to remove css and javascript resources which blocks "above the fold" content from loading, which means that users with low bandwidth will see nothing except a blank page before the external resources finished loading. Now there are several ways to avoid this. 

##### 1. Putting the resource on the end of the `body` block:

```html
<body>
  <p>Some Content here</p>
  
  <script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
</body>
```

This approach works only with javascript tags, because according to the [w3c specifications](http://www.w3.org/TR/html5/document-metadata.html#the-link-element) `<link>` elements including `rel=""` attributes are restricted to the `<head>` element.


##### 2. Using the html5 `asyc` script attribute

```html
<body>
  <script async src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js" onload="jQueryReady()"></script>

  <p>Some more Content here</p>
</body>
```

This little addition to a otherwise 100% usual `<script>` element tells the browser to NOT block on loading this resource. Furthermore Safari has added a `defer` attribute which behaves exactly like `async` except one difference: `defer` guarantees that the execution order of the scripts is exactly like their occurrence order in the html. `async` doesn't to something like this. The scripts could be executed absolutely random.

##### 3. Using a script loader

The solutions mentioned above do not work for javascript web application written in [AngularJS](http://www.angularjs.org) or likewise frameworks. This frameworks usually need a specific execution order which can not achieved in all browsers with the simple methods. To get around this issue we can use a script loader. Just to name a few: [RequireJS](http://requirejs.org/), [HeadJS](http://headjs.com), [LABJS](http://labjs.com). I decided to use Head.js because its eligible whether for javascript or css and furthermore my assets are already compressed, uglified and concatenated by grunt and usemin.

-- in progress --