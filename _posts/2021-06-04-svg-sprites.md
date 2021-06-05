---
title: I didn't know that using SVG sprites could be such a headache!
tags: svg sprite webpack headache css
---

Earlier we used image sprites to optimize network load of the sites. Now comes an era of vector graphics and particular SVG. I tried to use patterns that were familiar to me from previous lives and I could not. That's why...

# Sprite creation

First I asked the designer to make me a sprite of all svg icons. He gave me it and I could not fetch anything from it because it is not a raster image! I don't know the positions of icons by pixels. Ok! Trying to read how to deal with svg sprites.

* [How SVG Fragment Identifiers Work](https://css-tricks.com/svg-fragment-identifiers-work/)
* [SVG sprite as background](https://css-tricks.com/forums/topic/svg-sprite-symbols-as-css-content-or-background/)
* [Icon System with SVG Sprites](https://css-tricks.com/svg-sprites-use-better-icon-fonts/)

Ok. Tried to change designer's sprite to match structure that was described in articles below. Feel that it is wrong, especially that such operations could not be done from Adobe Illustrator where designers work. Another way is to create sprite myself from a bunch of files.

# Sprite generation

Figma has a nice feature to export assets as archive if you select them. Designer did huge work on creating brand book and icons are in it. Selected and exported them in one action i placed icons to appropriate element folders. Now is a time to teach webpack fetch them and make sprite.

```js

const SpritePlugin = require('svg-sprite-loader/plugin');

Encore
    /* ... */
    
    // svg-sprite-loader
    .addLoader({
        test: /\.svg$/,
        use: [
            {
                loader: 'svg-sprite-loader' ,
                options: {
                    extract: true
                }
            }
        ]
    })

    .addPlugin(new SpritePlugin())
;
```

Ok, now `sprite.svg` is in the public dir. How to attach it to a page? Only option is to include it via AJAX after page load. Other attempts to attach it via Twig or webpack have failed.

```js
import domready from 'domready';

domready(() => {
    fetch('build/sprite.svg')
        .then((resp) => {
            return resp.text();
        })  
        .then((data) => {
            document.getElementById('svg').innerHTML = data;
        });
});
```

Now to use elements from sprite I need to write construction as:

```html
  <svg class="class">
    <use xlink:href="#filename"></use>
  </svg>
```

Acceptable. But I'm used to putting images into `background-image` css style. Following efforts will show if I can work out all problems with this scheme.
