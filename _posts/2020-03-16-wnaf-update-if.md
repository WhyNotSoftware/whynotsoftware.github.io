---
layout: post
title: "Why Not a Function #5: update-if"
---

    (defn update-if [m k f] (if (contains? m k) (update m k f) m))

`update-if` is similar to `update`. The only difference is that it does nothing if the associative structure does not contain the key so there is nothing to update.

    (update {:speed 0} :speed str)
    => {:speed "0"}
    
    (update {:velocity 0} :speed str)
    => {:velocity 0, :speed ""}
    
    (update-if {:speed 0} :speed str)
    => {:speed "0"}
    
    (update-if {:velocity 0} :speed str)
    => {:velocity 0}
