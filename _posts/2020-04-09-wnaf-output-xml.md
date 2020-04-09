---
layout: post
title: "Why Not a Function #22: output-xml"
---

    (require '[clojure.data.xml :as xml] '[clojure.java.io :as io])
    (import '(java.io ByteArrayOutputStream))

    (defn output-xml
      [in]
      (let [element (xml/sexp-as-element in)]
        (with-open [s (ByteArrayOutputStream. 4096)]
          (with-open [w (io/writer s)]
            (xml/emit element w))
          (.toByteArray s))))

This little function `output-xml` accepts an input in the [Hiccup-like](https://github.com/weavejester/hiccup) format and returns a byte array. It's a wrapper around [org.clojure/data.xml](https://github.com/clojure/data.xml) library. While there are other [representations of XML](https://github.com/clojure-cookbook/clojure-cookbook/blob/master/04_local-io/4-22_read-write-xml.asciidoc) in Clojure the Hiccup-like is quite nice.
    
    (output-xml [:sky {:color "blue"}])
    => #object["[B" 0x3933198c "[B@3933198c"]

    (String. *1)
    => "<?xml version=\"1.0\" encoding=\"UTF-8\"?><sky color=\"blue\"></sky>"
