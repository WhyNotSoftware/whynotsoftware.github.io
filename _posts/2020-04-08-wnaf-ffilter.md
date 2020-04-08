---
layout: post
title: "Why Not a Function #21: ffilter"
---

    (def ffilter (comp first filter))

This one is present in multiple sets of handy utility functions which extend what is available in Clojure core. It could be called `find` except there is already a function with this name in Clojure (`find` is used with maps).

`ffilter` finds the first item that matches a criteria in a collection.
    
    (ffilter odd? (range 2 5))
    => 3
