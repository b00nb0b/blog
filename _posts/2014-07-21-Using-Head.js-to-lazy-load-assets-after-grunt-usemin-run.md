Recently i had to improve the loading behavior of a mobile optimized webpage and used the [PageSpeed Insights - Tool](https://developers.google.com/speed/pagespeed/insights/) from Google to analyze my page. It told me to remove css and javascript resources which blocks "above the fold" content from loading, which means that users with low bandwith will see nothing except a blank page before the external resources finished loading. Now there are several ways to avoid this. 

##### 1. Putting the resource on the end of the `body` block:

```html
<body>
  <p>Some Content here</p>
  
  <script>http://my/external/script/here</script>
</body>
```

This approach works only with javascript tags, because according to the [w3c specifications](http://www.w3.org/TR/html5/document-metadata.html#the-link-element) `<link>` elements including `rel=""` attributes are restricted to the `<head>` element.


##### 2. Using the html5 `asyc` script attribute

```html
<body>
  <script async>http://my/external/script/here</script>

  <p>Some more Content here</p>
</body>
```

In progress
