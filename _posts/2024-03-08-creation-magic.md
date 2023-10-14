---
layout: post
title:  "Object creation"
date:   2024-03-08 11:00:01 +0200
tags:   [java, encapsulation]
---

```java
class Point {
  int x;
  int y;
}
```
Is there any problem in this code?
We can discuss it a lot, but the main issue
is that Java creates constructor by default
and by using
```java
Point p = new Point();
p.x = 10;
```
This object is incomplete! 
since we encapsulated `x` and `y` in the class `Point`.
We will have `x = 10`, `y = 0`,
due to the fact Java has default values.
And we will not be able to distinguish that from 
```java
Point p = new Point();
p.x = 10;
p.y = 0;
```
So, the solution is simple, just introduce a constructor
```java
class Point {
  int x;
  int y;

  Point(int x, int y) {
    this.x = x;
    this.y = y;
  }
}
```
in case we have at least one constructor defined, Java will not create
"default" constructor without parameters.
and our code
```java
Point p = new Point();
```
will stop compiling.
Now the only valid code is
```java
Point p = new Point(3, 5);
```