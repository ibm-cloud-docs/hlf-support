---

copyright:
  years: 2021
lastupdated: "2021-11-11"

keywords: smart contract, private data, private data collection, anchor peer, multicloud

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}



# Deploy a smart contract
{: #ibm-hlfsupport-console-smart-contracts}


A smart contract is the code, packaged as chaincode, that applications interact with to read and update data on the blockchain ledger. A smart contract turns business logic into an executable program that is agreed to and verified by all members of a blockchain network. This tutorial is the third part in the [sample network tutorial series](#ibm-hlfsupport-console-smart-contracts-structure) and describes how to deploy smart contracts to start transactions in the blockchain network.
{: shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network. Additionally, application developers might be interested in the sections that reference how to create a smart contract.

## Sample network tutorial series
{: #ibm-hlfsupport-console-smart-contracts-structure}

You are currently on the third part of our three-part tutorial series. This tutorial guides you through the process of using the console to deploy a smart contract onto a channel in your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network.

* [Build a network tutorial](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network) guides you through the process of hosting a network by creating an orderer and peer.
* [Join a network tutorial](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-join-network#ibm-hlfsupport-console-join-network) guides you through the process of joining an existing network by creating a peer and joining it to a channel.
* **Deploy a smart contract on the network** (Current tutorial) Provides information on how to write a smart contract and deploy it on your network.

You can use the steps in these tutorials to build a network with multiple organizations in one cluster for the purposes of development and testing. Use the **Build a network** tutorial if you want to form a blockchain consortium by creating an orderer node and adding organizations. Use the **Join a network** tutorial to connect a peer to the network. Following the tutorials with different consortium members helps you create a truly **distributed** blockchain network.

Tutorial for a v2.x configured channel:
- <img src="../images/2-x_Pill.png" alt="version 2.x" width="30" style="width:30px; border-style: none"/> [Channel application capabilities and peer Fabric images are 2.x](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v2)

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 only supports v2.2.3 Fabric images.
{: important}


