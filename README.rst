=============================================
Mongo Orchestration cluster integration tests
=============================================

The YAML and JSON files in this directory tree are platform-independent 
integration tests that drivers can use with the Mongo Orchestration service.

Format
------

Each YAML file has the following keys:

- description
- setup
- steps
- tests

setup
~~~~~~

The "setup" for the test has the following keys:

- MO-requests
- uriReferenceInResponse
- clientURI
- setUpOperation

The "MO-requests" are a list of requests that need to be made to Mongo Orchestration. The order of these requests is critical. 

**The last request's response must be cached by the test code.**

Each request has a subset of the following keys:

- method
- uri
- payload

A "payload" is specified if the request method is "POST".

The key "uriReferenceInResponse" specifies the key name in the response from the *last* MO request that should be used as a base uri for a client to the MO resource.

A "setUpOperation" is a document defining an operation using the client that must be done before the test is run. It has a key "action" and optionally a key "doc". The action will be insert" or "findOne". The "doc" is the document that should be inserted if the action is "insert".

The "clientUri" key defines the uri, possibly with options, that should be used to create a client to the MO resource. It has the following keys:

- base
- options

The "base" key is the base uri and "options" is the string to concatenate with the base uri.

steps
~~~~~~

A "step" is a sequence of actions creating the context for a test. Order of these actions is critical. A step can be one of two types: a list of operations using MO or using the client. 

MO-operations

A document of "MO-operations" is an list describing operations to be performed using Mongo Orchestration. The document may have the following keys:

- member
- request

If the "member" key is present, the cached request response (see above in setup) should be referenced to get the relative link for that member.

The "request" document has the following keys:

- method
- rel-link (optional)
- uri (optional)
- payload (optional)

"method" is the http request type. If "rel-link" is present, the rel-link in the last response received from MO should be used as the uri. If "uri" is present, it should be used. "payload" is defined for POST requests.

client-operations

The "client-operations" key defines a list of actions using the client. It is either a "findOne" or "insert". An "insert" has a corresponding "doc" to insert. Both types have a "outcome" document defining whether the operation is expected to be a success or failure.

tests
~~~~~

The "tests" key defines an array of either operations performed with the client or checks on the client's topology. A test document will be one of two types. It may have a single key

- members

defining the expected members in the client's topology.
Otherwise, it will have two keys:

- operation
- outcome

An "operation" is either "findOne" or "insert". The "outcome" document describes whether the operation is expected to be a success or failure.





