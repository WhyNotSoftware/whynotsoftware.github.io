---
layout: post
title: "Why Not a Function #25: list-index-of"
---

    (defn list-index-of
      [list element]
      (let [r (reduce
                (fn [n x] (if (= x element) (reduced n) (inc n)))
                0 list)]
        (when (not= r (count list))
          r)))

Checking an index of an element in a list cannot be done using a dedicated function in Clojure. If possible, it may be better to use a vector and its [`.indexOf`](https://github.com/clojure/clojure/blob/38bafca9e76cd6625d8dce5fb6d16b87845c8b9d/src/jvm/clojure/lang/APersistentVector.java#L186) method in this case. It still must do a linear search, mind you.

But what if we have a sequence of a "reasonable" size and don't want to convert it to a vector)? Or, if we are in ClojureScript and we would have to convert it to a JavaScript array and then depend on JavaScript's strict equality semantics when calling the array's `.indexOf` method?

We can use `list-index-of`. If element is not found it will return `nil` otherwise it will return a zero-based index.

    (list-index-of '(:green :amber :red) :blue)
    => nil

    (list-index-of '(:green :amber :red) :amber)
    => 1
