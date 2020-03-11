---
layout: post
title: "Why Not a Function #2: unapply"
---

    (defn unapply [f & args] (f args))

`unapply` is a kind of a reverse of `apply` from Clojure.

`apply' lets us use a collection instead of multiple arguments:

    (apply + [1 2])
    => 3

`unapply` lets us use multiple arguments instead of a collection:

    (defn counts [coll] (map count coll))
    => #'user/counts

    (unapply counts "why" "not" "a" "function")
    => (3 3 1 8)
