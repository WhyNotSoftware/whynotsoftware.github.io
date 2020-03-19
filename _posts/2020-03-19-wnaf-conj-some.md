---
layout: post
title: "Why Not a Function #7: conj-some"
---

    (defn conj-some [coll x] (if (some? x) (conj coll x) coll))

`conj-some` is very similar to `conj` but it will do nothing if `x` is `nil`.

    (conj-some [1 2] 3)
    => [1 2 3]

    (conj-some [1 2] nil)
    => [1 2]
