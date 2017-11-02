Kotlin Cons
===========

[![Download](https://api.bintray.com/packages/aedans/maven/kotlin-cons/images/download.svg)](https://bintray.com/aedans/maven/kotlin-cons/_latestVersion) 

Kotlin linked list implementation using lazily evaluated cons cells.

Examples
--------

```kotlin
tailrec fun <A, R> Cons<A>.fold(r: R, fn: (R, A) -> R): R = when (this) {
    Nil -> r
    else -> cdr.fold(fn(r, car), fn)
}

// Lazily filters a list
tailrec fun <T> Cons<T>.filter(fn: (T) -> Boolean): Cons<T> = when (this) {
    Nil -> Nil
    else -> if (fn(car)) ({ car }) cons { cdr.filter(fn) } else cdr.filter(fn)
}

// Stack safe due to lazy evaluation
fun <A, B> Cons<A>.map(fn: (A) -> B): Cons<B> = when (this) {
    Nil -> Nil
    else -> let { (x, xs) -> { fn(x) } cons { xs.map(fn) } }
}

// List of all natural numbers
val nat = generateCons(0) { it + 1 }

// List of all positive even numbers
val evens = nat.map { it * 2 }

// List of all primes
val primes = nat.filter { i -> (2 until i).none { i % it == 0 } }
```
