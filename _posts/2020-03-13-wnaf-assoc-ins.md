---
layout: post
title: "Why Not a Function #4: assoc-ins"
---

    (defn assoc-ins
      [m ks-v-s]
      (reduce (fn [r [ks v]] (assoc-in r ks v)) m (partition 2 ks-v-s)))

`assoc-ins` is similar to `assoc-in`. The only difference is that it accepts multiple pairs of path and value.

    (assoc-in {:clouds {:motion true}} [:clouds :speed] 10)
    => {:clouds {:motion true, :speed 10}}
    
    (assoc-ins *1 [[:clouds :speed] 5
                   [:sun :shining] true])
    => {:clouds {:motion true, :speed 5}, :sun {:shining true}}
