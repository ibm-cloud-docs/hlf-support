---

copyright:
  years: 2019
lastupdated: "2021-08-11"

keywords: smart contract, private data, private data collection, anchor peer, multicloud

subcollection: blockchain-sw-252

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:release-note: data-hd-content-type='release-note'}
{:right: .ph data-hd-position='right'}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}



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

Select the tutorial that corresponds to your channel configuration:
- <img src="../images/2-x_Pill.png" alt="version 2.x" width="30" style="width:30px; border-style: none"/>*** [Channel application capabilities and peer Fabric images are 2.x](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v2)
- <img src="../images/1-4_Pill.png" alt="version 1.4" width="30" style="width:30px; border-style: none"/> [Channel application capabilities are 1.4 and peer Fabric images are 1.4 or 2.x](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v14)

*** Fabric v2.0 introduced a new distributed process to manage the lifecycle of a smart contract that allows for decentralizing the governance of smart contracts on a channel. Whenever possible, it is recommended that customers should move to using the new smart contract lifecycle to avoid any interruption of service in later upgrades when Fabric no longer supports the v1.4 process for installing and instantiating smart contracts.
{: important}
