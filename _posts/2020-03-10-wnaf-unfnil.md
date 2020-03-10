---
layout: post
title: Why Not a Function #1: unfnil
---

    (defn unfnil
      [f]
      (fn [& args]
        (let [val (apply f args)]
          (if (empty? val)
            nil
            val))))

`unfnil` is a kind of a reverse of `fnil` from Clojure.

For example, `fnil` can be used with `conj` like this: `(fnil conj #{})` which will replace the first argument to `conj` with `#{}` if it is `nil`. Useful if we want a set instead of a list, because `conj` with `nil` produces a list.

    (update {:alarm true} :days (fnil conj #{}) :monday)
    => {:alarm true, :days #{:monday}}

`unfnil` is the reverse, for instance `(unfnil disj)` with `#{:a} :a` produces a `nil` instead of an empty set. Unlike `fnil`, `unfnil` works with collections only.

    (update *1 :days (unfnil disj) :monday)
    => {:alarm true, :days nil}
