---
layout: post
title: "Why Not a Function #29: decimal-fmt"
---

	(import '[goog.i18n NumberFormat] '[goog.i18n.NumberFormat Format])

	(defn decimal-fmt
	  ([x] (decimal-fmt x 2))
	  ([x digits] (decimal-fmt x digits digits))
	  ([x min-digits max-digits]
	   (.format
	     (doto (NumberFormat. Format.DECIMAL)
	       (.setMinimumFractionDigits min-digits)
	       (.setMaximumFractionDigits max-digits))
	     x)))

`decimal-fmt` is almost a reverse of [parse-num](https://whynotsoftware.github.io/wnaf-parse-num/) from the previous blog. It is a ClojureScript wrapper around Google Closure's [NumberFormat](https://google.github.io/closure-library/api/goog.i18n.NumberFormat.html) allowing formatting of decimal numbers with rounding using a current Google Closure locale. We specify the number of decimal digits we want to include. Moreover, we can use a nice `NumberFormat` feature which automatically selects minimum decimal digits required from a given range to represent our value.
    
	(decimal-fmt 1.55555)
	=> "1.56"

	(decimal-fmt 1.55555 0 3)
	=> "1.556"

	(decimal-fmt 1.0 0 3)
	=> "1"
