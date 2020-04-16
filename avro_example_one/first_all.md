#### FirstMsg 

This is how it looks all together.

```json
{
  "type" : "record",
  "name" : "FirstMsg",
  "namespace" : "com.zgconsultants.avro.schema",
  "doc" : "FirstMsg  some very relevant doc about this type",
  "fields" : [ {
    "name" : "head",
    "type" : {
      "type" : "record",
      "name" : "HeadRecord",
      "fields" : [ {
        "name" : "dtF",
        "type" : "long",
        "doc" : "myDateTime doc string"
      }, {
        "name" : "sF",
        "type" : [ "null", {
          "type" : "string",
          "avro.java.string" : "String"
        } ],
        "doc" : "sField could be null",
        "default" : null
      }, {
        "name" : "lF",
        "type" : "long",
        "doc" : "lucky number",
        "default" : 777
      }, {
        "name" : "bF1",
        "type" : [ "null", "boolean" ],
        "doc" : "default could be only null, so is redundant here",
        "default" : null
      }, {
        "name" : "bF2",
        "type" : [ "boolean", "null" ],
        "doc" : "default is boolean , so it could be true or false",
        "default" : true
      } ]
    },
    "doc" : "head:   some very relevant doc"
  }, {
    "name" : "first",
    "type" : "long",
    "doc" : "first:  some very relevant doc"
  }, {
    "name" : "second",
    "type" : "double",
    "doc" : "second: some very relevant doc"
  }, {
    "name" : "third",
    "type" : [ "null", {
      "type" : "string",
      "avro.java.string" : "String"
    } ],
    "doc" : "third:  some very relevant doc",
    "default" : null
  }, {
    "name" : "forth",
    "type" : [ "null", {
      "type" : "enum",
      "name" : "EnumTypeOne",
      "symbols" : [ "ONE", "TWO", "THREE", "FORE" ]
    } ],
    "doc" : "forth:  some very relevant doc",
    "default" : null
  }, {
    "name" : "tail",
    "type" : [ "null", {
      "type" : "record",
      "name" : "TailRecord",
      "fields" : [ {
        "name" : "sF",
        "type" : [ "null", {
          "type" : "string",
          "avro.java.string" : "String"
        } ],
        "doc" : "some very relevant doc",
        "default" : null
      }, {
        "name" : "lF",
        "type" : [ "null", "long" ],
        "doc" : "some very relevant doc",
        "default" : null
      } ]
    } ],
    "doc" : "tail:   some very relevant doc",
    "default" : null
  } ]
}
```

