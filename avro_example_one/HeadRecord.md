``` json
{
  "namespace": "com.zgconsultants.avro.schema",
  "type": "record",
  "name": "HeadRecord",
  "fields": [
    {"name": "dtF",   "type": "long",                                                 "doc": "myDateTime doc string"},
    {"name": "sF",    "type": ["null", "string"],                   "default": null,  "doc": "sField could be null"},
    {"name": "lF",    "type": "long",                               "default": 777,   "doc": "lucky number"},
    {"name": "bF1",   "type": ["null", "boolean"],                  "default": null,  "doc": "default could be only null, so is redundant here"},
    {"name": "bF2",   "type": ["boolean", "null"],                  "default": true,  "doc": "default is boolean , so it could be true or false"}
  ]
}
```
