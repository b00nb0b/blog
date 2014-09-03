---
layout: post
title: Speed up your webfonts by using the woff2 format
comments: true
---

Today, I stumbled on the new webfont format `woff2`, which is natively supported by Chrome 36 and above. The file format
 delivers file size savings between 30 and 50% in comparison to `woff`. This saving can directly result in a better user experience
 on your site and can be achieved with a single additional step in your build process.

#### Get in touch

To convert your webfont files from `ttf` to `woff2`, you can easily use the reference implementation code of google. You can
can get it from [Google Code](https://code.google.com/p/font-compression-reference/).

To get started just open a terminal and type:

```bash
git clone https://code.google.com/p/font-compression-reference/
cd font-compression-reference/woff2
make clean all

./woff2_compress somefont.ttf
```

### Usage in CSS

A simple usage example for [FontAwesome](http://fontawesome.io) could look like this:

```css
@font-face {
  font-family: 'FontAwesome';
  src: url('../fonts/fontawesome-webfont.eot?v=4.2.0');
  src:  url('../fonts/fontawesome-webfont.eot?#iefix&v=4.2.0') format('embedded-opentype'),
        url('../fonts/fontawesome-webfont.woff2?v=4.2.0') format('woff2'),
        url('../fonts/fontawesome-webfont.woff?v=4.2.0') format('woff'),
        url('../fonts/fontawesome-webfont.ttf?v=4.2.0') format('truetype'),
        url('../fonts/fontawesome-webfont.svg?v=4.2.0#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
```

### MIME Type

Configure your webserver to serve the file with the proper mime type. The official recommendation of the W3C is `application/font-woff2`.
Until now Crome 37 doesn't recognize this mime type as a proper font file but this issue is already fixed by the dev team and will
disappear in future releases of the browser (Chrome Canary 39 already includes the fix).

#### Browsersupport

- Chrome 36
- Opera 23

#### Useful Links

- [W3C: WOFF File Format 2.0](http://www.w3.org/TR/WOFF2/)
- [Gist of sergejmueller about woff2](https://gist.github.com/sergejmueller/cf6b4f2133bcb3e2f64a)
- [I. Grigorik: Talk about "Making webfonts fast(er)"](https://docs.google.com/presentation/d/10QJ_GABjwzfwUb5DZ3DULdv82k74QdPArkovYJZ-glc/present#slide=id.p19)
