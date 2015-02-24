=============================================
Mongo Orchestration cluster integration tests
=============================================

The YAML and JSON files in this directory tree are platform-independent 
integration tests that drivers can use with the Mongo Orchestration service.

Format
------

Each YAML file has the following keys:

description
~~~~~~

A description of the test.

type
~~~~~~

The type of the test is "Standalone", "ReplicaSet", or "Sharded".

initConfig
~~~~~~~~~~~

A document to be used as the payload for the initial request to MO creating the resource.

clientSetUp
~~~~~~~~~~~

A document describing the configuration for the client used in the test. It has two keys:

- hosts
- options

'hosts' is an array of servers which have been set up in the initial Mongo Orchestration request. The hosts associated with each one of these servers should be provided to the client and can be determined from the response from the Mongo Orchestration initial setup request.
It's important that only the listed servers be provided as hosts for the initial client setup.

'options' are options for the client. The keys 'readPreference' and 'heartbeatFrequency' may be in this document.


phases
~~~~~~

A 'phase' is a list of actions to get the client into a state before the assertion. If any step fails, the test can be aborted.
Each document in the list has one top-level key. The key can be one of the following:

- MOOperation
- clientOperation
- wait

A MOOperation is a document describing a request to Mongo Orchestration. It has the following keys:

- method
- uri
- payload

The method is the type of http request (i.e. POST, GET, etc)
The uri is the relative link for the request.
The payload is the POST data that should be sent.

A clientOperation is an operation that should be done using the client set up in the beginning of the test. The document will have the following keys:

- operation
- doc (optional)
- outcome

The document will have an operation of type 'insertOne' or 'findOne'.
An 'insertOne' operation will have a key 'doc', supplying a document to be inserted. A 'findOne' operation will not have this key.

The 'outcome' doc has either a 1 or 0 value for 'ok'. This describes whether the operation is expected to be a success or failure.



tests
~~~~~~

A 'test' is a list of operations that should be asserted. A document in this list has one top-level key, either:

- clientOperation
- clientHosts

A clientOperation document has the same structure as the clientOperation document in the 'phases' list. The difference is that the outcome should be asserted and determines the success or failure of the test itself. In the phases list, a failed clientOperation should abort the test.

A clientHosts document has the keys:

- primary
- secondaries

The test should assert that the primary listed matches the client's primary.
The test should assert that the secondaries listed match the client's list of secondaries.






