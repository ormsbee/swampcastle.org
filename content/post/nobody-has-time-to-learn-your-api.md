+++
date        = "2017-05-13"
title       = "Nobody Has Time to Learn Your API"
tags        = ["short"]
+++

> "If they'd just learn how to use it _correctly_..."

If people are shooting themselves in the foot with your API, it's probably your
fault. If your defaults are unsafe for production use, your API is broken.

Most of the time, your API is going to give someone access to some small piece
of data or functionality, and learning how to use it is a tiny footnote among
the hundreds of things that need to be done in order to deliver some product.
People are going to swoop into your documentation (you have that, right?), pick
the first thing that sort of looks like it will work, try it, and then move on.
Rather than lament that the unworthy masses aren't doing their homework, you
need to _design_ with their limited time in mind:

* Anticipate common use cases and make them as safe and simple as possible.
  Bring your users to the [pit of success](https://blog.codinghorror.com/falling-into-the-pit-of-success/).
* Use sane defaults, like reasonable timeouts on connections.
* Advertise and enforce limits loudly and clearly.
* Make your documented example code realistic. Avoid dummy variable names like
  "foo" and "bar". Don't write incomplete, "illustrative" code examples that
  would lead to horrible performance or security problems in a real environment.
