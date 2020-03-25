---
layout: post
title: "Why Not a Function #11: some-map"
---

    (defn some-map [m] (remove-vals m nil?))

`some-map` builds on top of [remove-vals](https://whynotsoftware.github.io/wnaf-remove-vals/) to provide a really quick way of filtering maps out of `nil` values. It is named `some-map` because in Clojure there is `some?` function which returns `false` only if the argument is `nil`.

    (some-map {:flower "red", :apple "green", :fabric nil})
    => {:flower "red", :apple "green"}

    (some? nil)
    => false
