 ## Schema-Registry-GUI
 
 We use the following [schema-registry-ui](https://github.com/lensesio/schema-registry-ui).
 It gives a nice alternative to the plain _curl_ command.
 
 The built version is installed  on the _kafkaqamini1_ box in the _/home/zconsult/stage/schema-registry-ui_ directory.
 Please note that currently it could be used only from the local box.
 
 To use it one needs [npm](https://www.sitepoint.com/beginners-guide-node-package-manager/) installed.
 
 To build it "from scratch" please have a look at the [README](https://github.com/lensesio/schema-registry-ui/blob/master/README.md) file of that project.
 
Please note that after _"npm install"_ command from the README above one has to edit env.js file.

Our changes are: 

    diff --git a/env.js b/env.js
    index 667a22d..b33edc3 100644
    --- a/env.js
    +++ b/env.js
     @@ -2,9 +2,9 @@ var clusters = [
     {
     NAME: "prod",
     // Schema Registry service URL (i.e. http://localhost:8081)
    -    SCHEMA_REGISTRY: "http://localhost:8081", // https://schema-registry.demo.landoop.com
    +    SCHEMA_REGISTRY: "http://kafkaqa1:8081", // https://schema-registry.demo.landoop.com
     COLOR: "#141414", // optional
    -    readonlyMode: true // optional
    +    readonlyMode: false // optional
      },
      {
     NAME: "dev",

We changed only two lines: the path to our schema_registry service (which currently running on the _kafkaqa1_ box) and _readonlyMode_ to false.

To use it **run a web browser on the same box** (firefox on the _kafkaqadmin1_ box) and go to the http://localhost:8080/
