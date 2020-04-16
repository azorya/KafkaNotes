 ## Schema-Registry-GUI
 
On a basic level the regirstry is managed via a set of plain _curl_ commands.  In this project we install the following [schema-registry-ui](https://github.com/lensesio/schema-registry-ui) as user-friendly alternative.
 
 The built version is installed  on the _kafkaqamini1_ box in the _/home/zconsult/stage/schema-registry-ui_ directory.
 Please note that currently it could be used only from the local box.
 
 To use it one needs [npm](https://www.sitepoint.com/beginners-guide-node-package-manager/) installed.
 On the _kafkaqamini1_ we have it in the ~./apps/node_js directory. 
 
    zconsult@kafkaqamini1:~/apps/node_js/bin$ ls
    node  npm  npx
 
 To build it "from scratch" please have a look at the [README](https://github.com/lensesio/schema-registry-ui/blob/master/README.md) file of that project.
 
Note that after _"npm install"_ command from the README above one has to edit env.js file.

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

To start it 
1. cd _zconsult@kafkaqamini1:~/stage/schema-registry-ui_
2. run _npm start_

To use it **run a web browser on the same box** (firefox on the _kafkaqadmin1_ box) and go to the http://localhost:8080/
