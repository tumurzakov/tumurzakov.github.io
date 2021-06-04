I was faced with problem to make fast search over 300k table on multiple columns for thrird party integration. This is not so big volume for modern databases, especially if you have multiple replicas, but I didn't want even this small load on my core servers. I decided to index data for this problem in separate microservice. From previous projects I wanted to use Sphinx, but discovered that this project literally dead. Last commit was near a year ago. It is bad decision to use inactive projects in long perspective. Other option is Elasticsearch, very complex and heavy thing. Looks overkill for that small problem. Luckily Google picked up an article about [Manticore](https://docs.manticoresearch.com/latest/html/) search engine in Chrome app at phone. It is active clone of Sphinx with a number of functions that parent project should have but don't.

Requiremets:
* Two different sources of table data
* 300k rows total with 6 columns each
* Search over all columns simultanuously
* Search over each column separately

Killing feature of Manticore product is that it supports mysql protocol that allow use it without any additional libraries in familar web framework. Just needs to be attached as new datasource. Unfortunetly it don't support all mysql dialect, so too smarty frameworks are fail trying index table structure with `SHOW FULL COLUMNS FROM table` command. Right way to implement full datasource interface for framework and use its full power, but I didn't have enough time for that and made simple client class that uses low level datasource features without trying to work with tables as models. Available only plain SQL functional.

After client implementation we need to create an index for data. It is creating by ordinary `CREATE TABLE` command:

```sql
CREATE TABLE table (
  key integer,
  value string attribute indexed,
  text text
)
```

There are several kinds of columns: attributes and full text search fields. Everything that not type `text` is an attribute. Attributes not participating in full text search if special keyword `indexed` not added. 
