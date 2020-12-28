---
categories: article
---

Prevent yourself from the pitfalls of using Cassandra!
There are a lot of articles that scream about the advantages of Cassandra, and I really love this database. As I would like you to love it too. For this, you should know about all the pitfalls you can meet on your journey! I aggregated the main points here. Let's quickly look at the list of features don't exist in Cassandra and probably you will miss them:
Cassandra = No Joins
* No subqueries for selecting data
* No commit/rollback features
* Insert with "if exist" condition is extremely expensive
* Update with "if" condition is extremely expensive
* No queries "like" expression for selecting data
* Adding new columns is not possible in some cases

Let's look at every point in detail.
Cassandra = No Joins
```
SELECT t1.name, t2.salary
FROM employee AS t1 
INNER JOIN info AS t2 
ON t1.name = t2.name;
```
You have "no" to all relations in your data model. You have to rethink your model for Cassandra. To perform joins you need to check every node for consistency. It means losing the performance and scalability that NoSQL databases offer. De-normalize your tables. Don't worry if they will have too many columns - Cassandra deals with it very well.
Also, you can perform joins by using the open-source framework Apache Spark on top of your Cassandra database.
No subqueries for selecting data
```
SELECT * FROM table_1
WHERE col_1 IN (SELECT col_1 FROM table_2 WHERE col_2="value")
```

Cassandra doesn't support such a query. To perform such an operation on the cluster you need to go to every node. It means performance degradation and disability for scalability. So, this feature doesn't make sense for Cassandra.
No transactions with commit/rollback features
```
START TRANSACTION;
SELECT @A:=SUM(salary) FROM table1 WHERE type=1;
UPDATE table2 SET summary=@A WHERE type=1;
COMMIT;
```

In relational databases, you have a powerful concept of transactions. In the example above, you can lock "table1" for writes and guarantee that records will be saved in "table2" without any changes to "table1". If you don't want to lock "table1", you can check if the sum is the same as it selected after the update statement and commit transaction in this case, and if not the same - rollback transaction. It allows you to avoid race conditions on the table.
So, Cassandra doesn't implement the same functionality at all. Ideally, you have to forget about these transactions. You have CAS (compare-and-set) features in Cassandra to perform some checks, but they are too expensive! Ironically, they are called Light Weight Transactions (LWT) though there's nothing about "light weight"! You should avoid using them because they are implemented via the Paxos protocol under the hood of Cassandra. The protocol requires 4 round trips across the cluster. Too expensive!
Let's look in detail at it in the next two points.
Using LWT - really risky! 

Insert with "if exist" condition is extremely expensive
```
INSERT INTO table_1 (col_1)
VALUES ("value")
IF NOT EXISTS;
```
Let's look at the possibility add new data only if they don't exist in Cassandra. Be very careful here: you need to remember that this operation requires checking all nodes in the cluster. Under the hood, Cassandra uses Paxos protocol to implement this feature. That comes with the extreme degradation of the performance. Rethink your model to avoid using this feature.
Update with "if" condition is extremely expensive
```
UPDATE table_1 
SET col_1 = "value"
WHERE id = 1 
IF col_2 = "condition";
```
The reason for not supporting this operation is the same as for the "if not exist": to perform it, you need to check data on all nodes across the cluster. That again will use the Paxos protocol. For this case, you need to rethink your model to avoid such queries.
No queries "like" expression for selecting data
```
SELECT * FROM table_1 WHERE col_1 like 'b%';
```
To query data in Cassandra you should follow the restriction: fields in the "where" condition should be part of the partition key. Because to find the data, firstly, there should be decided which node stores these data. That's why you can't specify regular expressions in your query. If you need to perform as "like 'b%'…" on some data you need to put it in another place like a search index. Consider using Lucene Index and frameworks above it - SolrCloud or ElasticSearch.
Adding new columns is not possible in some cases
Cassandra is a column-oriented database. This concept lets you add a new column to your table without data migration. But not for all cases… Which cases are? Well, if your primary key incorrectly is designed and you have to update it by adding a new column to the table or even only add a new column to this primary key - you won't be able to do it. You need to delete an old table and create a new one with the new primary key. If it requires to migrate for terabytes of data - that could be impossible for your business! To avoid this, you need to try modeling your schema for all possible business cases before releasing it to the production!
Conclusion
Every Cassandra node like a cup of coffee. Cassandra is powerful! I recommend considering using it in your project if you have suitable business cases! But be ready to analyze all possible business scenarios and predict all possible changes in your data model so you can apply them. Take care of the cluster and be kind to your Cassandra nodes!
Don't spare on claps!