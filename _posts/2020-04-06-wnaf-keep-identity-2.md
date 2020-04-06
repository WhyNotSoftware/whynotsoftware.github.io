---
layout: post
title: "Why Not a Function #19: keep-identity-2"
---

    (def keep-identity-2 (partial keep (comp seq (partial keep identity))))

`keep-identity-2` is like `(keep identity ...)` but squared. We have a sequence of sequences and we want to keep only non-nil elements in those sequences. If the whole nested sequence becomes empty, we want to exclude it from the result.
    
    (keep-identity-2 (repeat 3 (range 5)))
    => ((0 1 2 3 4) (0 1 2 3 4) (0 1 2 3 4))

    (keep-identity-2 [[1 false 2 nil 3] [nil nil] [4 5]])
    => ((1 false 2 3) (4 5))
