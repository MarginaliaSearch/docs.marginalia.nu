---
title: '1.5 System Overview'
---

The search engine consists of several components, each of which is run in a separate Docker container.  

## Index Nodes 

The system is designed to be able to run with multiple partitions.  At least one partition is required, but more can be added.  Each partition is called an `Index Node`.  Each index node is a separate Docker container, and can be run on a separate server, or on the same server.  

Each domain is assigned to a specific index node.  This is called node affinity.  This will ensure that all data for a specific domain is stored on the same index node.  

For each index node, two services are run, an index-service, and an executor-service.  The index-service is responsible for indexing data, and the executor-service is responsible for executing control tasks.  The two services are run in separate Docker containers, but are tightly coupled. 

Nodes self-configure upon start-up.  The default docker-compose file will start with two nodes, but more can be added by editing the docker-compose file. 

Index nodes are configurable in the control interface.  You can configure them to be included or excluded from answering queries, and whether to retain intermediate data or not. 

## Query Service

The query service is responsible for parsing queries and executing them against the index nodes.  It is a stateless service.  It is also responsible for serving the search interface when running in a barebones configuration.

The query service is configured to run on port 8080 by default, and can be accessed at [http://localhost:8080/](http://localhost:8080/).  The search endpoint will answer both HTML and JSON depending on the `Accept` header.  


Apart from the 'q' parameter, the search endpoint also accepts the following parameters:

* `count` - The number of results to return.
* `set` - The ranking set to use (see the section on ranking sets)

A query can be executed by sending a GET request to the search endpoint with the query in the 'q' parameter.  For example, to search for 'marginalia', you can use the following curl command:

```bash
$ curl -H 'Accept: application/json' 'http://localhost:8080/search?q=marginalia&count=5'
```

## Control Service

The control service is responsible for the control interface, and for executing some control tasks.   By default, it is configured to run on port 8081, and can be accessed at [http://localhost:8081/](http://localhost:8081/). 

The control interface does not offer any authentication, and should not be exposed to the public Internet as-is.  Use a reverse proxy with authentication if you need to expose it.

## Database - MariaDB

The database is used to store supplemental data for the search engine.  This primarily consists of the domain database and various administrative tables.  Most of the search-engine related data is stored outside of the database for performance reasons.

## Reverse Proxy - Trafeik

The supplied docker-compose file uses Trafeik as a reverse proxy.  This is used to route traffic to the various services.   This can be replaced with other solutions, such as nginx.  If you do so, you will need to modify the docker-compose file accordingly.

If you want to use something else, inbound external traffic needs to have the header `X-Public: 1` set.  Traffic with this header will only be permitted to endpoints prefixed by `/public`.  This is used to prevent a misconfigured reverse proxy from accidentally exposing the internal APIs to the public Internet.  As long as the reverse proxy is adding this header, it should be fine.

In nginx, something like this is required:

```
proxy_set_header X-Public 1;
location / {
    proxy_pass http://service:80/public;
}
```