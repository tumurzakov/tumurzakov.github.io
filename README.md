* [About Me](aboutme.html)

## Manticore search engine
2021-05-22

Recently I faced the need to organize a full text search on users table by a number of attributes such as address, phone, etc. First thing I looked at was Sphinx. I was surprised that such a project was literally dead. I don't want to use Elasticsearch due to complexity of this solution. By lucky Google picked up an article in chrome app about [Manticore](https://manticoresearch.com/) search engine. That was what I need. It is an actively developing clone of Sphinx. Thanks to MySQL protocol support it is easy to use especialy in web projects. But SQL syntax differs from MySQL's a bit so I couldn't use the full power of the framework's database stack, only basic query functionality. If I had enough time or project were a little bit more important I possibly wrote a full DataSource class. It will be cool to work with search indexes as with models. Even attach them to ordinary models for automatic index. I made simple client class that could create index and add data to it. Enough for my project.

## Uncle Bob taught me writing unit tests
2021-05-22

After I read Uncle Bob's book "Clean coder" I became a big fan of TDD. Book clarified to me the true purpose of writing tests. I made a few last projects using described ping-pong technique and was very satisfied with that. The key of that technique is switching every 30s between test and production code. Write testing code line, write coresponding production code, test and again. It require some discipline to follow TDD. Every time I feel lazy writing a test I hear Uncle Bob's claim "Who do you think you are?" and this is enough motivation for me now. Also I liked his statement that for being professional you must spend 20 hours a week on self improvement. You can not to spend this time but then don't call yourself a "Professional".

Shame on me! While writing Manticore client class i didn't used TDD practice. I actualy learn protocol while writing this class so I didn't know what result I want to get. So I didn't know what to test. After I got this work done result satisfied me and this class gone to production. If Uncle Bob read this text he will say that I created a mess. Proper strategy for this kind of situations could be first learn protocol, think about it and then you will know what want to get.
