---
description: >-
  This page describes loopchain (ICON1) and is there obsolete. We will provide a
  new architecture description for goloop in the future.
---

# Overview

## **Network Diagram of P-Rep Nodes**

![](../.gitbook/assets/1ba3122-2c531fb-prep-network-diagram.jpeg)

The diagram above illustrates how P-Rep nodes interact with each other and external dApps in the TestNet environment.

## **Inside a P-Rep Node**

**A process view of a P-Rep node**

There are five processes running inside a P-Rep node, namely `iconrpcserver`, `iconservice`, `loopchain`, `loop-queue`, and `loop-logger`.

![](../.gitbook/assets/02b20d0-5d03c7a-p-rep2.png)

**iconrpcserver**

* `iconrpcserver` handles JSON-RPC message requests
* ICON RPC Server receives request messages from external clients and sends back responses. When receiving a message, ICON RPC Server will identify the requested method and transfer the request to its corresponding component, in either loopchain or ICON Service. 

**iconservice**

* ICON Service manages the state of ICON network \(i.e., states of user accounts and SCOREs\) using LevelDB.
  * Before processing transactions, ICON Service performs syntax check on the request messages and pre-validates the status of accounts to see if the transactions are executable.

**loopchain**

* `loopchain` is the high-performance Blockchain Consensus & Network engine of ICON.

**loop-queue \(RabbitMQ\)**

* RabbitMQ is the most widely deployed open source message broker. 
* loopchain uses RabbitMQ as a message queue for inter-process communication. 

**loop-logger \(Fluentd\)**

* Fluentd is the open source data collector, which lets you unify data collection and consumption.
* Fluentd is included in the P-Rep node image. You can use Fluentd to systematically collect and aggregate the log data that other processes produce. 

### **P-Rep Node Port Usage**

**External Communications**

* TCP 7100: gRPC port used for peer-to-peer connection between peer nodes.
* TCP 9000: JSON-RPC or RESTful API port serving application requests.

**Internal communications**

* TCP 5672: RabbitMQ port for inter-process communication.

**RabbitMQ Management Console**

* TCP 15672: RabbitMQ Management will listen on port 15672. 
  * You can use RabbitMQ Management by enabling this port. It must be enabled before it is used.\(Check the `USE_MQ_ADMIN` environment variable\)
  * You can access the management web console at `http://{node-hostname}:15672/`

