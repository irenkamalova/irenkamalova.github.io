---
categories: techNotes
---
Adding logger on top of main functionality

I inspired that Scala with it complex concepts of traits let you play with extending your services

Here is example how you can add logging to your service without changing anything inside service:

Suppose you have a service
```scala
trait Service {
  def add(a: Int, b: Int): Int
}
```

and simple implementation:
```scala
class SimpleService(name: String) extends Service {
  def add(a: Int, b: Int): Int = {
    a + b
  }
}
```

Now, you want to add with logger to service implementation:
```scala
trait StdoutLogger {
  def log(msg: String) = println(msg)
}
```
Of course, you could add it easy by extending this trait in your SimpleService class:
```scala
class SimpleService(name: String) extends Service with StdoutLogger {
  def add(a: Int, b: Int): Int = {
    val result = a + b
    log(s"Result of computation is $result")
    result
  }
}
```

But here, Scala let you bring more power and some tricks:
```scala
trait LoggedService extends SimpleService with StdoutLogger {
  override def add(a: Int, b: Int): Int = {
    val result = super.add(a, b)
    log(s"Result of computation is $result")
    result
  }
}
```
And you just implement this interface when you create a new entity of SimpleService
```scala
object Example {
  def main(args: Array[String]): Unit = {
    val service = new SimpleService("service") with LoggedService
    service.add(1, 2)
  }
}
```
Volia!

(This is trivial example that can help deal with complex dependencies and swap implementations safely without lots refactoring)