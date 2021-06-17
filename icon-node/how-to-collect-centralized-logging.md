# How to collect centralized logging

This document is a guideline about How to collect centralized logging on the MainNet.

### Intended Audience

We recommend all P-Rep candidates to go through this guideline.

### Purpose

This section describes how to use `fluentd` to collect/ manage/ and analyze logs generated when a node is running.

P-Rep node will create a log file as noted below. The log file can be used to identify service operation information and cause of failure.

### Pre-requisites

We assume that you have previous knowledge and experience in:

* IT infrastructure management
* Linux or UNIX system administration
* Linux server and docker service troubleshooting
* Docker container

**Description of log files in prep-node**

| service name | log file name |
| :--- | :--- |
| docker container booting | booting.log |
| iconrpcserver | iconrpcserver.log |
| iconservice | iconservice.log |
| loopchain | loopchain.channel-txcreator-icon\_dex\_broadcast.icon\_dex.log |
|  | loopchain.channel-txcreator.icon\_dex.log |
|  | loopchain.channel-txreceiver.icon\_dex.log |
|  | loopchain.channel.icon\_dex.log |

The logs can be managed on the local disk. However, in order to manage the old log files and the large log files, the log should be kept in a separate space. In addition, multiple nodes may be operated depending on network modeling. It is recommended to collect and analyze logs from the central server rather than accessing the server each time to check the logs of multiple nodes.

**Logging architecture**

The below diagram shows the architecture of log collection/analysis method using Fluent, Elasticsearch, and Kibana.

