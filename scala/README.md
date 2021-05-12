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

## Libraries

https://github.com/estatico/scala-newtype
https://github.com/gnieh/diffson


//        "io.estatico" %% "newtype" % "0.4.4",
//        "eu.timepit" %% "refined" % "0.9.20"

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

## Self Type

disambiguates what `this` means when functions are defined in a class. You can always refer to `self` (or whatever you call it, it's arbitrary) when talking about the enclosing class

```
trait Foo {
  self =>
}
```

## Extractor Objects

In many cases, this is better than using a raw case class. [Extractor Objects docs](https://docs.scala-lang.org/tour/extractor-objects.html)

### Scratch

scala parallel test execution

testOptions in Test += Tests.Argument("-P")

ParallelTestExecution

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

## Integration Tests within a Docker container

```
import scala.sys.process._
addCommandAlias("it", "runIntegrationTestsInDocker")
lazy val runIntegrationTestsInDocker = taskKey[Unit]("Runs Integration Tests within Docker container")
runIntegrationTestsInDocker := {
  s"""
     |docker run -i
     | --mount type=bind,src=${sys.props("user.dir")},dst=/w
     | --mount type=bind,src=${sys.env("HOME")}/.ivy2,dst=/root/.ivy2
     | --mount type=bind,src=${sys.env("HOME")}/Library/Caches/Coursier,dst=/root/.cache/coursier
     | -w /w
     | -e AWS_SECRET_ACCESS_KEY=${sys.env("AWS_SECRET_ACCESS_KEY")}
     | -e AWS_ACCESS_KEY_ID=${sys.env("AWS_ACCESS_KEY_ID")}
     | s22s/rasterframes-circleci
     | sbt
     | it:test
  """.stripMargin.split("\n").mkString.!
}
```


Metals

metals.showInferredType


    docker run --rm -p 22909:22909 s22s/eod-catalog-svc:latest

    dependencyCheckSkipProvidedScope := true,
    dependencyCheckFailBuildOnCVSS := 0,
    dependencyCheckSuppressionFiles := Seq(new File("dependency-check-suppressions.xml"))



class FooSpec
    extends AnyFlatSpec with Matchers with ScalaFutures with Inspectors with OptionValues


it should "do something" in {

it should "do something" ignore {


// Current test -- taggedAs Current
package foo

import org.scalatest.Tag

object Current extends Tag("foo.Current")


  type Vare[A] = ValidatedNec[ItemValidation, A] // Vare == Validation Result




  def time[R](tag: String)(block: => R)(implicit logger: Logger): R = {
    val start = System.currentTimeMillis()
    val result = block
    val end = System.currentTimeMillis()
    logger.info(f"Elapsed time of $tag - ${(end - start) * 1e-3}%.4fs")
    result
  }


logback.xml

<configuration>

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <!-- encoders are assigned the type
             ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="debug">
        <appender-ref ref="STDOUT"/>
    </root>

    <logger name="com.sksamuel.elastic4s" level="error" additivity="false">
        <appender-ref ref="consoleAppender"/>
    </logger>
    <logger name="software.amazon.awssdk.auth.credentials" level="OFF"/>

</configuration>


  /** Anything that structurally has a close method. */
  type CloseLike = { def close(): Unit }

  /** Applies the given thunk to the closable resource. */
  def using[T <: CloseLike, R](t: T)(thunk: T => R): R = {
    try {
      thunk(t)
    } finally {
      t.close()
    }
  }



  val NoStore: `Cache-Control` = `Cache-Control`(`no-cache`, `no-store`, `must-revalidate`)
  val NoCache: `Cache-Control` = `Cache-Control`(`no-cache`)
  val MaxAgeTenMin: `Cache-Control` = `Cache-Control`(`max-age`(600), `must-revalidate`)


import akka.http.scaladsl.model.StatusCode

/**
 * We want to be able to fail a future with `ErrorResult`
 * but since it doesn't extend `Throwable` we need to wrap it into one.
 */
case class Error(
  code: StatusCode,
  title: Option[String] = None,
  detail: Option[String] = None
) extends Throwable(title.orNull)



  private[errors] class ErrorFactory(code: StatusCode) {

    def apply(): Error =
      Error(code)

    def apply(title: String): Error =
      Error(code, Some(title))

    def apply(title: String, detail: String): Error =
      Error(code, Some(title), Some(detail))
  }

  object NotFound extends ErrorFactory(StatusCodes.NotFound)

  object Forbidden extends ErrorFactory(StatusCodes.Forbidden)

  object BadRequest extends ErrorFactory(StatusCodes.BadRequest)

  object Unexpected extends ErrorFactory(StatusCodes.InternalServerError)

  object PreconditionFailed extends ErrorFactory(StatusCodes.PreconditionFailed)



Acode


gdal_translate

gdal_translate x.tif x_cog.tif 
  -of COG -co COMPRESS=DEFLATE
  -co NUM_THREADS=ALL_CPUS
  --config GDAL_CACHEMAX 1024


Bash params

#!/usr/bin/env bash
set -Eeuo pipefail
set -x

suffix=${1-TIF}
srcName=$2
prefix=${2%.$suffix}
originalName=${prefix}-original.tif
cogName=${prefix}-cog.tif




project/plugins.sbt

addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.10")
addSbtPlugin("org.scalameta" % "sbt-scalafmt" % "2.4.2")
addSbtPlugin("net.virtual-void" % "sbt-dependency-graph" % "0.10.0-RC1")
addSbtPlugin("net.vonbuchholtz" % "sbt-dependency-check" % "3.0.0")
addSbtPlugin("de.heikoseeberger" % "sbt-header" % "5.6.0")
addSbtPlugin("com.typesafe.sbt" % "sbt-license-report" % "1.2.0")
addSbtPlugin("com.github.cb372" % "sbt-explicit-dependencies" % "0.2.15")
addSbtPlugin("io.spray" % "sbt-revolver" % "0.9.1")
addSbtPlugin("com.typesafe.sbt" % "sbt-git" % "1.0.0")
addSbtPlugin("com.timushev.sbt" % "sbt-updates" % "0.5.1")
addSbtPlugin("com.github.gseitz" % "sbt-release" % "1.0.13")

addSbtPlugin("com.typesafe.sbt" % "sbt-native-packager" % "1.5.2")
addSbtPlugin("com.mintbeans" % "sbt-ecr" % "0.15.0")

addSbtPlugin("com.lightbend.sbt" % "sbt-javaagent" % "0.1.5")
addSbtPlugin("org.openapitools" % "sbt-openapi-generator" % "5.0.0")
addSbtPlugin("com.eed3si9n" % "sbt-buildinfo" % "0.10.0")

implicit ordering

Implicit scope
* normal scope = local scope
* imported scope
* companions of all types involved in the method signature, incl. A and supertypes

## Scala Option vs. Haskell Maybe

scala> Option.empty[Int] == Option.empty[String]
val res2: Boolean = true


Prelude> let x = Nothing :: Maybe Int
Prelude> let y = Nothing :: Maybe String
Prelude> x
Nothing
Prelude> y
Nothing
Prelude> x == y

<interactive>:8:6: error:
    • Couldn't match type ‘[Char]’ with ‘Int’
      Expected type: Maybe Int
        Actual type: Maybe String
    • In the second argument of ‘(==)’, namely ‘y’
      In the expression: x == y
      In an equation for ‘it’: it = x == y



## ScalaTest


FlatSpec or FlatSpecLike

Matchers

ArgumentMatchersSugar
IdiomaticMockito
withClue for exceptions



it should "" in { }
ignore should "" in {} 
"X" should "Y" in {} 
"X" should "Y" ignore {}

taggedAs
-n io.astraea.Current


PropSpec for properties
Fixtures

Eventually http://doc.scalatest.org/1.8/org/scalatest/concurrent/Eventually.html



How to handle inconsistent results?

testOptions in Test += Tests.Argument(TestFrameworks.ScalaTest, "-P5")

ParallelTestExecution


http://doc.scalatest.org/1.8/org/scalatest/concurrent/Eventually.html



Older

should == must

list.size shouldBe 3

result should equal (3) // can customize equality
result should === (3)   // can customize equality and enforce type constraints
result should be (3)    // cannot customize equality, so fastest to compile
result shouldEqual 3    // can customize equality, no parentheses required
result shouldBe 3       // cannot customize equality, so fastest to compile, no parentheses required

result should have size 10

should be theSameInstanceAs


 val string = """I fell into a burning ring of fire.
         I went down, down, down and the flames went higher"""
 string should startWith("I fell")
 string should endWith("higher")
 string should not endWith "My favorite friend, the end"
 string should include("down, down, down")
 string should not include ("Great balls of fire")

 string should startWith regex ("I.fel+")
 string should endWith regex ("h.{4}r")
 string should not endWith regex("\\d{5}")
 string should include regex ("flames?")

 string should fullyMatch regex ("""I(.|\n|\S)*higher""")

"abbccxxx" should startWith regex ("a(b*)(c*)" withGroups ("bb", "cc"))

xs should contain theSameElementsAs Seq()

one should be < 7
one should be > 0
one should be <= 7
one should be >= 0

x shouldBe 'booleanF

num shouldBe odd
num should not be even

ref1 should be theSameInstanceAs ref2

sevenDotOh shouldBe 6.9 +- 0.2

traversable shouldBe empty
javaMap should not be empty

traversable should contain ("five")

should contain oneOf
should contain noneOf 
should contain atLeastOneOf (2, 3, 4)
should contain atMostOneOf (5, 6, 7)
contain allOf (2, 3, 5)
contain only

(Vector(" A", "B ") should contain atLeastOneOf ("a ", "b", "c")) (after being lowerCased and trimmed)

should contain inOrderOnly

should contain inOrder

should contain theSameElementsInOrderAs -- same iteration order


FlatSpec
  "An empty Set" should "have size 0" in {
  it should "produce NoSuchElementException when head is invoked" in {



http://www.scalatest.org/user_guide/using_matchers
https://scalamock.org/
https://www.safaribooksonline.com/library/view/testing-in-scala/9781449360313/ch05.html
