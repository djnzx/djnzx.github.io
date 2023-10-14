---
layout: post
title:  "What was the most expensive line of code you have been paid for?"
date:   2024-03-10 11:00:01 +0200
tags:   [scala, http4s, saml, pac4j]
---

### Fix seems very innocent

Two years ago, company paid me $10,000.00 for this
[fix](https://github.com/pac4j/http4s-pac4j/commit/5a621500a3d5e287f28f8d71a546f70243920463)
```diff
-   override def getRequestContent: String =
+   override lazy val getRequestContent: String =
      bodyExtractor(request.bodyText.compile.to(Collector.string))
```