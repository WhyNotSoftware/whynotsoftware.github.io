---
layout: post
title: "Why Not a Function #10: remove-vals"
---

    (defn remove-vals [m f] (into {} (remove (comp f val) m)))

`remove-vals` removes map entries which have values satisfying a test. This is useful when we are interested in values to filter out some entries.

    (remove-vals {:speed 100, :velocity 11} odd?)
    => {:speed 100}
