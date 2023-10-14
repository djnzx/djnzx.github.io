---
layout: post
title:  "Java 5. Atomic Reference"
date:   2004-09-30 02:00:01 +0200
tags:   [intel, cas, atomic]
permalink: /java5/
---
### Java 5 has Atomic reference support!

Notable Java5 Features:

- Generics!
- AtomicReference was introduced to leverage [i486](/i486dx/) CMPXCHG instruction
  Now we can stop using `synchronized`, but it's hard to do
  because it metastasized into the whole codebase 
- varargs
- enums
- auto boxing/unboxing
- foreach loop
