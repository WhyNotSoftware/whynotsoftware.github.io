---
layout: post
title: "Why Not a Function #26: months-to"
---

    (require '[cljs-time.core :as t])

    (defn months-to
       [date]
       (interval-in
         (t/first-day-of-the-month (t/today))
         (t/first-day-of-the-month date)
         t/in-months))

`months-to` builds on top of [`interval-in`](https://whynotsoftware.github.io/wnaf-months-to/) to provide the number of months that are between today and a date specified. It ignores the day of the month. Can be used in pair with [`add-months`](https://whynotsoftware.github.io/wnaf-add-months/).
    
    (months-to (t/today))
    => 0

    (months-to (t/local-date 2020 05 01))
    => 1
