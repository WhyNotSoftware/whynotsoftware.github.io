---
layout: post
title: "Why Not a Function #12: deep-merge (with)"
---

    (defn deep-merge
      [m1 m2 & {:keys [with]}]
      (into {}
        (map (fn [k]
               (let [v1 (get m1 k)
                     v2 (get m2 k)]
                 [k (cond
                      (and (map? v1) (map? v2)) (deep-merge v1 v2 :with with)
                      (and with (contains? m1 k) (contains? m2 k)) (with v1 v2)
                      (contains? m2 k) v2
                      :else v1)]))
          (set/union (keys m1) (keys m2)))))

`deep-merge` was already mentioned on this blog when we [discovered a bug in one of the previous versions of it](https://whynotsoftware.github.io/Deep-Merge-Bug/). We showed a fixed version back then as well, but the version above is even more up to date as we have added a new functionality.

The basic `deep-merge` works just as standard `merge` but is "deep" meaning that if maps have submaps it will merge these submaps rather than replacing them.
    
    (merge
      {:keyboard {:color "black", :plug "usb"}, :mouse {:make "logitech"}}
      {:keyboard {:plug "ps/2"}})
    => {:keyboard {:plug "ps/2"}, :mouse {:make "logitech"}}

    (deep-merge
      {:keyboard {:color "black", :plug "usb"}, :mouse {:make "logitech"}}
      {:keyboard {:plug "ps/2"}})
    => {:keyboard {:plug "ps/2", :color "black"}, :mouse {:make "logitech"}}

We have replaced `:plug` and retained `:color`. What if we want to keep both `:plug` values? We can use the `:with` option which works similarly to Clojure's `merge-with`.

    (deep-merge
      {:keyboard {:color "black", :plug "usb"}, :mouse {:make "logitech"}}
      {:keyboard {:plug "ps/2"}}
      :with (fn [a b] (str a "; " b)))
    => {:keyboard {:plug "usb; ps/2", :color "black"}, :mouse {:make "logitech"}}
