---  
categories: article  
---  
Several things compare in Scala and Java

Case: you're creating table Person with a name and age in a database. Let's look at code!

### Case class

Java code:
```java
public class Person {  
  
 private final String name;  
 private final int age;  
  
 private final static int AGE_LOW_BOUNDER = 0;  
 private final static int AGE_HIGH_BOUNDER = 200;  
  
 public Person(String name, int age) {  
        this.name = name;  
        this.age = age;  
  }  
  
    public String getName() {  
        return name;  
  }  
  
    public int getAge() {  
        return age;  
  }  
  
    @Override  
  public boolean equals(Object o) {  
        if (this == o)  
            return true;  
 if (o == null || getClass() != o.getClass())  
            return false;  
  Person person = (Person) o;  
 return getAge() == person.getAge() &&  
            Objects.equals(getName(), person.getName());  
  }  
  
    @Override  
  public int hashCode() {  
        return Objects.hash(getName(), getAge());  
  }  
  
    @Override  
  public String toString() {  
        return "example.person.Person{" +  
            "name='" + name + '\'' +  
            ", age=" + age +  
            '}';  
  }  
}
```

Scala code
```scala
case class Person(name: String, age: Int)
```

Example of usage:

```scala
val p = Person("Peter", 5)  
val peter = Person("Peter", 5)  
val dom = Person("Dominique", 3)  
p == dom // false  
p == peter // true
```

Methods get/equals/hashCode/toString - all of them already implemented for you!

### Objects

Suppose, we want to restrict out Person by age.
Java code:
```java
public class Person {  
  
 private final String name;  
 private final int age;  
  
 private final static int AGE_LOW_BOUNDER = 0;  
 private final static int AGE_HIGH_BOUNDER = 200;  
  
 private Person(String name, int age) {  
        this.name = name;  
        this.age = age;  
  }  
  
    public static Person instanceOf(String name, int age)  
        throws IllegalAccessException {  
        if (age < AGE_LOW_BOUNDER || age > AGE_HIGH_BOUNDER)  
            throw new IllegalAccessException("Age is incorrect");  
        return new Person(name, age);  
  }
}
```
In Scala for this goal you can use an Object:
```scala
object Person {  
  private val AGE_LOW_BOUNDER = 0  
  private val AGE_HIGH_BOUNDER = 200  
  
  def apply(name: String, age: Int): Person = {  
    if (age < AGE_LOW_BOUNDER || age > AGE_HIGH_BOUNDER)  
      throw new IllegalArgumentException("Age is incorrect")  
    new Person (name, age)  
  }  
}
```
Now, you even don't need to say the word "apply" implicitly to invoke this method:
```scala
Person("Peter", 18)  
Person("Peter", -1) // throw an exception 
Person("Peter", 201) // throw an exception
```

### Traits
 
You can provide behavior there!

```scala
trait PersonRepository {  
  
  def logger: Logger  
  
  def save(person: Person)  
  
  def findAll: List[Person]  
  
  def findByName(name: String): List[Person] = {  
    findAll.filter(p => p.name == name)  
  }  
}
```
Implementation:

```scala
class InMemoryPersonRepository extends PersonRepository {  
  val list = ListBuffer[Person]()  
  
  override val logger = LoggerFactory.getLogger(classOf[InMemoryPersonRepository])  
  
  override def save(person: Person): Unit = {  
    list += person  
    logger.debug(s"Saved person with name ${person.name} and age ${person.age}")  
  }  
  
  override def findAll: List[Person] = list.toList  
}
```

4. Pattern matching

Code on Java:

```java
public void handle(Operation operation) {  
    switch (operation) {  
        case ADD:  
        case INSERT:  
        case UPDATE:  
            operations.add(operation);  
			break; 
		case DELETE:  
            operations.remove(operation);  
		    break;  
	}  
}
```
Error prone, and it's easy to forget about "break" statement!

Code in Scala:
```scala
def save(operation: Operation): Unit = {  
  operation match {  
    case ADD || INSERT || UPDATE =>  
      list += operation  
    case DELETE =>  
      list -= operation  
  }  
}
```

### for-yield 

When you need to create an object only if all options are present, you can use "for yield" statement:

```scala
def person(optName: Option[String],  
  optAge: Option[Int]) =  
  for {  
    name <- optName  
    age <- optAge  
  } yield Person(name, age)
```
Usage:
```scala
val optName: Option[String] = Some("Peter")  
val optAge: Option[Int] = Some(18)  
val optNone = None  
  
person(optName, optAge)  // Some(Person("Peter", 18))
person(optName, optNone)  // None
person(optNone, optAge)  // None
```

### Either

Evaluation with Either - the way to wrap up Exception or another result
```scala
def div(a: Int, b: Int): Int = {  
  if (b == 0) throw new ArithmeticException("Division by Zero")  
  a / b  
}  
  
val a = 1  
val b = 0  
  
def evaluate(a: Int, b: Int): Either[Exception, Int] = {  
  try {  
    Right(div(a, b))  
  } catch {  
    case e: ArithmeticException => Left(e)  
    case _ => Left(new UnsupportedOperationException)  
  }  
}  
  
val result = evaluate(a, b).getOrElse(0)
```

### String interpolation
Java code:
```java
public String toString() {  
    return "example.person.Person{" +  
        "name='" + name + '\'' +  
        ", age=" + age +  
        '}';  
}
```
you can inject the value of variable intro string with `$` symbol: 
```scala
return "example.person.Person{name=$name, age=$age}"
```


All examples could be find here: https://github.com/irenkamalova/comparison-scala-vs-java