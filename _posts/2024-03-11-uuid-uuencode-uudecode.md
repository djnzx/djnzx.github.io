---
layout: post
title:  "UUID generation, base64 encode/decode"
date:   2024-03-11 11:00:01 +0200
tags:   [linux, uuidgen, base64]
---

#### How to generate UUID in the linux

```shell
uuidgen
```
will get something like
```text
40CB8E14-B8E7-4C7F-849E-76E1ACC23101
```

#### How to encode your secret with `base64`

```shell
echo -n "my super secret" | base64
```
the result will be
```text
bXkgc3VwZXIgc2VjcmV0
```
#### How to decode your secret (previously encoded with `base64`)

```shell
echo bXkgc3VwZXIgc2VjcmV0 | base64 -d
```
the result will be
```text
my super secret%
```
the `%` char in the end means there is no CR/LF in the end

#### Note:

If you forget `-n` during encode:
the result will be
```text
bXkgc3VwZXIgc2VjcmV0Cg==
```
That's a different thing.
We encoded line `my super secret` with `\n` in the end.

During the decode output will be:
```text
my super secret
```
without `%` char in the end.
That means, that output contains `\n` in the end.  

### There is a useful pipe on the macOS to put output to the clipboard

```shell
echo -n "my super secret" | base64 | pbcopy
```
the result `bXkgc3VwZXIgc2VjcmV0` will be in clipboard
