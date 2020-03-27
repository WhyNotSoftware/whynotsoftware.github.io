---
layout: post
title: "Why Not a Function #13: deep-merge-with"
---

    (defn deep-merge-with [f m1 m2] (deep-merge m1 m2 :with f))

`deep-merge-with` is just a concise way of calling [deep-merge](https://whynotsoftware.github.io/wnaf-deep-merge/) with a function as the first argument.
    
    (deep-merge-with + {:room {:chairs 3, :table 1}} {:room {:chairs 1}})
    => {:room {:chairs 4, :table 1}}
