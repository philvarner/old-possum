# Scala

## Cheatsheets

* [Official](https://docs.scala-lang.org/cheatsheets/)
* [Java developer's Scala cheatsheet](https://mbonaci.github.io/scala/)
* [brenocon](http://brenocon.com/scalacheat/)
* [progfun-wiki](https://github.com/lampepfl/progfun-wiki/blob/gh-pages/CheatSheet.md)
* [Alvin Alexander](alvinalexander.com/downloads/scala/Scala-Cheat-Sheet-devdaily.pdf)

## Web

* [Awesome Scala](https://github.com/lauris/awesome-scala)

## Books

* [Functional Programming for Mortals with Cats](https://leanpub.com/fpmortals-cats/read#leanpub-auto-cats-typeclasses)

## Examples

### Java <-> Scala conversion

[Conversions Between Java and Scala Collections <= 2.12](https://docs.scala-lang.org/overviews/collections/conversions-between-java-and-scala-collections.html)
[Conversions Between Java and Scala Collections == 2.13](https://docs.scala-lang.org/overviews/collections-2.13/conversions-between-java-and-scala-collections.html)

```scala

// 2.12
import collection.JavaConverters._

// 2.13
import scala.jdk.CollectionConverters._

ArrayBuffer(1, 2, 3).asJava.asScala

```

### Tailrec Writer

```
import cats.data.Writer
import scala.annotation.tailrec

def countAndLog(n: Int): Writer[Vector[String], Int] = {
  @tailrec
  def f(in: Writer[List[String], Int]): Writer[List[String], Int] =
    if (in.value <= 0) in.mapWritten("starting!" :: _)
    else f(in.mapBoth((l, v) => (s"$v" :: l, v - 1)))

  f(Writer(Nil, n)).mapWritten(_.to(Vector))
}

println(countAndLog(10).run)
```

### Executor Service

```scala
implicit val ec = ExecutionContext.fromExecutor(
                    Executors.newFixedThreadPool(4))
```

### Scratch


list map {
  case Some(x) => x
  case None => default
}


Scala Array Extractor

a.foreach(e => {
   e match {
      case a: Array[Int] if a.last == 5 => 
      case _ =>
   }
})
You can do something a little better for matching on the first elements:
a.foreach(e => {
   e match {
      case Array(1, _*) => 
      case _ => 
   }
})
Unfortunately the @_* thing has to be the last item in the list of array arguments. But you can make the matching before that as complex as you want.
scala> val Array(1, x @_*) = Array(1,2,3,4,5)
x: Seq[Int] = Vector(2, 3, 4, 5)

scala> val Array(1, b, 3, x @_*) = Array(1,2,3,4,5)
b: Int = 2
x: Seq[Int] = Vector(4, 5)


## Futures

recover -> T
recoverWith -> F[T]

## Type Classes

statically-checked at compile time!

Type Enrichment

```scala
implicit class Foo(val t: T) extends AnyVal {
  def anEnrichingMethod(): U = ???
}
```

```scala
implicit def f(t: T): U = ???
```

implicit class FooOps[T](value: T) {
  def f(implicit actioner: Bar[T]): Baz = actioner.f(value)
} 