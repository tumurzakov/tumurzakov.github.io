---
title: Figma UI testing
---

There was a task in the backlog to update the footer design of the corporate site. Recently we found time for that. It was one of the simplest things that could be done I thought. But this task took over a month for four people and that is why...

It is a shame how workflow was bad on this task. I delegated it to the front-end developer. Requirements were in figma drawn by the designer. Everytime when developer ends a piece of work he wrote me over whatsapp to ask the stakeholder to check. I wrote a message to the stakeholder and he sat down with the designer in front of one monitor to compare results on staging and wrote notices in figma. Then communication came back in reverse order for the next iteration. Disgusting!

Almost all problems were that some part of the footer was offset with the original place in required design. Everytime we were asked to "place icons on the same level as that inscription" or "space between those elements must be equal" and other similar things. That pixel hunting exhausted all of us. Actual problem was in implicit requirements, absence of definition of done. I started to think about how to automate this and come to this solution.

Designer working in Figma and composing screens that ideally fit as an acceptance test. We could fetch them by id in PNG format, take screenshots of what we actually have and compare them.

## Export from Figma

Figma has an awesome [API](https://www.figma.com/developers/api) to export assets from drawings. We need to draw screens as acceptance tests, write down IDs of these blocks and fetch them over API using [Get Image endpoint](https://www.figma.com/developers/api#get-images-endpoint). IDs of the block are encoded in url and could be  seen in the address bar of the browser when selecting the block. That part `node-id` is what we need. `%3A` is the url encoded `:` symbol.


```
https://www.figma.com/file/<drawing>?node-id=387%3A4882
```

## Taking screenshot and comparing with reference

There are a number of tools to compare screenshots but I was lucky to select [Backstop](https://github.com/garris/BackstopJS) that fit all my needs. Normally it is used for controlling regressions between versions of one screen. Backlog has the command `backstop approve` that took recent screenshots as reference to compare. All we need is to place our templates in the backlog_reference dir in the same format as references. Then start the `backlog test` and get results.

## Results

I'm testing this tool now and it seems very promising. Templating becomes very easy. Make minor changes, run `backstop test`, watch the difference. "Aha, one more pixel to the left here". Delightfully!

