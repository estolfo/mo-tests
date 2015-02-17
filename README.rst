=============================================
Mongo Orchestration cluster integration tests
=============================================

The YAML and JSON files in this directory tree are platform-independent 
integration tests that drivers can use with the Mongo Orchestration service.

Format
------

Each YAML file has the following keys:

- 

type
~~~~~~

The type of the test is "Standalone", "ReplicaSet", or "Sharded".

initConfig
~~~~~~~~~~~

A document used as the payload for the initial request to MO creating the resource.




phases
~~~~~~

A "phase" is a document with one top-level key. The key can be one of the following:

- MOOperation
- ClientOperation



**MOOperation**






