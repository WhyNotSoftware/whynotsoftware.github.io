---
layout: post
title: "Why Not a Function #6: assoc-non-empty-in"
---

    (defn assoc-non-empty-in
      [m ks v]
      (if (or (nil? v) (= v ""))
        (let [ret (dissoc-in m ks)]
          (if (= (get-in ret (butlast ks)) {})
            (dissoc-in ret (butlast ks))
            ret))
        (assoc-in m ks v)))

`assoc-non-empty-in` is different from `assoc-in` because it will dissoc a key if the value is a `nil` or if it is an empty string. In addition to this, if the resulting parent map is empty, it will dissoc it as well. It does not dissoc further parent maps.

    (assoc-non-empty-in {} [:room :lights :color] "white")
    => {:room {:lights {:color "white"}}}

    (assoc-non-empty-in *1 [:room :lights :color] "")
    => {:room {}}
