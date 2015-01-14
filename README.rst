=============================================
Mongo Orchestration cluster integration tests
=============================================

The YAML and JSON files in this directory tree are platform-independent 
integration tests that drivers can use with the Mongo Orchestration service.

Format
------

Each YAML file has the following keys:

- description
- type
- init_config
- uriOptions
- setup
- phases

type
~~~~~~

The type of the test is "standalone", "replica_sets", or "sharded". This should be used as the uri when creating the Mongo Orchestration resource.

**Note that the test code must cache the response received after creating the MO resource.**

init_config
~~~~~~~~~~~

The "init_config" is a document used as the payload for the initial request to MO creating the resource.

uriOptions
~~~~~~~~~~

The "uriOptions" should be concatenated with the uri returned from the request creating the MO resource.

setup
~~~~~

There may be some operations required before the test phases are run. They are defined in the "setup" list. Each document could contain the following keys:

- action
- doc (optional)

"action" is either "insertOne" or "findOne".
If the action is "insertOne", the value at the key "doc" is the document that should be inserted. It is critical that the operations be executed in order.


phases
~~~~~~

A "phase" is a document with one top-level key. The key can be one of the following:

- MO-operations
- client-operations
- tests

Each key is a list of operations or tests.

**MO-operations**

A document of "MO-operations" is a list describing operations to be performed using the MO resource. The document may have the following keys:

- member
- request

If the "member" key is present, the cached request response (see above) should be referenced to get the relative link for that member.

The "request" document has the following keys:

- method
- rel-link (optional)
- uri (optional)
- payload (optional)

"method" is the http request type. If "rel-link" is present, the rel-link in the last response received from MO should be used as the uri. When there are multiple operations in the list, the response reference is always the last received.
If "uri" is present, it should be used.
"payload" is defined for POST requests.

**client-operations**

The "client-operations" key defines a list of actions using the client. The value of the "action" key is either "findOne" or "insert". An "insert" has a corresponding "doc" to insert.

**tests**

The "tests" key defines an array of either operations performed with the client or checks on the client's topology. A test document will be one of two types. It may have a single key

- members

defining the expected members in the client's topology.

Otherwise, it will have two keys:

- operation
- outcome

An "operation" is either "findOne" or "insert". The "outcome" document describes whether the operation is expected to be a success or failure.





