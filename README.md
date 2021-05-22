# [About Me](aboutme.html)

## [2021-05-22] Manticore search engine

Recently I faced the need to organize a full text search on users table by a number of attributes such as address, phone, etc. First thing I looked at was Sphinx. I was surprised that such a project was literally dead. I don't want to use Elasticsearch due to complexity of this solution. By lucky Google picked up an article in chrome app about Manicore search engine. That was what I need. It is an actively developing clone of Sphinx. It is easy to use it thanks to MySQL protocol support. But SQL syntax differs from MySQL's a bit so I couldn't use the full power of the framework's database stack, only basic query functionality. If I had enough time or project were a little more important I possibly wrote a full datasource class. It will be cool to work with search indexes as with models.
