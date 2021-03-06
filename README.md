# iota

Infix Operators for Test Assertions

## What is this?

When you are writing tests, it's common to want to check a number of properties against a given value.

```clojure
(require '[clojure.test :refer :all]
         '[schema.core :as s])

(deftest my-test
  (let [response ...]
    (is (= (:status response) 200))
    (is (= (count (get-in response [:headers "content-length"])) (count "Hello World!")))
    (is (= (count (get-in response [:headers "content-type"])) "text/plain;charset=utf-8"))
    (is (nil? (s/check s/Str (get-in response [:headers "content-type"]))))
    (is (instance? java.nio.ByteBuffer response))
    ))
```

This can get a bit cumbersome.

__iota__ is a micro-library that provides a single macro, `juxt.iota/given`, that allows you to create triples, each of which expands into a `clojure.test/is` assertion.

```clojure
(require '[clojure.test :refer :all]
         '[schema.core :as s]
         '[juxt.iota :refer [given]])

(deftest my-test
  (given response
          :status := 200
          :headers :⊃ {"content-length" (count "Hello World!")
                       "content-type" "text/plain;charset=utf-8"}
          [:headers "content-type"] :- s/Str
          :body :instanceof java.nio.ByteBuffer))
```

It's a little less typing, which might give you the extra time to add more checks, improving your testing.

## Valid operators

Operator       | Meaning
---------------|-------------------
`:=`           | is equal to
`:!=`          | isn't equal to
`:-`           | conforms to schema
`:!-`          | doesn't conform to schema
`:?`           | satifies predicate
`:!?`          | doesn't satisfy predicate
`:#`           | matches regex
`:!#`          | doesn't match regex
`:>` or `:⊃`   | is superset of
`:!>` or `:⊅`  | isn't superset of
`:<` or `:⊂`   | is subset of
`:!<` or `:⊄`  | isn't subset of
`:instanceof`  | is instance of
`:!instanceof` | isn't instance of

## Paths

The left hand operand of your expressions can be a vector, which acts as a path. A bit like `->` but you can also use strings.

## Installation

Add the following dependency to your `project.clj` file

```clojure
[juxt/iota "0.2.3"]
```

## Copyright & License

The MIT License (MIT)

Copyright © 2015 JUXT LTD.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
