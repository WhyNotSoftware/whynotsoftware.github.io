---
layout: post
title: "Why Not a Function #28: parse-num"
---

	(import '[goog.i18n NumberFormat] '[goog.i18n.NumberFormat Format])

	(defn parse-num [s] (.parse (NumberFormat. Format.DECIMAL) s))

`parse-num` is a thin ClojureScript wrapper around Google Closure's [NumberFormat](https://google.github.io/closure-library/api/goog.i18n.NumberFormat.html) allowing parsing of decimal numbers using a current Google Closure locale.
    
	(parse-num "1,000.23")
	=> 1000.23
