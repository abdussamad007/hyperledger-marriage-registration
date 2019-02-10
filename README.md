# hyperledger-marriage-registration
This project will help you to design and build a marriage registration certificate on Hyperledger platform

**Table of Contents**

* Introduction to Hyperledger Fabric and related applications

* Installing the prerequisites, tools, and a Fabric runtime

* Creating and deploying our business network

* Testing our business network

* Generating a REST API server

* Generating an Angular application which uses the REST API

### Introduction to Hyperledger Fabric and related applications

![Platform diagram](https://github.com/abdussamad007/hyperledger-marriage-registration/blob/master/Hyperledger_platform_diagram.png)

Deployment Environment overview for Hyperledger

**Hyperledger Fabric** is an open source framework for making private (permissioned) blockchain business networks, where identities and roles of members are known to other members. The network built on fabric serves as the back-end, with a client-side application front-end. SDK’s are available for Nodejs and Java to build client applications, with Python and Golang support coming soon.

**Hyperledger Composer** is a set of Javascript based tools and scripts which simplify the creation of Hyperledger Fabric networks. Using these tools, we can generate a **business network archive (BNA)** for our network. Composer broadly covers these components:

* Business Network Archive (BNA)

* Composer Playground

* Composer REST Server

**Business Network Archive **--  Composer allows us to package a few different files and generate an archive which can then be deployed onto a Fabric network. To generate this archive, we need:

* **Network Model** —  A definition of the resources present in the network. These resources include Assets, Participants, and Transactions. We will come back to these later.

* **Business Logic** — Logic for the transaction functions

* **Access Control Limitations ** — Contains various rules which define the rights of different participants in the network. This includes, but is not limited to, defining what Assets the Participants can control.

* **Query File (optional)**  — A set of queries which can be run on the network. These can be thought of as similar to SQL queries. You can read more on queries on https://hyperledger.github.io/composer/latest/reference/query-language

**Composer Playground**  is a web based user interface that we can use to model and test our business network. Playground is good for modelling simple Proofs of Concept, as it uses the browser’s local storage to simulate the blockchain network. However, if we are running a local Fabric runtime and have deployed a network to it, we can also access that using Playground. In this case, Playground isn’t simulating the network, it’s communicating with the local Fabric runtime directly.

**Composer REST Server**  is a tool which allows us to generate a REST API server based on our business network definition. This API can be used by client applications and allows us to integrate non-blockchain applications in the network.
