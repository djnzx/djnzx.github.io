---
layout: post
title:  "Teaching ChatGPT"
date:   2024-04-26 11:00:01 +0200
tags:   [scala, fp]
---

One of my favourite questions on the interview is to write 
implementation for function:
```scala
def repack[A](xs: Seq[Future[A]]): Future[(Seq[A], Seq[Throwable])] = ???
```
it's not so complicated, and even ChatGPT was able to write the code.

But more interesting is that fact that ChatGPT was able to derive the answer on
more complicated one:
```scala
def repack[F[_], A](xs: Seq[F[A]]): F[(Seq[A], Seq[Throwable])] = ???
```
the answer was:

```text
To implement the function repack with the signature
def repack[F[_], A](xs: Seq[F[A]]): F[(Seq[A], Seq[Throwable])], 
you need to put constraints on F[_] that provide the necessary 
functionality to work with F as a context for asynchronous computations.
Here are the constraints you need:

F[_] must be a type constructor:
This means that F must be a higher-kinded type,
which allows it to take another type (e.g., A)
and produce a concrete type (e.g., F[A]).
This constraint enables you to work with 
generic effect types like Future, IO, Task, etc.

F[_] must have an instance of MonadError:
MonadError provides the ability to handle errors within the context of F.
It includes operations like flatMap for sequencing computations and raiseError
for lifting exceptions into the F context. This constraint allows you to handle
both successful results and failures within F.
```

and the code:
```scala
import cats.MonadError
import cats.implicits._

def repack[F[_]: MonadError[*[_], Throwable], A](xs: Seq[F[A]]): F[(Seq[A], Seq[Throwable])] = 
  xs.foldLeft(MonadError[F, Throwable].pure((Seq.empty[A], Seq.empty[Throwable]))) { (acc, next) =>
    acc.flatMap { case (successes, failures) =>
      next.attempt.map {
        case Right(value) => (successes :+ value, failures)
        case Left(exception) => (successes, failures :+ exception)
      }
    }
  }
```

that's awesome!