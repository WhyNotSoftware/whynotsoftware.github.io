---
layout: post
title: "Why Not a Function #14: uuids->named-uuid"
---

    (import '(java.nio ByteBuffer) '(java.util UUID))

    (defn uuids->named-uuid
      [^UUID id1 ^UUID id2]
      (let [buffer (doto (ByteBuffer/allocate 32)
                     (.putLong (.getMostSignificantBits id1))
                     (.putLong (.getLeastSignificantBits id1))
                     (.putLong (.getMostSignificantBits id2))
                     (.putLong (.getLeastSignificantBits id2)))]
        (UUID/nameUUIDFromBytes (.array buffer))))

`uuids->named-uuid` "combines" two UUIDs into a version 3 [namespace name-based UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier#Versions_3_and_5_(namespace_name-based)). This can be useful if we need a repeatable way of generating always the same UUID based on two known UUIDs.
    
    (def id1 (UUID/randomUUID))
    => #'user/id1

    (def id2 (UUID/randomUUID))
    => #'user/id2

    id1
    => #uuid"9df3af32-dd8b-4d4b-9c3c-b3f6f674c3e7"

    id2
    => #uuid"8af86769-a324-455d-a118-6703103c357d"

    (uuids->named-uuid id1 id2)
    => #uuid"5a262f76-9504-3668-839e-c49319a11aea"

    (uuids->named-uuid id1 id2)
    => #uuid"5a262f76-9504-3668-839e-c49319a11aea"
