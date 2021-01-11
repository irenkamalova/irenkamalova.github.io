---  
categories: article  
---  
### Only 3 design components can solve your issue!

Didn’t you think about how many challenges in software engineering could be solved by using only **three design components** in your system?

→ Just imagine: you have an online shop where you receive updates in real-time about goods, you need to handle these events, store information about goods in the database, and make this information searchable.

→ Or suppose that you support a payment system and have real-time events that you need to store, and then you want to be able to search through them.

→ Or maybe you have an ads platform, you have to handle them in real-time, validate them, and make them searchable for users.

All these cases can be solved by three powerful enterprise frameworks: _message queues_, _database,_ and _search index_. In the center, you have the service which processes data and connects these parts together.

![](https://cdn-images-1.medium.com/max/1000/0*57b491Xp6DmN46gd)

Let’s look at these components in detail.

**Message Queue**

Publishing events to the message queue allows you to go to [event sourcing](https://en.wikipedia.org/wiki/Event-driven_architecture), the architecture of your system which lets you subscribe to different topics with messages. There’re a lot of ready solutions in the market to pick up and use inside your system: Kafka/RabbitMQ/ZeroMQ… For example, Kafka gives you resilient storage for your events where you can manage and guarantee how long all events can exist in your system. In the meantime, partitioning helps you consume messages with several readers and you can increase your performance by several times!

**NoSQL Database**

For tests or sales-demonstrations, it’s possible to use in-memory databases here — maps, caches, or frameworks like [h2DB](https://en.wikipedia.org/wiki/H2_%28DBMS%29). But when by the time for an application is ready for production, all your data should be in the resilient storage. Here you can come to lots of ready for production open-source products. The question is about which solution you want to pick up: classic relational database or modern NoSQL? The second gives you scalability for free, that’s why I would recommend seriously consider them at the start of design your system and check how to build the correct model which let you don’t use joins between tables and make your data always available:

![](https://cdn-images-1.medium.com/max/1000/0*dcF1fCUCag28cEEl)

**Search Index**

When you start to go with the approach of using NoSQL databases, you’re using not only the possibility to join tables, but also the possibility of flexible querying: no more complex conditions in the “where” clause, no more queries with regular expression “LIKE ‘%…’”! However, you have one more ready to production open-source solution for storing data: search index. With the power of search indexes, you can scan your documents by any indexed field, making regular expressions on these fields. One more feature is that the value of the field can be huge as Tolstoy novel, and still be searchable and available for regular expressions applied on this field.

The most popular open**-**source solutions are Solr Cloud and ElasticSearch, both built on the Lucene Index engine. Solr Cloud exists on the market already for several years, is fully supported by the community, and has grown accreted with new features every year. ElasticSearch is a younger project, but already has a lot of fun and you probably met it by using ELK stack for your logs.

**Conclusion**

Of course, nothing comes for free in the world: for using these components, you need to build a strong environment, provide servers for monitoring tools, and tune performance, while looking at this monitoring. But the good news: this framework is so popular, so a lot of solutions have already been implemented! For example, you can find out that they have ready dashboards for the visualization tool. There’s a lot of documentation and tips for building infrastructure. So, don’t be afraid to start with experimenting with this design for your system!