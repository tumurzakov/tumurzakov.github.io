* [About Me](aboutme.html)

## Manticore search engine
2021-05-22

Recently I faced the need to organize a full text search on users table by a number of attributes such as address, phone, etc. First thing I looked at was Sphinx. I was surprised that such a project was literally dead. I don't want to use Elasticsearch due to complexity of this solution. By lucky Google picked up an article in chrome app about Manicore search engine. That was what I need. It is an actively developing clone of Sphinx. It is easy to use it thanks to MySQL protocol support. But SQL syntax differs from MySQL's a bit so I couldn't use the full power of the framework's database stack, only basic query functionality. If I had enough time or project were a little more important I possibly wrote a full datasource class. It will be cool to work with search indexes as with models. I made simple client class that could create index and add data to it. Enough for my project. 

## Uncle Bob taught me writing unit tests
2021-05-22

After I read Uncle Bob's book "Clean coder" I became a big fan of TDD. Book clarified to me the true purpose of writing tests. I made a few last projects using ping-pong technique and was very satisfied with that. Every time I feel lazy writing a test I hear Uncle Bob's claim "Who do you think you are?" and "Be professional!" and this is enough motivation for now.
