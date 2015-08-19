=============================================
Mongo Orchestration cluster integration tests
=============================================

The YAML and JSON files in this directory tree are platform-independent 
integration tests that drivers can use with the Mongo Orchestration service.

Converting to JSON
------------------

The tests are written in YAML because it is easier for humans to write and read,
and because YAML includes a standard comment format. A JSONified version of each
YAML file is included in this repository. Whenever you change the YAML,
re-convert to JSON. One method to convert to JSON is with
`jsonwidget-python <http://jsonwidget.org/wiki/Jsonwidget-python>`_::

    pip install PyYAML urwid jsonwidget    make
    make

Or instead of "make"::

    for i in `find . -iname '*.yml'`; do
        echo "${i%.*}"
        jwc yaml2json $i > ${i%.*}.json
    done

Alternatively, you can use `yamljs <https://www.npmjs.com/package/yamljs>`_::

    npm install -g yamljs
    yaml2json -s -p -r .

Format
------

Each YAML file has the following keys:

description
~~~~~~

A description of the test.

type
~~~~~~

The type of the test is "servers", "replica_sets", or "sharded_clusters". This can be used in the Mongo Orchestration setup URL.

initConfig
~~~~~~~~~~~

A document to be used as the payload for the initial request to Mongo Orchestration creating the resource.

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

A 'phase' is a list of actions to get the client into a state before the test assertion. If any step fails, the test should be aborted.
Each document in the list has one top-level key. The key can be one of the following:

- MOOperation
- clientOperation
- wait

A MOOperation is a document describing a request to Mongo Orchestration. It has the following keys:

- method
- uri
- payload

The method is the type of http request (i.e. POST, GET, etc).

The uri is the relative link for the request.

The payload is the POST data that should be sent.

A clientOperation is an operation that should be executed using the client instantiated in the beginning of the test. The document has the following keys:

- operation
- doc (optional)
- outcome

The document will have an operation of type 'insertOne' or 'find'.
An 'insertOne' operation will have a key 'doc', supplying a document to be inserted. A 'find' operation will not have this key.

The 'outcome' doc has either a 1 or 0 value for 'ok'. This describes whether the operation is expected to be a success or failure. This is not the test assertion but if this step fails, the test can be aborted.

The 'wait' document describes a number of seconds the test should pause. The step is intended to help avoid race conditions.

tests
~~~~~~

The 'tests' list is a series of operations that should be asserted to determine the success or failure of the test. A document in this list has one top-level key, either:

- clientOperation
- clientHosts

A clientOperation document has the same structure as the clientOperation document in the 'phases' list. The difference is that the outcome should be asserted and determines the success or failure of the test itself.

A clientHosts document has the keys:

- primary
- secondaries

The test should assert that the primary listed matches the client's primary.

The test should assert that the secondaries listed match the client's list of secondaries.






