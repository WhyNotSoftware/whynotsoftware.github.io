---
layout: post
title: "Why Not a Function #27: xsd-decimal-fmt"
---

    (defn xsd-decimal-fmt [x] (String/format "%.2f" (to-array [x])))

`xsd-decimal-fmt` is useful when [building an XML document](https://whynotsoftware.github.io/wnaf-output-xml/) which has to represent decimal numbers in the default way specified by the XML Schema standard.
    
    (xsd-decimal-fmt 1.0)
    => "1.00"
