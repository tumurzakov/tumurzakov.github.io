I was faced with problem to make fast search over 300k table on multiple columns for thrird party integration. This is not so big volume for modern databases, especially if you have multiple replicas, but I didn't want even this small load on my core servers. I decided to index data for this problem in separate microservice. From previous projects I wanted to use Sphinx, but discovered that this project literally dead. Last commit was near a year ago. It is bad decision to use inactive projects in long perspective. Other option is Elasticsearch, very complex and heavy thing. Looks overkill for that small problem. Luckily Google picked up an article about [Manticore](https://docs.manticoresearch.com/latest/html/) search engine in Chrome app at phone. It is active clone of Sphinx with a number of functions that parent project should have but don't.

Requiremets:
1 Two different sources of table data
1 300k rows total with 6 columns each
1 Search over all columns simultanuously
1 Search over each column separately

Killing feature of Manticore product is that it supports mysql protocol that allow use it without any additional libraries in familar web framework. Just needs to be attached as new datasource. Unfortunetly it don't support all mysql dialect, so too smarty frameworks are fail trying index table structure with `SHOW FULL COLUMNS FROM table` command. Right way to implement full datasource interface for framework and use its full power, but I didn't have enough time for that and made simple client class that uses low level datasource features without trying to work with tables as models. Available only plain SQL functional.

After client implementation we need to create an index for data. It is creating by ordinary `CREATE TABLE` command:

```sql
CREATE TABLE table (
  key integer,
  value string attribute indexed,
  text text
)
```

There are several kinds of columns: attributes and full text search fields. Everything that not type `text` is an attribute. Attributes not participating in full text search if special keyword `indexed` not added but could be tested in conditional part of SQL query. For example if we searching somebody by name in known city, we could make query something like that:

```sql
SELECT * FROM table WHERE MATCH('name') AND city='City';
```

In first implementation I decided to meet forth requirement with REGEX function over indexed attributes but it was mistake because REGEX testing strings case sensitive and there is no way to change this behavior. Proper way is to [use](https://manual.manticoresearch.com/Searching/Full_text_matching/Operators#Full-text-operators) very flexible query syntax of MATCH operator. Now I'm searching by field separatly with query:

```sql
SELECT * FROM table WHERE MATCH('@value value');
```

I'm very glad to find such a good project as Manticore. Huge thanx for it to maintainers!
