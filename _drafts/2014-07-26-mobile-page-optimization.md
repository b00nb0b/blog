---
layout: post
title: Mobile page optimization
comments: true
---

In 2010, the internet research company Comscore already predicted that within 5 year the amount of mobile users will surpass the amount of desktop users. As you can see in figure 1, the intersection will happen somewhere in 2014 which is actually the date on which this blog post is written.


![Fig. 1: Number of global desktop and mobile users in millions]({{ site.baseurl }}public/images/mobile_page_optimization/comscore_traffic_distribution.jpg =100%)

Now we know the reason why its importent to optimze our webpages for mobile traffic. There are several factors which should be taken into account while optimizing for mobile (the list is not ordered in any kind):

- device display parameters
- available data network
- device performance

In the following part of this article I will explain some of this factors in detail.

#### 1. Device display parameters

Concerning the display, you should definitely consider the **display resolution** of the device. There is a wide spread of available resolutions and also a difference in devices pixel density. It's crucial to choose a proper font size. There are some easy rules:

- use `px` over other metrics to define font sizes (because it's related to the [reference pixel](http://www.w3.org/TR/CSS2/syndata.html#length-units))
- use a font size of `16px` for standard text
- for other text use only sizes relative to the standard text
- avoid the use of to many different font types


Also try to keep an eye on the pixel denstity of the devices. Disrespecting this can lead to a cloudy appearance of images on your website. In `css` you can write specific code for each pixel density by using `media queries`:

If you can use [lesscss](http://lesscss.org), the following piece of code will give you a highdensity media query:

```less
@highdensity: ~"only screen and (-webkit-min-device-pixel-ratio: 1.5)",
              ~"only screen and (min--moz-device-pixel-ratio: 1.5)",
              ~"only screen and (-o-min-device-pixel-ratio: 3/2)",
              ~"only screen and (min-device-pixel-ratio: 1.5)";
```

You can now use it with:

```css
.logo {
    background: url(/static/images/logo.png) no-repeat 0 0;
 
    @media @highdensity {
        background-image: url(/static/images/logo@2x.png);
        background-size: contain;
    }
}
```

The example above (taken from [here](http://blog.scur.pl/2012/06/variable-media-queries-less-css/)) sets a double sized background image to the `logo` for high density displays because otherwise, the graphic would appear cloudy. 


#### 2. Available data network

- 1 second concentration window
- latency (dns, tcp, tls, data)
- bandwidth
- blocking elements
- above-the-fold

#### 3. Device performance

- brower render time
