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
- phases
- tests

The "setup" for the test has the following keys:

- MO-requests
- uriReferenceInResponse
- clientURI
- setUpOperation

The "MO-requests" are a list of requests that need to be made to Mongo Orchestration. Order is critical.
Each request has a subset of the following keys:

- method
- uri
- payload

A "payload" is specified if the request method is "POST".