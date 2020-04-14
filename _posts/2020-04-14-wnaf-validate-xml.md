---
layout: post
title: "Why Not a Function #23: validate-xml"
---

    (import
      '(javax.xml.validation SchemaFactory)
      '(javax.xml XMLConstants)
      '(javax.xml.transform.stream StreamSource)
      '(org.xml.sax ErrorHandler)
      '(java.net URL))

    (defn validate-xml
      [xml ^URL xsd]
      {:pre [xml]}
      (let [factory (SchemaFactory/newInstance
                      XMLConstants/W3C_XML_SCHEMA_NS_URI)
            schema (.newSchema factory xsd)
            validator (.newValidator schema)
            source (-> xml (io/input-stream) (StreamSource.))
            warnings-as-errors (reify ErrorHandler
                                 (warning [_ ex] (throw ex))
                                 (error [_ ex] (throw ex))
                                 (fatalError [_ ex] (throw ex)))]
        (.setErrorHandler validator warnings-as-errors)
        (.validate validator source)))

Validating an XML document against an XML Schema (XSD) can be done in pure JVM and this is a Clojure code to do it. The first argument to `validate-xml` is anything that can be converted into input stream (the XML) and the second argument is an `URL` (the XSD). In case of any validation warnings or errors this routine will throw a `SAXParseException`. Otherwise it will just return `nil`.

Below we are additionally using the `output-xml` function defined in [the previous blog](https://whynotsoftware.github.io/wnaf-output-xml/).
    
    (output-xml [:yeehaw])
    => #object["[B" 0x3dd69b7f "[B@3dd69b7f"]

    (validate-xml *1 (URL. "https://www.w3.org/2001/03/xml.xsd"))
    Execution error (SAXParseException) at com.sun.org.apache.xerces.internal.util.ErrorHandlerWrapper/createSAXParseException (ErrorHandlerWrapper.java:203).
    cvc-elt.1: Cannot find the declaration of element 'yeehaw'.
