---
title: I didn't know that using SVG sprites could be such a headache!
---

Earlier we used an image sprites to optimize network load of the sites. Now comes an era of vector graphic and particular SVG. I tried to use patterns that was familar to me from previous live and I could not. Thats why...

# Sprite creation

First I asked designer to make me a sprite of all svg icons. He gave me it and I could not fetch nothing from it because it is not raster image! I don't know positions of icons by pixels. Ok! Trying to read how to deal with svg sprites.

* [How SVG Fragment Identifiers Work](https://css-tricks.com/svg-fragment-identifiers-work/)
* [SVG sprite as background](https://css-tricks.com/forums/topic/svg-sprite-symbols-as-css-content-or-background/)
* [Icon System with SVG Sprites](https://css-tricks.com/svg-sprites-use-better-icon-fonts/)

Ok. Tried to change designer's sprite to match structure that was described in articles below. Feel that it wrong way, especialy that such operations could not be done from Adobe Illustrator where designer work. Another way is to create sprite myself from a bunch of files.

# Sprite generation

Figma has nice feature to export assets as archive if you select them. Designer did huge work on creating of brand book and icons are in it. Selected and exported them in one action i placed icons to appropriate element folders. Now is a time to teach webpack fetch them and make sprite.

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

Ok, now `sprite.svg` in public dir. How to attach it to page? Only option is to include it via AJAX after page load. Other tries to attach it inject via Twig or webpack are failed.

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

Acceptable. But I'm used to put images into `background-image` css style. Following efforts will show if I can work out all problems with this scheme.
