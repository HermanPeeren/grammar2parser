
@HermanPeeren in https://github.com/mathiasverraes/parsica/issues/9 26-8-2020

Unique keys in JSON-object
---
The current JSON-parser in Parsica has the same behaviour as json_decode regarding non-unique keys in a JSON-object: it just overwrites the value by the value of the last occurence of that key:
~~~
$JSON = '{"key1":"value1","key2":"value2a","key2":"value2b","key3":"value3"}';
$object = json_decode($JSON);
var_dump($object);
~~~
gives:
~~~
object(stdClass)[1]
  public 'key1' => string 'value1' (length=6)
  public 'key2' => string 'value2b' (length=7)
  public 'key3' => string 'value3' (length=6)
~~~

PHP uses [RFC 7159](http://www.faqs.org/rfcs/rfc7159.html) for its JSON-definition, which states in section 4: "The names within an object SHOULD be unique" ("names" == "keys"). If you want to use the parser to check the validity of the JSON-input, then it should give a warning or error. PHP's json_decode doesn't do that; it just overwrites the value.

In the original JSON-definition [RFC 4627]( https://tools.ietf.org/html/rfc4627#section-2.2) object-keys don't need to be unique (although that doesn't make much sense to me). Checking for unique keys makes the JSON-definition context-sensitive. See [this posting on Stackoverflow](https://stackoverflow.com/questions/3335026/which-formal-language-class-are-xml-and-json-with-unique-keys-they-are-not-cont). 

[In Parsica](https://github.com/mathiasverraes/parsica/blob/main/src/JSON/JSON.php#L93-L99) the key-value-pairs are sequentially written in an associative array, which is then cast in an object.

Parsing context-free languages is generally simpler than context-sensitive languages. Context-sensitivity cannot be expressed in [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) / [ABNF](https://github.com/mathiasverraes/parsica/issues/8) grammars. But context-sensitivity is a necessary property for a Turing-complete language.

---

@mathiasverraes answer 30-8-2020

Context sensitivity will be supported when we introduce parser state in Parsica. The current plan is to do something similar to Haskell's State Monad, but no work has been done in that regard yet.

As for the json parser:

* The goal in Parsica is specifically to have the same behaviour as php's json_encode, because it can then serve as a drop in replacement, and as a target for benchmarking. A json parser that fails on duplicate could be built as a POC of the state monad of course.
* ["SHOULD" is meant as "recommended" in the RFC](http://www.faqs.org/rfcs/rfc2119.html).
