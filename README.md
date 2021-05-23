* [About Me](aboutme.html)

## Manticore search engine
2021-05-23

I was faced with problem to make fast search over 300k table on multiple columns for thrird party integration. This is not so big volume for modern databases, especially if you have multiple replicas, but I didn't want even this small load on my core servers. I decided to index data for this problem in separate microservice. From previous projects I wanted to use Sphinx, but discovered that this project literally dead. Last commit was near a year ago. It is bad solution to use inactive projects in long perspective. Other option is Elasticsearch, very complex and heavy thing is overkill for that small problem. Luckily Google picked up an article about Manticore search engine in Chrome app at phone. It is active clone of Sphinx with a number of functions that parent project should have but don't.

Requiremets:
* Two different sources of table data
* 300k rows total with 6 columns each
* Search over all columns simultanuously
* Search over each column separately


