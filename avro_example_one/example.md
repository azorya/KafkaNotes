
## AVRO Example

This example is to illustrate a few thigs:
1. [What is an avro message](#aeo_flink_one)
2. [What is the interface to the schema-registry and how to handle schemas.](#aeo_flink_two)
3. [How it works](#aeo_flink_three)
4. [Final notes](#aeo_flink_fore)

In this example we will use two different types of messages: *FirstMsg* and *SecondMsg* <a name="aeo_flink_one"/>.
Avro message description is a json file.

**FirstMsg**  

```json
{
  "namespace": "com.zgconsultants.avro.schema",
  "type": "record",
  "name": "FirstMsg",
  "doc":  "FirstMsg  some very relevant doc about this type",
  "fields": [
     {"name": "head",             "type": "com.zgconsultants.avro.schema.HeadRecord",                                 "doc": "head:   some very relevant doc"},
     {"name": "first",            "type": "long",                                                                     "doc": "first:  some very relevant doc"},
     {"name": "second",           "type": "double",                                                                   "doc": "second: some very relevant doc"},
     {"name": "third",            "type": ["null","string"],                                       "default": null,   "doc": "third:  some very relevant doc"},
     {"name": "fourth",           "type": ["null","com.zgconsultants.avro.schema.EnumTypeOne"],    "default": null,   "doc": "forth:  some very relevant doc"},
     {"name": "tail",             "type": ["null","com.zgconsultants.avro.schema.TailRecord"],     "default": null,   "doc": "tail:   some very relevant doc"}
   ]
}
```
It is a _record_ which has six fields. head and tail in turn are also _records_ and the field "fourth" stores an _enum_.
* [head](./HeadRecord.md)
* first  is long
* second is double
* third  is string
* [fourth](./EnumTypeOne.md)
* [tail](./TailRecord.md)

**SecondMsg**

``` json
{
  "namespace": "com.zgconsultants.avro.schema",
  "type": "record",
  "name": "SecondMsg",
  "doc":  "FirstMsg  some very relevant doc about this type",
  "fields": [
     {"name": "head",             "type": "com.zgconsultants.avro.schema.HeadRecord",                                 "doc": "head:   some very relevant doc"},
     {"name": "first",            "type": "long",                                                                     "doc": "first:  some very relevant doc"},
     {"name": "second",           "type": "double",                                                                   "doc": "second: some very relevant doc"},
     {"name": "third",            "type": ["null","string"],                                       "default": null,   "doc": "third:  some very relevant doc"},
     {"name": "fourth",           "type": ["null","com.zgconsultants.avro.schema.EnumTypeOne"],    "default": null,   "doc": "forth:  some very relevant doc"},
     {"name": "fifth",            "type": ["null", {"type": "array", "items":"long"}],             "default": null,   "doc": "fifth:  an array element      "},
     
     {"name": "tail",             "type": ["null","com.zgconsultants.avro.schema.TailRecord"],     "default": null,   "doc": "tail:   some very relevant doc"}
   ]
}
```
It has one more field _fifth_ which is an array of longs.

### Interacting with schema-registry <a name="aeo_flink_two"/>

Keep in mind that to register our schema with _curl_ one must [put it all together](./first_all.md). Messages in this example are tiny, so it is not that big of a deal, but with real-life structures it becomes very inconvenient. We can build a few tools to deal with nested schemas, and to help register schemas with proper names.

To work with the schema registry confluent community doesn't have any special tools but curl. Their [crash cource](https://docs.confluent.io/current/schema-registry/using.html) is very helpfull. 

In our example we already have this messages registered, so let us examine it using curl <a name="aeo_blink_one"/>.

    zconsult@kafkaqa3:~/test/zSDump-0.0.1/bin$ curl -X GET http://kafkaqa1:8081/subjects | jq
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    100    24  100    24    0     0   3000      0 --:--:-- --:--:-- --:--:--  3000
    [
       "FirstMsg",
       "SecondMsg"
    ]

check versions of our first schema. Piping curl output to "jq" allows for better formating.

    zconsult@kafkaqa3:~/test/zSDump-0.0.1/bin$ curl -X GET http://kafkaqa1:8081/subjects/FirstMsg/versions | jq
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
     100     3  100     3    0     0    500      0 --:--:-- --:--:-- --:--:--   500
    [
      1
    ]

Looking at the schema, the fields that were _records_, or _enums_, defined originally in separate files, are now recursively included into one large json-like string.

    zconsult@kafkaqa3:~/test/zSDump-0.0.1/bin$ curl -X GET http://kafkaqa1:8081/subjects/FirstMsg/versions/1 | jq  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
     100  1817  100  1817    0     0   197k      0 --:--:-- --:--:-- --:--:--  197k
    {
    "subject": "FirstMsg",
    "version": 1,
    "id": 1,
    "schema": "{\"type\":\"record\",\"name\":\"FirstMsg\",\"namespace\":\"com.zgconsultants.avro.schema\",\"doc\":\"FirstMsg  some very relevant doc about this type\",\"fields\":[{\"name\":\"head\",\"type\":{\"type\":\"record\",\"name\":\"HeadRecord\",\"fields\":[{\"name\":\"dtF\",\"type\":\"long\",\"doc\":\"myDateTime doc string\"},{\"name\":\"sF\",\"type\":[\"null\",{\"type\":\"string\",\"avro.java.string\":\"String\"}],\"doc\":\"sField could be null\",\"default\":null},{\"name\":\"lF\",\"type\":\"long\",\"doc\":\"lucky number\",\"default\":777},{\"name\":\"bF1\",\"type\":[\"null\",\"boolean\"],\"doc\":\"default could be only null, so is redundant here\",\"default\":null},{\"name\":\"bF2\",\"type\":[\"boolean\",\"null\"],\"doc\":\"default is boolean , so it could be true or false\",\"default\":true}]},\"doc\":\"head:   some very relevant doc\"},{\"name\":\"first\",\"type\":\"long\",\"doc\":\"first:  some very relevant doc\"},{\"name\":\"second\",\"type\":\"double\",\"doc\":\"second: some very relevant doc\"},{\"name\":\"third\",\"type\":[\"null\",{\"type\":\"string\",\"avro.java.string\":\"String\"}],\"doc\":\"third:  some very relevant doc\",\"default\":null},{\"name\":\"fourth\",\"type\":[\"null\",{\"type\":\"enum\",\"name\":\"EnumTypeOne\",\"symbols\":[\"ONE\",\"TWO\",\"THREE\",\"FOUR\"]}],\"doc\":\"forth:  some very relevant doc\",\"default\":null},{\"name\":\"tail\",\"type\":[\"null\",{\"type\":\"record\",\"name\":\"TailRecord\",\"fields\":[{\"name\":\"sF\",\"type\":[\"null\",{\"type\":\"string\",\"avro.java.string\":\"String\"}],\"doc\":\"some very relevant doc\",\"default\":null},{\"name\":\"lF\",\"type\":[\"null\",\"long\"],\"doc\":\"some very relevant doc\",\"default\":null}]}],\"doc\":\"tail:   some very relevant doc\",\"default\":null}]}"
    }
  
Besides command line tools, there is a [gui app](../lenses.md) to work with schemas.

In this example we already created two topics: _DEV_FIRST_MSG_T_ & _DEV_SECOND_MSG_T_, and inserted a single message of the corresponding type into each of them.
 
One can read avro-encoded messages then using _kafka-avro-console-consumer_.

From the _DEV_FIRST_MSG_T_:

    zconsult@kafkaqa2:~$ /opt/kafka/bin/kafka-avro-console-consumer --bootstrap-server localhost:9092 --property schema.registry.url=http://kafkaqa1:8081 --topic DEV_FIRST_MSG_T --from-beginning --group AC
    {"head":{"dtF":1,"sF":{"string":"header: sF value that could be null"},"lF":5,"bF1":null,"bF2":null},"first":1,"second":1.1,"third":{"string":"third filed value: "},"forth":{"com.zgconsultants.avro.schema.EnumTypeOne":"TWO"},"tail":{"com.zgconsultants.avro.schema.TailRecord":{"sF":{"string":"tail: sF value that could be null"},"lF":{"long":10}}}}

From the _DEV_SECOND_MSG_T_:

    zconsult@kafkaqa2:~$ /opt/kafka/bin/kafka-avro-console-consumer --bootstrap-server localhost:9092 --property schema.registry.url=http://kafkaqa1:8081 --topic DEV_SECOND_MSG_T --from-beginning --group AC
    {"head":{"dtF":1,"sF":{"string":"header: sF value that could be null"},"lF":5,"bF1":null,"bF2":null},"first":1,"second":1.1,"third":{"string":" some value"},"forth":{"com.zgconsultants.avro.schema.EnumTypeOne":"TWO"},"fifth":{"array":[1,2,3,4,5,6,7,8,9]},"tail":{"com.zgconsultants.avro.schema.TailRecord":{"sF":{"string":"tail: sF value that could be null"},"lF":{"long":10}}}}

### How it works

Let us elaborate what's going on behind the scenes. <a name="aeo_flink_three"/>

The message we sent had been generated by our _python_ code. Here is a slightly simplified version.  <a name="aeo_flink_three"/>.

```python
    records = []
    
    recordType = 'FirstMsg'

    i = 1
    h = {"dtF": i, "sF": "header: sF value that could be null", "lF": 5, "bF1": None, "bF2": None}
    t = {"sF": "tail: sF value that could be null", "lF": i * 10}
    array = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    r = {"head": h, "tail": t, "first": i, "second": i + 0.1, "third": " some value", "forth": 'TWO', "fifth": array}

    records.append(Record(recordType, r))
```

Python is not a strong typed language, so _it uses a dictionary to represent the avro record type_.
Java & C# are strongly typed; A comlete java code for the [HeadRecord is here](./head_record_java.md)
The main idea is that one gets a structured access to the fields in Java. C# implementation should be very similar.

How does it all work? In the first step, a python dictionary must be validated against the proper schema. But how does it know which schema to use? The answer is given by the _recordType = 'FirstMsg'_ string in our python code. That is not really a type (python doesn't have any) it is a [_subject name_](#aeo_blink_one) we registered our schema with. In the second step, using this schema it serializes python dictionary into an avro binary message and sends it. Unlike google protobuf for example, an avro message is not self described, in other words, it doesn't have a complete information about its structure inside itself, only a schema-id, which makes fora smaller payload, but requires a way, prodived by schema registry in our case, to fetch the schema by its id.)

When the _kafka-avro-console-consumer_ reads the message, it checks schema-id in its cache. If it is the _first time it recieves a message with this schema-id_, it fetches the schema from schema registry, validates it and creates a proper structure. 

We omit some details related to the _schema-versioning_ from this description.

### Final note  <a name="aeo_flink_fore"/>

The key point here is not about the "strongly- vs weakly-typed language" debate. It is about the benefits a distribute system gains from making its _structured_ and _described_:
* making language interop simple
* ability with third-party tools
* making  _message routing_ possible.
* making use of all the kafka-streams infrastructure.
* making use kafka connectors

One of the greates benefits of message buses like kafka is it greatly simplifies "layering and splitting" of system components, something not easily possible with the blob type of messages.
