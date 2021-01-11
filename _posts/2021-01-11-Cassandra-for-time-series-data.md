As the volume of everyday data is growing,
 it is critical to find a database suitable for storing it.
  When we want to store a large amount of records about real-time transactions or events we need to consider solutions for time-series data. I have done some research and found out that there are some solutions designed to store time-series data: InfluxDB, TimescaleDB, Graphite… Well, here I would like to introduce Cassandra for time-series data.

There are a lot of rants about Cassandra and I’m not going to repeat all of them! I’d better answer the question: why is it useful for time series data as it wasn’t designed for this purpose? Is it an ideal solution for your business?

Let’s look at the key factors of Cassandra features:

1. High insert volume
2. High availability
3. Elastic and easy scalability
4. Integration with Spark
5. CQL language & Open Source

### High insert volume

Under the hood, Cassandra is using a log-structured merge-tree. This data structure allows you to access files with a high volume of inserts, such as transactional log data. Cassandra doesn’t allow you to read data before it is written to the disk. The storage engine groups inserts and updates in memory. Once written to disk, the data is immutable and is never overwritten. 



### High availability

Cassandra is using peer-to-peer architecture. It allows there to be no point of failure. All nodes in the cluster are equal. They use gossip protocol for interacting with each other. Sometimes you have clients from different countries  so downtime is not an option! For example, the MySQL server with master-slave architecture doesn't allow such a feature. It is a significant point to consider Cassandra! The other thing: you can distribute your data on two or more data-centers and separate data geographically for different regions. As a result, your clients can access data faster as their data is located closer to them!


### Elastic and easy scalability

One of the key features and the important argument to use Cassandra for time-series data. If you have seasoned peaks in your incoming data, it’s good to be able to scale up nodes during expected peaks and scale down after. As a result, you get more flexible clusters!

Integration with Spark

The most reasonable argument against using Cassandra for time-series data - it doesn’t have aggregators! But… It has something more interesting: integration with Spark provided “out-of-the-box”. You can connect to your Cassandra cluster with Spark Connector with only several lines of code! And then you can perform big-data analytics on top of your cluster!

CQL language & Open Source

And the last thing is the comfort of using Cassandra. It comes to you with CQL language - the language very similar to SQL language. If you’re good at SQL then you already know how to interact with Cassandra by queries. And both Cassandra and Spark are open source solutions, so you can pick it up for your enterprise project and give it to your client without requiring them to pay for the license! 

Sounds good, doesn’t it?

### Conclusion

I hope you’ll find your way to use this favorite database. Be aware of the pitfalls that I described here. Thank you for reading my note and don’t spare claps!
