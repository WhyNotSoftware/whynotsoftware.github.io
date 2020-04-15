---
layout: post
title: "Why Not a Function #24: xml->map"
---

    (require '[goog.dom.xml :as xml])

    (defn xml->map
      [xml element-name]
      (->> (xml/selectNodes (xml/loadXml xml) (str "//" element-name "/node()"))
        (filter #(instance? js/Element %))
        (mapcat (fn [e] [(.-tagName e) (.-textContent e)]))
        (apply hash-map)))

Parsing XML in ClojureScript doesn't have to be difficult. It all comes down to a specific requirement at hand, but Google Closure library is here very useful. The `xml->map` function uses it to convert an XML element to a ClojureScript map. Each child element creates an entry in the map.
    
    (xml->map "<basket><name>ball</name><colour>orange</colour></basket>"
              "basket")
    => {"colour" "orange", "name" "ball"}
