---
layout: post
title:  "How Boolean can be complicated?"
date:   2024-04-13 11:00:01 +0200
tags:   [java, scala, database]
---

Let's consider a trivial example.
We design a job board which has `isRemote: Boolean` property.

It seems simple. Very simple, but...

From the UI perspective, we add a checkbox `remote`. Access to the value via `remote.checked`  

From the database perspective we add a `boolean` column `remote`. Seems easy, but...

| UI      | semantics                           |
|---------|-------------------------------------|
| `false` | I don't care about this filter      |
| `true`  | I want to filter `remote`-only jobs |

From the database perspective

| database | semantics                                      |
|----------|------------------------------------------------|
| `false`  | we __don't include__ `WHERE filter ...` clause |
| `true`   | we __include__ `WHERE filter=true` clause      |

As a result we have completely different booleans should be treated differently. Completely differently.

| UI      | database                               |
|---------|----------------------------------------|
| `false` | `SELECT * FROM jobs`                   |
| `true`  | `SELECT * FROM jobs WHERE remote=true` |

It becomes quite tedious to handle that, but...

In `Scala`, we tend to represent all filters as `Option[A]` defaulting them to `None`
```scala
case class Filter(
  ...
  remote: Option[Boolean] = None
  ...
)

val filter: Filter = ???
```
- when field set it becomes `Some(true)`
- when filed unset it become `Some(false)`

`Some(false)` leads to `SQL` query `SELECT * FROM jobs WHERE remote=false`
which is not what we wanted to do...

in Scala, `Option` has an amazing method `.filter(A => Boolean)`
our field has type of `Boolean` we can write
```scala
val x = Some(false).filter(identity)
// x: Option[Boolean] = None
```
and now we can nicely exploit that fact and write:
```scala
val maybeFragment: Option[Fragment] = 
  filter.remote.filter(identity).map(remote => fr"remote = $remote")
```
which will do exactly what we wanted to do.
And compose it with all other fragments.
Clear and concise

And that was pretty trivial, but...

Who told you that boolean is easy?