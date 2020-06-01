# Either Utils

When you working with Scala Either you really wish some things to be there by some utils lib (espesially, when you can't swith to next version of Scala because of some reasons)

Here is examples of functions:

```scala
  def flatEitherSeqOptionToEitherSeq[E, T](e: Either[E, Seq[Option[T]]]): Either[E, Seq[T]] = {
    e.flatMap(s => Right(s.flatten))
  }
```

```scala
  def mapOptionEitherOptionToEitherOption[E,T](e: Option[Either[E,Option[T]]]) : Either[E, Option[T]] =
    e.map {
      case Right(b) => Right(b)
      case Left(e) => Left(e)
    }.getOrElse(Right(None))
```

```scala
  def flatEitherSeqOptionToEitherSeq[E, T](e: Either[E, Seq[Option[T]]]): Either[E, Seq[T]] = {
    e.flatMap(s => Right(s.flatten))
  }
```

```scala
  def isDefined[E, T](opt: Either[E, Option[T]]): Boolean = {
      opt match {
        case Right(res) => res.isDefined
        case Left(_) => false
      }
  }
```