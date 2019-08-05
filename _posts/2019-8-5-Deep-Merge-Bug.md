---
layout: post
title: Deep merge in Clojure with a bug
---

I've found a bug in our `deep-merge` `fn`. It was used to override default parameters but some defaults were not overwritten. This is how our `deep-merge` looked like:

    (defn deep-merge
      [m1 m2]
      (into {}
        (map (fn [k]
               (let [v1 (get m1 k)
                     v2 (get m2 k)]
                 [k (if (and (map? v1) (map? v2))
                      (deep-merge v1 v2)
                      (or v2 v1))]))
          (set/union (keys m1) (keys m2)))))

It is rather simple and self-explanatory. It even works most of the time. However, a bug is lurking there. Can you see it? I'll give you a second...

Hopefully you've found it. In which case you can skip this paragraph. If not, consider that the problem was the `or` form. If a value was not present in one of the maps it worked as intented. If it was present in both, it worked as indented too *most of the time*. But if `v2` was present in the map but was `falsy` (`nil` or `false` in Clojure) it would be ignored.

This is how we fixed it:

    (defn deep-merge
      [m1 m2]
      (into {}
        (map (fn [k]
               (let [v1 (get m1 k)
                     v2 (get m2 k)]
                 [k (cond
                      (and (map? v1) (map? v2)) (deep-merge v1 v2)
                      (contains? m2 k) v2
                      :else v1)]))
          (set/union (keys m1) (keys m2)))))

And the associated test:

    (fact (c/deep-merge
            {:a 1, :b true, :c 2, :d 3}
            {:a 4, :b false, :c nil, :e 5})
      => {:a 4, :b false, :c nil, :d 3, :e 5})

Yes, it was a basic error, but easy to make.
