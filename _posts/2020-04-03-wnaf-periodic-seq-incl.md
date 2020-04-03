---
layout: post
title: "Why Not a Function #18: periodic-seq-incl"
---

    (require '[clj-time.core :as t] '[clj-time.periodic :as tp])

    (defn periodic-seq-incl
      [start end period]
      (tp/periodic-seq start (t/plus end period) period))

`clj-time`'s `periodic-seq` is like Clojure's `range' in terms of boundaries - the end boundary is exclusive. `periodic-seq-incl` is a wrapper over it which includes the `end` of the boundary in the result.
    
    (periodic-seq-incl (t/local-date 2020 2 1) (t/local-date 2020 3 1) (t/months 1))
    => (#date"2020-02-01" #date"2020-03-01")
