# hyperledger-marriage-registration
This project will help you to design and build a marriage registration certificate on Hyperledger platform

Before I begin, Hyperledger Fabric only runs on Unix-based operating systems. As a result, it will not be able to run on Windows and you’ll have restrictions on what you can do. I suggest setting up a virtual machine if you are running Windows before continuing.

This article assumes some knowledge of Javascript. It isn’t a tutorial aimed at beginner programmers, but rather at programmers who are beginners in the blockchain space.

**What are we building?**

So, you want to build a blockchain application but have no idea where to start? Don’t worry. Through this tutorial, we will set up a Marraige Certificate network where two person get marraied on hyperledger platform ang get a marriage certificate.
We’ll also set up a REST API server to allow client side software to interact with our business network. Finally, we will also generate an **Angular 6** application which uses the REST API to interface with our network.

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


### Installing the prerequisites, tools, and a Fabric runtime

**1. Installing Prereqs**
Now that we have a high level understanding of what is needed to build these networks, we can start developing. Before we do that, though, we need to make sure we have the prerequisites installed on our system. An updated list can be found https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html

* Docker Engine and Docker Compose

* Nodejs and NPM

* Git

* Python 2.7.x

For Ubuntu users, Hyperledger has a bash script available to make this process extremely easy. Run the following commands in your terminal:

curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh

chmod u+x prereqs-ubuntu.sh

./prereqs-ubuntu.sh

Unfortunately, Mac users have to manually install the aforementioned tools and make sure they have all the prerequisites on their system. Please follow https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html

**2. Installing tools to ease development**

Run the following commands in your Terminal, and make sure you’re **NOT** using sudo when running npm commands.

npm install -g composer-cli

npm install -g composer-rest-server

npm install -g composer-playground

npm install -g yo generator-hyperledger-composer

**composer-cli** is the only essential package. The rest aren’t core components but will turn out to be extremely useful over time. We will learn more about what each of these do as we come across them.

**3. Installing a local Hyperledger Fabric runtime**

mkdir ~/fabric-dev-servers

cd ~/fabric-dev-servers

curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz

tar -xvf fabric-dev-servers.tar.gz

export FABRIC_VERSION=hlfv12

./downloadFabric.sh

./startFabric.sh

./createPeerAdminCard.sh

Let’s go through the commands and see what they mean. First, we make and enter a new directory. Then, we download and extract the tools required to install Hyperledger Fabric.

We then specify the version of Fabric we want, at the time of writing we need 1.2, hence hlfv12. Then, we download the fabric runtime and start it up.

Finally, we generate a PeerAdmin card. Participants in a Fabric network can have business network cards, analogous to real life business cards. As we mentioned before, Fabric is a base layer for private blockchains to build upon. The holder of the PeerAdmin business card has the authority to deploy, delete, and manage business networks on this Fabric runtime (aka YOU!)

If everything went well, you should see an output like this:

**Put the screenshot here**

Basically what we did here was just download and start a local Fabric network. We can stop is using **./stopFabric.sh** if we want to. At the end of our development session, we should run **./teardownFabric.sh**

**NOTE:** This local runtime **is meant to be frequently started, stopped, and torn down for development use.** For a runtime with more persistent state, you’ll want to deploy the network outside the dev environment. You can do this by running the network on Kubernetes or on managed platforms like IBM Blockchain. Still, you should go through this tutorial first to get an idea.

### Creating and deploying our business network

Remember the packages yo and generator-hyperledger-composer we installed earlier?

yo provides us a generator ecosystem where generators are plugins which can be run with the yo command. This is used to set up boilerplate sample applications for various projects. generator-hyperledger-composer is the Yo generator we will be using as it contains specs to generate boilerplate business networks among other things.

**1. Generating a business network**

Open terminal in a directory of choice and type yo hyperledger-composer

**screenshot**

**2. Modeling our business network**

The first and most important step towards making a business network is identifying the resources present. We have four resource types in the modeling language:

* Assets

* Participants

* Transactions

* Events

For our marraige-certificate-network , we will define an asset type MarraigeCertificate , a participant type Person , a transaction Marraige and an event MarraigeNotification.

