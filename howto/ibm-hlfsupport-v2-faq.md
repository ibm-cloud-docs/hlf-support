---

copyright:
  years: 2021
lastupdated: "2021-09-15"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions, multicloud

subcollection: hlf-support
content-type: faq

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



# FAQs
{: #ibm-hlfsupport-v2-faq}


**Hyperledger Fabric**
- [What is the value of using {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric over native Hyperledger Fabric?](#ibm-hlfsupport-v2-faq-v2-ibm-hlfsupport-Overview-1-7)
- [What version of Hyperledger Fabric is being used with {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?](#ibm-hlfsupport-v2-faq-v2-Hyperledger-Fabric-3-1)

**Planning for your network**

- [Where can a customer deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric and how will {{site.data.keyword.IBM_notm}} support those deployment environments?](#ibm-hlfsupport-v2-faq-sw-support)
- [What ports are used by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?](#ibm-hlfsupport-v2-ports)
- [How can I estimate the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric sizing requirements for my development, test, and production environments?](#ibm-hlfsupport-v2-faq-sizing)

**Deploying the platform or upgrading**
- [Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?](#ibm-hlfsupport-v2-faq-multicloud)
- [How can I find what version of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric that I am running?](#ibm-hlfsupport-v2-faq-version)
- [How do I get the latest Fabric version and Fabric functionalities on my {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network?](#ibm-hlfsupport-v2-faq-v2-fabric-upgrade)

**Blockchain components**
- [What database do the peers use for their ledger?](#ibm-hlfsupport-v2-faq-v2-ibm-hlfsupport-Overview-1-3)
- [What types of off-chain databases are supported with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?](#ibm-hlfsupport-v2-faq-offchain-db)
- [If service discovery is on, will an endorsement request be routed to any peer on the network?](#ibm-hlfsupport-v2-faq-service-discovery)
- [Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?](#ibm-hlfsupport-v2-faq-raft-tls)
- [How can I back up and restore components and networks?](#ibm-hlfsupport-v2-faq-backup-restore)
- [What benefits are available with the new smart contract lifecycle available on nodes and channels running on Fabric v2.x?](#ibm-hlfsupport-v2-faq-new-lifecycle)
- [How can I check and interpret the status of my components through the Kubernetes command line?](#ibm-hlfsupport-v2-faq-cr-status)

**Certificates**
- [Do you support using certificates from non-IBM Certificate Authorities (CAs)?](#ibm-hlfsupport-v2-faq-v2-external-certs)
- [What is the recommended way to manage private keys?](#ibm-hlfsupport-v2-faq-hsm)
- [Can I integrate my corporate LDAP server with the Certificate Authority (CA) in the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?](#ibm-hlfsupport-v2-faq-ldap)
- [What is the process for rotating certificates on a periodic basis?](#ibm-hlfsupport-v2-faq-cert-mgmt)

**Developing applications and smart contracts**
- [What languages are supported for smart contracts?](#ibm-hlfsupport-v2-faq-v2-ibm-hlfsupport-Overview-1-4)
- [What version of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric works with the Ansible collection?](#ibm-hlfsupport-v2-faq-ansible-version)
- [How do I get support for running the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric Ansible playbook?](#ibm-hlfsupport-v2-faq-ansible-support)
- [Do I need OpenShift to run CodeReady Workspace?](#ibm-hlfsupport-v2-faq-codeready-openshift)
- [How often do updates get rolled out for the CodeReady Workspace extension?](#ibm-hlfsupport-v2-faq-codeready-updates)
- [How can I test my smart contract that is running in CodeReady workspace?](#ibm-hlfsupport-v2-faq-test-smart-contracts)
- [How can I find the examples and tutorials within the VSCode extension?](#ibm-hlfsupport-v2-faq-vscode-tutorials)
- [Can the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric monitor the health of a client application?](#ibm-hlfsupport-v2-faq-mon-client-app)

**Monitoring your network**
- [Where does {{site.data.keyword.IBM_notm}} store the customer's logs and how long does {{site.data.keyword.IBM_notm}} keep the audit logs for the blockchain platform service?](#ibm-hlfsupport-v2-faq-customer-logs)
- [Do we have access to logging services and what logs are available to me?](#ibm-hlfsupport-v2-faq-v2-Logging-and-Monitoring-11-6)
- [Is there a best practice for monitoring my blockchain resources?](#ibm-hlfsupport-v2-faq-mon-res)

## What is the value of using {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric over native Hyperledger Fabric?
{: #ibm-hlfsupport-v2-faq-v2-ibm-hlfsupport-Overview-1-7}
{: faq}

Hyperledger Fabric is a powerful, versatile, pluggable, open source, distributed ledger technology capable of addressing a wide variety of use cases across many industries. {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric is {{site.data.keyword.IBM_notm}}'s commercial distribution of Hyperledger Fabric. A key benefit of the platform is that  {{site.data.keyword.IBM_notm}} tests the open source code for security vulnerabilities daily and provides 24x7x365 support with SLAs appropriate for production environments. The platform is the **commercial distribution of Hyperledger Fabric** and includes integrated tools that provide end to end features for developers and network operators to develop, test, operate, monitor, and govern Fabric components by using an intuitive console UI. Quickly deploy an instance and use the streamlined console UI to [build a network](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network), easily [deploy smart contracts](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts), [govern your components](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern-components), and [govern your channel](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern). Interested in APIs? See the [{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric API reference](https://cloud.ibm.com/apidocs/blockchain){: external}. With the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, it is easy to extend a basic network, work with multicloud solutions, and receive {{site.data.keyword.IBM_notm}} worldwide support when needed. Finally, the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric provides additional security benefits that are essential for running an enterprise-grade production network.



## What version of Hyperledger Fabric is being used with {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?
{: #ibm-hlfsupport-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}




Refer to the following table for information on which version of Fabric is used by each version of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric:

| {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric | Hyperledger Fabric |
|----------------------------------------------------| -------------------|
| 1.0.0 | v2.2.3 |

## Where can a customer deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric and how will {{site.data.keyword.IBM_notm}} support those deployment environments?
{: #ibm-hlfsupport-v2-faq-sw-support}

For an updated list of all the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment options check out the [Supported Platforms](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-prerequisites).

It is important to note that the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric support only spans across Hyperledger Fabric based component issues for customers who have purchased the offering. Some examples include assistance in Fabric upgrades, smart contract deployment, adding peers to channels, etc.

On the other hand, {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric will not provide deployment support for environments outside of the supported platform. Also it will be up to the customer to configure the network and address their own infrastructure related issues. Examples of these types of issues include failed deployment to a Kubernetes service, infrastructure capacity, custom firewall settings, etc.

## What ports are used by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?
{: #ibm-hlfsupport-v2-ports}
{: faq}

See the port information in the [Security topic](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-security#ibm-hlfsupport-security-ibm-hlfsupport-ports).

## How can I estimate the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric sizing requirements for my development, test, and production environments?
{: #ibm-hlfsupport-v2-faq-sizing}
{: faq}

After you understand how many CAs, peers, and ordering nodes are required, you can examine the default resource allocations table for  [OpenShift](/docs/hlf-support?topic=hlf-support-deploy-ocp-getting-started#deploy-ocp-resources-required) or [Kubernetes](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-resources-required) to get an approximate estimate of the CPUs (VPCs) required for your network.  You will need to buy licenses per VPC (CPU) based on expected usage. As a reminder, these are only sold per VPC so you should always round up the number of VPCs if you plan to use fractional parts. For example, if you estimate that you will need 11.2 VPCs then you should license 12 VPCs.












## Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?
{: #ibm-hlfsupport-v2-faq-multicloud}
{: faq}

You cannot currently deploy blockchain nodes to multiple hosted cloud providers. However, you can use your console to operate a distributed multicloud network by importing nodes deployed by using consoles on other clouds.

## How can I find what version of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric that I am running?
{: #ibm-hlfsupport-v2-faq-version}
{: faq}
{: support}

View the Support page by clicking the question mark icon <img src="../images/support-link.png" alt="Support link icon" width="30" style="width:30px; border-style: none"/> in the upper right corner of the page. The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric version is visible under the page heading.

## How do I get the latest Fabric version and Fabric functionalities on my {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network?
{: #ibm-hlfsupport-v2-faq-v2-fabric-upgrade}
{: faq}

Depending on the contents of a Fabric release and {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, it might only be necessary to upgrade nodes to get access to the latest features. However, often it is necessary to update channel configurations with the latest capabilities in order to get access to the latest Fabric features. In these cases, it is important to upgrade components and channels in a particular order:

1. Upgrade nodes to the latest Fabric versions. Note that nodes are always backward compatible with earlier versions and earlier capabilities. For more information about upgrading nodes, see [Upgrading to a new version of Fabric](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern-components#ibm-hlfsupport-console-govern-components-upgrade).
2. Update channels with any new channel capabilities. If you update capabilities to a capability level (such as 2.0) before upgrading nodes to the Fabric version corresponding to a capability, the node may crash. For more information, see [Capabilities](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern#ibm-hlfsupport-console-govern-capabilities).

If you are moving from v1.4.x to v2.x, you may have to update your smart contracts to conform to new smart contract lifecycle. For more information, see [Upgrading to a new version of Fabric](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v2#ibm-hlfsupport-console-smart-contracts-v2).
{: tip}

## What database do the peers use for their ledger?
{: #ibm-hlfsupport-v2-faq-v2-ibm-hlfsupport-Overview-1-3}
{: faq}
{: support}

You have the choice of either CouchDB or LevelDB when you configure your peer database. Because data is modeled differently in a Couch database than in a Level database, the peers in a channel must all use the same database type. See [LevelDB versus CouchDB](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-level-couch) to decide what is best for your business needs.

## What types of off-chain databases are supported with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?
{: #ibm-hlfsupport-v2-faq-offchain-db}
{: faq}

As a best practice it is recommended that you do not query the entire blockchain ledger for the purpose of aggregation or reporting. If you want to build a dashboard or collect large amounts of data as part of your application, you can query an off chain database that replicates the data from your blockchain network. This allows you to understand the data on the blockchain without degrading the performance of your network or disrupting transactions.

You can use block or smart contract events from your application to write transaction data to an off-chain database or analytics engine. For each block received, the block listener application would iterate through the block transactions and build a data store by using the key/value writes from each valid transaction's read-write set. The [Peer channel-based event services](https://hyperledger-fabric.readthedocs.io/en/release-2.2/peer_event_services.html#peer-channel-based-event-services) provide replayable events to ensure the integrity of downstream data stores. For an example of how you can use an event listener to write data to an external database, see the [Off chain data sample](https://github.com/hyperledger/fabric-samples/tree/release-1.4/off_chain_data) in the Fabric samples.

Blockchain solutions can use any RDBMS or NoSQL DB such as {{site.data.keyword.IBM_notm}} Cloudant for offchain data storage. Hyplerledger Fabric does not govern, interact with, or manage off-chain databases. In most cases, the off-chain database is used for reference data and non-transactional data. {{site.data.keyword.IBM_notm}} has successfully built blockchain products and solution accelerators with Hyperledger Fabric and NoSQL databases such as OrientDB.

## If service discovery is on, will an endorsement request be routed to any peer on the network?
{: #ibm-hlfsupport-v2-faq-service-discovery}
{: faq}

It depends on whether your endorsement policy is set to "ANY", in which any peer can sign an endorsement request, or whether the policy is bound directly to an organization's peers. The service discovery information provided by the peer supplies two pieces of information, `Layouts` and `EndorsersByGroup`. With these two pieces of data, the SDK has the ability to send requests to peers in different organizations that meet the endorsement policy requirements. The Node.js SDK provides default code that uses the `Layouts` and `EndorsersByGroup` and sends the requests to the appropriate peers to meet the endorsement policy requirements. This existing logic can be customized to meet the business needs.

## Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?
{: #ibm-hlfsupport-v2-faq-raft-tls}
{: faq}

Yes. The Raft ordering service nodes are configured to use TLS communication. TLS is embedded in the trust model of Hyperledger Fabric. By default, server-side TLS is enabled for all communications using TLS certificates. TLS is used to encrypt the communication between your nodes and as well as between your nodes and your applications. TLS prevents man-in-the-middle and session hijacking attacks. All {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric components use TLS to communicate with each other.

## How can I back up and restore components and networks?
{: #ibm-hlfsupport-v2-faq-backup-restore}
{: faq}

As with anything that is deployed to a Kubernetes-based cluster, backups of components and networks in the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric are a matter of backing up its [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external}. These volumes are where ledgers and other types of storage are mounted so they can be used by nodes.

As a result, "backing up" a component or a network is the process of saving a copy of the relevant persistent volumes, while "restoring" a component or network involves bringing up components and pointing them to these saved volumes. For more information, check out [Backing up and restoring components and networks](/docs/hlf-support?topic=hlf-support-backup-restore){: external}.

## What benefits are available with the new smart contract lifecycle available on nodes and channels running on Fabric v2.x?
{: #ibm-hlfsupport-v2-faq-new-lifecycle}
{: faq}

The new smart contract lifecycle allows channel members to collaborate in the decision making process about smart contracts like never before. Where previously smart contracts were instantiated on a channel by a single channel member and other organizations only had the ability to choose whether to install the smart contract, the new lifecycle allows organizations to propose, approve, and commit smart contracts at an organizational level.

This separation of concerns opens exciting new opportunities for collaborating organizations. For example, different organizations can install smart contracts on their peers that contain only the code relevant to their business role and make minor updates to these smart contracts where necessary, without needing to seek new approvals from other organizations.

For a tutorial on how this process is handled by the console, check out [Deploy a smart contract using Fabric v2.x](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v2).

For information about to take advantage of the new lifecycle when writing a smart contract, check out [Writing powerful smart contracts](/docs/hlf-support?topic=hlf-support-write-powerful-smart-contracts).

## How can I check and interpret the status of my components through the Kubernetes command line?
{: #ibm-hlfsupport-v2-faq-cr-status}
{: faq}

To check the status of your component, run the following command:
```
kubectl get <CUSTOM_RESOURCE_TYPE> <CUSTOM_RESOURCE_NAME> -n <NAMESPACE> -o yaml
```
{: codeblock}

- Replace `<CUSTOM_RESOURCE_TYPE>` with the custom resource type of your component (`ibpca`, `ibppeer`, `ibporderer`, or `ibpconsole`).
- Replace `<CUSTOM_RESOURCE_NAME>` with the name of your component.
- Replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace or OpenShift project.

The `spec.status` field will contain details of your component's status:
- `errorCode`
- `lastHeartbeatTime`: when the controller last reconciled the component
- `message`: long explanation of the status type
- `reason`: short explanation of the status
- `status`: "true" or "false" bsed on if status is valid
- `version`: the product (IBP) version of the component
- `versions`: the operand version of the component
- `type`: describes the current status of the component
  - **Deploying**: component pod(s) spinning up but not yet running and ready
  - **Deployed**: component pod(s) are running
  - **Precreated**: (specific to the Orderer) Orderer is waiting for the genesis block to be created
  - **Error**: component hit an error during reconcile, or a certificate has expired
  - **Warning**: one or more of the component's certificate will be expiring within 30 days (by default)
  - **Initializing**: component is being reconciled again due to spec updates in the pre-reconcile checks

## Do you support using certificates from non-IBM Certificate Authorities?
{: #ibm-hlfsupport-v2-faq-v2-external-certs}
{: faq}

Yes, you can bring your own certificates if they are issued by a CA that is X.509 compliant. The CA should sign by using ECDSA and the defaults should be set to use P256 curve. See this topic about [Using certificates from an external CA with your peer or ordering node](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-third-party-ca).

## What is the recommended way to manage private keys?
{: #ibm-hlfsupport-v2-faq-hsm}
{: faq}

Because private keys are not stored by the platform, users are responsible for downloading and securing their private key. Therefore, when a higher level of security is required for private keys, an HSM is recommended. An HSM is a hardware appliance that performs cryptographic operations and provides the capability to ensure that the cryptographic keys never leave the HSM. Hyperledger Fabric supports HSM devices that implement the PKCS #11 standard. PKCS #11 is a cryptographic standard for secure operations, generation, and storage of keys. See [Configuring a node to use a Hardware Security Module (HSM)](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-cfg-hsm) to learn more.

## Can I integrate my corporate LDAP server with the Certificate Authority (CA) in the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric?
{: #ibm-hlfsupport-v2-faq-ldap}
{: faq}

You cannot currently directly integrate your [LDAP](#x2481619){: term} server with the CA. However, you can use an external mechanism to generate X.509 certificates for the LDAP users. To use those certificates with a peer or ordering service, see these topics on [Using certs from an external CA for your peer or ordering service](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-third-party-ca) and
[Manually building an organization MSP](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-organizations#console-organizations-build-msp).  

Also, you cannot configure the blockchain console login authentication to use an LDAP user registry at this time.
{: note}

## What is the process for rotating certificates on a periodic basis?
{: #ibm-hlfsupport-v2-faq-cert-mgmt}
{: faq}
{: support}

Similar to how passwords need to be regularly updated, identity certificates need to be renewed, a process also referred to as "certificate rotation". The platform displays certificate expiration dates for components throughout the console.  When a certificate expires, transactions on the network will fail because the identity can no longer be trusted.  It is your responsibility to monitor those expiration dates and manage your certificate renewal accordingly. The process varies depending on the type of certificate, when it was generated, and for organization admin certificates, whether Node OU support was enabled on the MSP when the identity was enrolled. The platform attempts to renew the peer and ordering node enrollment certificates 30 days before they expire. See [Managing certificates](/docs/hlf-support?topic=hlf-support-cert-mgmt) to learn more about the types of certificates that you need to monitor and how to renew them.

## What languages are supported for smart contracts?
{: #ibm-hlfsupport-v2-faq-v2-ibm-hlfsupport-Overview-1-4}
{: faq}
{: support}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric supports smart contracts that are written in Node.js, Golang (Go), JavaScript, and Java. The new Hyperledger Fabric programming model currently supports JavaScript, TypeScript, Java, and Go. If you are interested in preserving your existing application code, or by using Fabric SDKs for *Go*, you can still connect to your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network by using the lower-level Fabric SDK APIs.

## What version of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric works with the Ansible collection?
{: #ibm-hlfsupport-v2-faq-ansible-version}
{: faq}

Versions 1.0.0 of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric can be used with the Ansible collection to deploy a Hyperledger Fabric network.

## How do I get support for running the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric Ansible playbook?
{: #ibm-hlfsupport-v2-faq-ansible-support}
{: faq}

Ansible is an open source technology and this product is not officially supported by {{site.data.keyword.IBM_notm}}. For support related to the usage of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric and Ansible playbooks use the [GitHub repository](https://github.com/IBM-Blockchain/ansible-collection/issues){: external}.

## Do I need OpenShift to run CodeReady Workspace?
{: #ibm-hlfsupport-v2-faq-codeready-openshift}
{: faq}

Yes, OpenShift is a prerequisite to running CodeReady because you have to deploy your workspace into an OpenShift cluster.

## How often do updates get rolled out for the CodeReady Workspace extension?
{: #ibm-hlfsupport-v2-faq-codeready-updates}
{: faq}

Updates are scheduled to coincide with the VS Code extension and should be available every two weeks.

## How can I test my smart contract that is running in CodeReady workspace?
{: #ibm-hlfsupport-v2-faq-test-smart-contracts}
{: faq}

Currently this can be achieved by connecting to a Fabric network running outside of the CodeReady Workspaces extension. In a subsequent release we plan to introduce a simple way to deploy a Fabric network from within the extension.

## How can I find the examples and tutorials within the VSCode extension?
{: #ibm-hlfsupport-v2-faq-vscode-tutorials}
{: faq}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric extension provides guided tutorials within VS Code to help you get started. You can find these tutorials on the extension home page when the extension is first installed. However, you can return to the tutorials and the home page by going to the extensions tab. For more information, see [Guided tutorials in VS Code](/docs/hlf-support?topic=hlf-support-develop-vscode#develop-vscode-guided-tutorials).

## Can the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric monitor the health of a client application?
{: #ibm-hlfsupport-v2-faq-mon-client-app}
{: faq}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console does not monitor the health of blockchain client applications, but {{site.data.keyword.cloud_notm}} does offer tooling such as [{{site.data.keyword.la_full_notm}}](/catalog/services/ibm-log-analysis){: external} and [{{site.data.keyword.mon_full_notm}}](/catalog/services/ibm-cloud-monitoring){: external} that can be used for their health monitoring.

## Where does {{site.data.keyword.IBM_notm}} store the customer's logs and how long does {{site.data.keyword.IBM_notm}} keep the audit logs for the blockchain platform service?
{: #ibm-hlfsupport-v2-faq-customer-logs}
{: faq}

The logs are stored in the customer's Kubernetes cluster. {{site.data.keyword.IBM_notm}} does not have access to the logs and it is up to the customer to manage all of their log data including retention management.

## Do we have access to logging services and what logs are available to me?
{: #ibm-hlfsupport-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

With {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you can now directly access logs from your Kubernetes dashboard. It is recommend that you take advantage of the {{site.data.keyword.la_full_notm}} service that allows you to easily parse the logs in real time.

## Is there a best practice for monitoring my blockchain resources?
{: #ibm-hlfsupport-v2-faq-mon-res}
{: faq}
{: support}

You are responsible for the health monitoring and resource allocation of the blockchain nodes in your Kubernetes cluster. While requests against the nodes are being actively processed, you should be monitoring for spikes in resource consumption to avoid problems. You can configure a monitoring tool, such as [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-service-connection#network_alert_subnets){: external} with alert notifications for the nodes in your cluster.

You should be aware that JavaScript and TypeScript smart contracts require more resources than contracts written in Golang. Therefore, when you are allocating resources to your cluster, it is important to ensure adequate resources are available to your smart contract pods when they are deployed on a channel and during transaction processing. The pods containing the smart contracts will consume as much resources as they need to function.
{: tip}





