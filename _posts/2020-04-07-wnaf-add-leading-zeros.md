---
layout: post
title: "Why Not a Function #20: add-leading-zeros"
---

    (defn add-leading-zeros
      [n s]
      (str/replace s #"\d+"
        #(str/join (concat (repeat (- n (count %)) "0") [%]))))

`add-leading-zeros` prepends numbers in the string with the *0* characters until each of the numbers reaches the desired length. It can be used, for example, for intelligent sorting.
    
    (add-leading-zeros 3 "line 1: it's #wnaf #20! ")
    => "line 001: it's #wnaf #020! "
