# Records / Algebraic Data Types

Examples of Records / Algebraic Data Types in various languages.

## Scala

```scala
final case class Foo(x: Int, y: Option[String])
object Foo {
  def apply(x: Int, y: String) = Foo(x = x, y = Some(y))
  def apply(x: Int) = Foo(x = x, y = None)
}

final case object Bar
```

## Python

## Rust

## Haskell

## TypeScript

## Go