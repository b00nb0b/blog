---
layout: post
title: Mobile page optimization
comments: true
---

In 2010, the internet research company Comscore already predicted that within 5 year the amount of mobile users will surpass the amount of desktop users. As you can see in figure 1, the intersection will happen somewhere in 2014 which is actually the date on which this blog post is written.


![Fig. 1: Number of global desktop and mobile users in millions]({{ site.baseurl }}assets/mobile_page_optimization/comscore_traffic_distribution.jpg)
<a name="fig1">Fig. 1</a>: Number of global desktop and mobile users in millions

Now we know the reason why its importent to optimze our webpages for mobile traffic. There are several factors which should be taken into account while optimizing for mobile (the list is not ordered in any kind):

- device display parameters
- available data network
- device performance

In the following part of this article I will explain some of this factors in detail.

#### 1. Device display parameters

Concerning the display, you should definitely consider the **display resolution** of the device. There is a wide spread of available resolutions and also a difference in devices pixel density. It's crucial to choose a proper font size. You should always try to use `px`, because it's related to the device reference pixel. If you use other units, the font size can differ from device to device. There are some easy rules:

- use `px` over other units to define font sizes (because it's related to the [reference pixel](http://www.w3.org/TR/CSS2/syndata.html#length-units))
- use a font size of `16px` for standard text
- for other text use only sizes relative to the standard text
- avoid the use of to many different font types

For further reading I recommend to take a look into the [google developer help pages](https://developers.google.com/speed/docs/insights/UseLegibleFontSizes).


Also try to keep an eye on the pixel denstity of the devices. Disrespecting this can lead to a cloudy appearance of images on your website. There is a feature `css`, called `device-pixel-ratio` which specifies how a css pixel relates to a device pixel (default is 1.0).  You can write specific code for each pixel density by using [media queries](http://www.w3.org/TR/css3-mediaqueries/):

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

According to [Nielsen 1993](http://www.nngroup.com/articles/response-times-3-important-limits/),  the average website user is willing to wait for an maximum amount of 1.0 seconds until he loses focus on the dialogue. So the goal has to be to serve the visible content within this time frame.
Especially in mobile networks this 1 second time frame often causes trouble because of high build-in network latency. [Table 1](#table1) shows the average network latency on mobile networks of AT&T (the largest U.S. mobile provider). This data represents the duration of one network round-trip time 'RTT' (client->server->client) which
for example on EDGE or GPRS takes more then the half of our one second budget.


 <a name="table1">Table 1</a>. AT&T latencies for deployed 2G–4G networks (Grigorik 2014)

|           |LTE	    |HSPA+	    |HSPA	    |EDGE	    |GPRS       |
|-----------|----------|-----------|-----------|-----------|-----------|
|Latency    |40–50 ms  |100–200 ms |150–400 ms |600–750 ms |600–750 ms |

To fully understand the consequences you need to know how a http(s) request exactly works. Usually the flow on mobile looks like this:

```
Request -> Control Plane -> DNS Lookup -> TCP Handshake
( -> TLS Handshake) -> Data
```

and takes the following timings into account:

1. Control Plane: depends on network (50 - 2500ms, one time cost)
2. DNS Lookup: 1 RTT
3. TCP Handshake 1 RTT
4. TLS Handshake (secure connections only): 1-2 RTT
5. Data 1-n RTT

Below the bottom line, even a relatively up-to-date connection (3G+) consumes at least the half of our budget to establish
only a simple http connection. Another limiting factor which comes additionally is the [tcp slow-start](http://en.wikipedia.org/wiki/Slow-start) strategy.
Simply explained, this feature causes a slow increase of the TCP congestion window which limits the amount of data which can be transferred in the early phase of the connection.
On most of the systems, the initial congestion window is set to 10 segments, which means a total data of about 14kb. That's about the theory behind connections and latency.

HTML is rendered sequentially, so if the browser receives adequate visible content it can be visible within the first data transmission round-trip. So our consequences are:

- provide visible content on the very upper part of your html document (the so called [above-the-fold](http://en.wikipedia.org/wiki/Above_the_fold) content)
- avoid blocking elements in the head of your document (no `<script>` or `<link rel="stylesheet">`)
- for single page application try to split your javascript code and put all essential code inline (which not blocks rendering)
- don't load assets from other domains (which would cause another dns lookup, handshake.. and so on)
- use proper browser caching (cache times about 2 or more weeks), if necessary use a [cache-buster](https://github.com/cbas/grunt-rev) to force re-request of assets
- use gzip compression (don't forget about webfonts)
- if you can, use [SPDY](http://www.chromium.org/spdy/spdy-whitepaper). It will give you a performance increase of about 30%

#### 3. Device performance

Mind the critical rendering path. In [Figure 2](#fig2), you can see how the browser renders a HTML document. HTML is parsed into the DOM (Document Object Model) while CSS becomes
the CSSOM (Css Object Model). Both Models put together end up in the RenderTree, which can be layouted and finally paint by the browser. (For now, this information should be enough.
If you want to know it more detailed, then I encourage you to read this [document published by Google](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/).)

![Fig. 2: The critical rendering path]({{ site.baseurl }}assets/mobile_page_optimization/critical_rendering_path.png)
<a name="fig2">Fig. 2</a>: The critical rendering path ([Grigorik 2012](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/))

This process takes time, which can vary between different devices. Furthermore both the DOM and CSSOM can manipulated by javascript. And guess what happens if one of the models is changed --
we build the RenderTree, Layout and Paint... (and the user has to wait).

Another problem is the around 300 ms click delay on touch devices. The smartphone doesn't know if you want to perform a tap or a double-tap, so it waits around 300 ms to decide if it recognizes whether the first or the second one.

- minimize the amount of DOM/CSSSOM manipulations
- remove unnecessary HTML/CSS, so the browser doesn't need to put it into the DOM/CSSOM
- provide the browser with width and height information in `<img>` tags, so the browser can already calculate the exact size of the element in the RenderTree (otherwise a recalculation is necessary when the image is loaded)
- use hardware acceleration (rendering on the GPU) with a CSS3 transformation `-webkit-transform: translateZ(0);` for example
- use something like Google's FastButtons to avoid the 300 ms touch delay ([Google Article](https://developers.google.com/mobile/articles/fast_buttons) / [GitHub Repo](https://github.com/alexblack/google-fastbutton))

#### Conclusion

If you want to optimize your website for mobile devices, try to provide the user with useful data under 1000 ms. Its most likely that you aren't able to fully load and render the whole website within this time frame, but you can provide some useful information like a loading indicator or maybe some basic text. Use a proper font size to create a good user experience on your site. Use HTML5/CSS3 features, because almost all modern (mobile) web browsers supports it. Double check your static file cache headers and check for enabled gzip compression. If necessary ask your DevOps team to set it up properly.

##### Literature

[Nielsen 1993] J. Nielson, *Response Times: The 3 Important Limits*, 1993
[Grigorik 2014] I. Grigorik, *High Performance Browser Networking*, O'Reilly, 2014.
