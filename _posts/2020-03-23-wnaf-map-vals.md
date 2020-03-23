---
layout: post
title: "Why Not a Function #9: map-vals"
---

    (defn map-vals [m f] (into {} (map (fn [[k v]] [k (f v)]) m)))

`map-vals` goes through the values in a hash map and calls a function on them. The returned map has all the same keys but the values are changed with the function.

    (map-vals {:speed "100", :velocity "10"} #(Integer. %))
    => {:speed 100, :velocity 10}
