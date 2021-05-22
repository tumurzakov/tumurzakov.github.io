* [About Me](aboutme.html)

## Manticore search engine
2021-05-22

Recently I faced the need to organize a full text search on users table by a number of attributes such as address, phone, etc. First thing I looked at was Sphinx. I was surprised that such a project was literally dead. I don't want to use Elasticsearch due to complexity of this solution. By lucky Google picked up an article in chrome app about [Manticore](https://manticoresearch.com/) search engine. That was what I need. It is an actively developing clone of Sphinx. Thanks to MySQL protocol support it is easy to use especialy in web projects. But SQL syntax differs from MySQL's a bit so I couldn't use the full power of the framework's database stack, only basic query functionality. If I had enough time or project were a little bit more important I possibly wrote a full DataSource class. It will be cool to work with search indexes as with models. Even attach them to ordinary models for automatic index. I made simple client class that could create index and add data to it. Enough for my project.

## Uncle Bob taught me writing unit tests
2021-05-22

After I read Uncle Bob's book "Clean coder" I became a big fan of TDD. Book clarified to me the true purpose of writing tests. I made a few last projects using described ping-pong technique and was very satisfied with that. The key of that technique is switching every 30s between test and production code. Write testing code line, write coresponding production code, test and again. It require some discipline to follow TDD. Every time I feel lazy writing a test I hear Uncle Bob's claim "Who do you think you are?" and this is enough motivation for me now. Also I liked his statement that for being professional you must spend 20 hours a week on self improvement. You can not to spend this time but then don't call yourself a "Professional".

While writing the Manticore client class I didn't use TDD practice. I actually learned protocol while writing this class and didn't know what result I wanted to get. So I didn't know what to test. Result of this prototyping satisfied me enough to commit it to production. Of Course I need to write a test for it, otherwise it would be technical debt. Such situations decrease code coverage and could be seen on metrics if you collect them. Other option is to throw away all client code and rewrite it in TDD maner. Which is better in this situation? First option is ok, i think. Any objetions?
