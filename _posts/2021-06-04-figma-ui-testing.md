There was a task in backlog to update footer design of corporate site. Recently we found time for that. It was one of the simpliest things that could be done I thought. But this task took over a month of four people and that is why...

It is a shame how worklow was bad on this task. I delegated it to frontend developer. Requirements was in figma drawn by designer. Everytime when developer ends a piece of work he wrote me over whatsapp to ask stakeholder to check. I wrote a message to stakeholder and he sit down with designer in front of one monitor compare result on staging and wrote notices in figma. Then communication came back in reverse order for the next iteration. Disgusting!

Almost all problems was that some part of footer has offset with original place in required design. Everytime we were asked to "place icons on the same level as that inscription" or "space between that elements must be equal" and other same things. That pixel hunting exhausted all of us. Actual problem was in implicit requrements, absense of definition of done. I started to think how to automate this and come to this solution.

Designer working in Figma and composing screens that idealy fit as acceptance test. We could fetch them by id in PNG format, take screenshot of what we actualy have and compare them.

## Export from Figma

Figma has awesome [API](https://www.figma.com/developers/api) to export assets from drawings. We need draw screens as acceptance test, write down IDs of this blocks and fetch them over API using [Get Image endpoint](https://www.figma.com/developers/api#get-images-endpoint). IDs of the block is url encoded in url and could be  seen in address bar of browser when selecting block. That part `node-id` is that we need. `%3A` is url encoded `:` symbol.

```
https://www.figma.com/file/<drawing>?node-id=387%3A4882
```

## Taking screenshot and comparing with reference

There are a number of tools to compare screenshots but I was lucky to select [Backstop](https://github.com/garris/BackstopJS) that fit all my needs. Normaly it using for controlling regressions between versions of one screen. Backlog has command `backstop approve` that took recent screenshots as reference to compare. All we need is place our templates in backlog_reference dir in same format as references. Then start `backlog test` and get results.

## Results

I'm testing this tool now and it seems very promissing. Templating become very easy. Make minor change, run `backstop test`, watch difference. "Aha, one more pixel to the left here". Delightfully!

