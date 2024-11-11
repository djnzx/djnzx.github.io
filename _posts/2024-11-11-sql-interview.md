---
layout: post
title:  "SQL interview"
date:   2024-11-11 11:00:01 +0200
tags:   [SQL, interview]
---

Having table

| data | ratio |
|:-----|:-----:|
| a    |  0.3  |
| b    |  0.5  |
| c    |  0.4  |
| d    |  1.0  |

need to get the table where each element appears `1/ratio` times

| value |
|:------|
| a     |
| a     |
| a     |
| a     |
| b     |
| b     |
| c     |
| c     |
| c     |
| d     |

let's create the table

```sql
create table public.log
(
    data  text,
    ratio double precision
);
INSERT INTO public.log (data, ratio) VALUES ('a', 0.3);
INSERT INTO public.log (data, ratio) VALUES ('b', 0.5);
INSERT INTO public.log (data, ratio) VALUES ('c', 0.4);
INSERT INTO public.log (data, ratio) VALUES ('d', 1);
```

```sql
select data, ratio from log;
```
output:

| value | ratio |
|:------|:-----:|
| a     |  0.3  |
| b     |  0.5  |
| c     |  0.4  |
| d     |  1.0  |

```sql
select data, ceil(1/ratio) from log; 
```
output:

| value | ceil |
|:------|:----:|
| a     |  4   |
| b     |  2   |
| c     |  3   |
| d     |  1   |

let's explore `series` function

```sql
SELECT generate_series(1, ceil(1/.2)::integer);
```
output:

| value |
|:------|
| 1     |
| 2     |
| 3     |
| 4     |
| 5     |

let's combine

```sql
select data
  from log
  join generate_series(1, ceil(1/ratio)::integer) as series on true;
```

output:

| value |
|:------|
| a     |
| a     |
| a     |
| a     |
| b     |
| b     |
| c     |
| c     |
| c     |
| d     |

let's explore `array` function

```sql
SELECT array_fill(0, ARRAY[5]);
```
output:

| value       |
|:------------|
| {0,0,0,0,0} |

let's explore `unnest` function

```sql
SELECT unnest(array_fill(0, ARRAY[5]));
```
output:

| value |
|:------|
| 0     |
| 0     |
| 0     |
| 0     |
| 0     |

let's combine

```sql
select l.data
  from log l
  join unnest(array_fill(0, ARRAY[ceil(1/l.ratio)::integer])) on true;
```

output:

| value |
|:------|
| a     |
| a     |
| a     |
| a     |
| b     |
| b     |
| c     |
| c     |
| c     |
| d     |

What we see, that `SQL` has a bunch of interesting functions to work with data
