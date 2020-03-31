---
layout: post
title: "Why Not a Function #15: random-uuid"
---

    (import '(java.util UUID))

    (defn random-uuid [] (UUID/randomUUID))

`random-uuid` is just an equivalent for Clojure of what is available in ClojureScript under the same name.
    
    (random-uuid)
    => #uuid"a3d3dafe-707f-4f52-abbd-be1d5f2dcb77"
