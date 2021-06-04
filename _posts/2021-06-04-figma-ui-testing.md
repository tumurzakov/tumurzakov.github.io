There was a task in backlog to update footer design of corporate site. Recently we found time for that. It was one of the simpliest things that could be done I thought. But this task took over a month of four people and that is why...

It is a shame how worklow was bad on this task. I delegated it to frontend developer. Requirements was in figma drawn by designer. Everytime when developer ends a piece of work he wrote me over whatsapp to ask stakeholder to check. I wrote a message to stakeholder and he sit down with designer in front of one monitor compare result on staging and wrote notices in figma. Then communication came back in reverse order for the next iteration. Disgusting!

Almost all problems was that some part of footer has offset with original place in required design. Everytime we were asked to "place icons on the same level as that inscription" or "space between that elements must be equal" and other same things. That pixel hunting exhausted all of us. Actual problem was in implicit requrements, absense of definition of done. I started to think how to automate this and come to this solution.

Designer working in Figma and composing screens that idealy fit as acceptance test. We could fetch them by id in PNG format, take screenshot of what we actualy have and compare them.

## Export from Figma

Figma has awesome [API](https://www.figma.com/developers/api) to export assets from drawings. We need draw screens as acceptance test, write down IDs of this blocks and fetch them over API using [Get Image endpoint](https://www.figma.com/developers/api#get-images-endpoint).

```

```
