---
layout: post
title: "Why Not a Function #8: conj-sort-v"
---

    (defn conj-sort-v [v x by] (->> ((fnil conj []) v x) (sort-by by) (vec)))

`conj-sort-v` is a syntactic sugar to combine `conj`, `sort-by` and `vec` in a single call. Useful only in some very specific cases and not something that should be used lightly.

    (conj-sort-v [1 2 3] 4 -)
    => [4 3 2 1]
