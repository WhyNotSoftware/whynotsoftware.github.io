---
layout: post
title: "Why Not a Function #3: dissoc-in"
---

    (defn dissoc-in
      [m [k & knext :as ks]]
      (cond
        (and knext
          (contains?
            (get-in m (butlast ks))
            (last ks))) (update-in m (butlast ks) dissoc (last ks))
        (not knext) (dissoc m k)
        :else m))

`dissoc-in` is a mixture of `assoc-in` and `dissoc`. It lets us remove an entry from a nested associative structure using a path of keys.

    (assoc-in {:pencil "blue"} [:computer :mouse :left-button] "green")
    => {:pencil "blue", :computer {:mouse {:left-button "green"}}}
    
    (dissoc-in *1 [:computer :mouse :left-button])
    => {:pencil "blue", :computer {:mouse {}}}

It doesn't make any changes if the key path does not exist.

    (dissoc-in *1 [:monitor :stand])
    => {:pencil "blue", :computer {:mouse {}}}
