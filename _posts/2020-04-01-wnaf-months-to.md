---
layout: post
title: "Why Not a Function #16: interval-in"
---

    (require '[clj-time.core :as t])

    (defn interval-in
      [start end in]
      (if (t/after? end start)
        (in (t/interval start end))
        (- (in (t/interval end start)))))

`interval-in` serves as a wrapper around `clj-time`'s `interval`. The problem with the original `interval` is that it does not return negative values. It simply throws an exception. Our alternative returns a negative number if `start` is after `end`.
    
    (def now (t/now))
    => #'user/now

    (def hour-later (t/plus now (t/hours 1)))
    => #'user/hour-later

    (interval-in now hour-later t/in-hours)
    => 1
    
    (interval-in hour-later now t/in-hours)
    => -1
