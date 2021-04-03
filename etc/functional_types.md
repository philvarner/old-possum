# Functional types

concrete-abstract and precise-imprecise 

TypeScript

```typescript
function f(obj: { name: string }): { name: string } {
  
}
```


```scala
import cats.Semigroup

def f[T : Semigroup](obj1: T, obj2: T): T = 
  obj1 |+| obj2

implicit val intAdditionSemigroup: Semigroup[Int] = 
  Semigroup[Int].instance{ (a, b) => a + b }

f(2)

```