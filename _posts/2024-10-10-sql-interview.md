---
layout: post
title:  "SQL interview"
date:   2024-10-10 11:00:01 +0200
tags:   [SQL, interview]
---

Having table

| c1 |
|:---|
| 1  |
| 2  |
| 3  |
| 4  |
| 6  |
| 7  |
| 8  |
| 9  |

need to find the missing number `5` in the sequence

1. let's create the table

```sql
CREATE TABLE t1(c1 integer);

INSERT INTO t1 (c1)
VALUES (1),
       (2),
       (3),
       (4),
       (6),
       (7),
       (8),
       (9);
```

our login will be based on shifting the sequence by 1,
then find the missing bit.

```sql
select c1 + 1 from t1;
```
output:

| ?column? |
|:---------|
| 2        |
| 3        |
| 4        |
| 5        |
| 7        |
| 8        |
| 9        |
| 10       |

let's combine

```sql
select c1,c2
from t1
right outer join (
                  select c1 + 1 as c2 
                  from t1
                 ) as t2 on (t2.c2 = t1.c1);

```
output:

| c1   | c2 |
|:-----|:---|
| 2    | 2  |
| 3    | 3  |
| 4    | 4  |
| null | 5  |
| 7    | 7  |
| 8    | 8  |
| 9    | 9  |
| null | 10 |

let's pick what we need:

```sql
select c2
from t1
right outer join (select c1 + 1 as c2 from t1) as t2 on (t2.c2 = t1.c1)
where c1 is null;

```

output:

| c2 |
|:---|
| 5  |
| 10 |

let's narrow the results with `limit 1`

the final solution: 

```sql
select c2
from t1
         right outer join (select c1 + 1 as c2 from t1) as t2 on (t2.c2 = t1.c1)
where c1 is null
order by c2
limit 1
```

output:

| c2 |
|:---|
| 5  |
