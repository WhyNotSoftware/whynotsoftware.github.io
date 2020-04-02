---
layout: post
title: "Why Not a Function #17: add-months"
---

    (require '[clj-time.core :as t])

    (defn add-months
      [n]
      (-> (t/first-day-of-the-month (t/today))
        (t/plus (t/months n))))

`add-months` uses `clj-time` to compute a date at the first day of the month `n` months in the future.
    
    (add-months 3)
    => #date"2020-06-01"
