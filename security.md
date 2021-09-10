---

copyright:
  years: 2021
lastupdated: "2021-09-10"

keywords: security, encryption, storage, tls, iam, roles, keys, multicloud

subcollection: hlf-support

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



# Security
{: #ibm-hlfsupport-security}



{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric provides a scalable, highly reliable platform that helps customers deploy applications and data quickly and securely. This document provides information about securing your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric service instance, where the blockchain console runs, and best practices for securing the  Kubernetes cluster where the blockchain nodes are deployed.
{: shortdesc}

## Security on the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console
{: #ibm-hlfsupport-security-ibm-hlfsupport}

**Audience:** Tasks in this section are typically performed by **blockchain network operators**.  

 Configuration of an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network includes deploying the blockchain console that can then be used to create blockchain nodes that reside in the customer Kubernetes cluster. 

Considerations include:
- [IAM (Identity and Access Management)](#ibm-hlfsupport-security-ibm-hlfsupport-iam)
- [Ports](#ibm-hlfsupport-security-ibm-hlfsupport-ports)
- [Key management](#ibm-hlfsupport-security-ibm-hlfsupport-keys)
- [Membership service providers (MSPs)](#ibm-hlfsupport-security-ibm-hlfsupport-msp)
- [Access control lists (ACLs)](#ibm-hlfsupport-security-ibm-hlfsupport-acls)
- [API authentication](#ibm-hlfsupport-security-ibm-hlfsupport-apis)

### IAM (Identity and Access Management)
{: #ibm-hlfsupport-security-ibm-hlfsupport-iam}




Identity and access management allows the owner of a console to control which users can access to the console and their privileges within it. IAM is built into the blockchain console and includes local console authentication and role management. When the console is initially provisioned you need to specify the email address of the user who is designated as the console administrator, also known as the **Manager**. This administrator can then add and grant other users access to the console by using the **Users** tab. It is also possible to change the console administrator. Every user that accesses the console must be assigned an access policy with a user role defined. The policy determines what actions the user can perform within the console. Other users can be assigned with **Manager**, **Writer**, or **Reader** roles when a console manager adds them to the console. The definition of each role is provided in the [Role to permissions mapping table](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-role-mapping). For the steps required to add new users, see [Managing users from the console](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-users).

Note that users can also be managed with [APIs](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-apis#console-icp-manage-users-apis).


### Ports
{: #ibm-hlfsupport-security-ibm-hlfsupport-ports}




If you are using a client application to send requests to the console, either via the blockchain APIs or the Fabric SDKs, the standard `HTTPS` (443) port needs to be exposed in your firewall.


### Key management
{: #ibm-hlfsupport-security-ibm-hlfsupport-keys}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network is based on trusted identities. Customers use the Certificate Authorities (CAs) in the console to generate the identities and associated certificates that are required by all members to transact on the network. The generated public and private keys are `ECDSA` with Curve `P256`. These keys are stored in the browser when they are added to the member's blockchain wallet so that the console can use them to manage blockchain components. However, it is recommended that customers export these keys and import them into their own key management system in case they clear their browser cache or switch browsers. Customers are responsible for the storage, backup, and disaster recovery of all keys that they export.

Because these public and private key pairs are essential to how the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric functions, **key management** is a critical aspect of security. If a private key is compromised or lost, hostile actors might be able to access your data and functionality. Although you use the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console to generate the certificates and private keys, they are not _permanently_ stored by the browser or the cloud database. Public and private keys are temporarily stored in the browser and added to the member's wallet so that the console can use the private key to digitally sign transactions. Customers are ultimately responsible for exporting the keys and managing their storage, backup, and disaster recovery.

If a private key is lost and cannot be recovered, you will need to generate a new private key by registering and enrolling a new identity with your Certificate Authority. You should also then remove and replace your signCert in any components or organizations where you had used the lost or corrupted identity. See [Updating an organization MSP definition](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-organizations#ibm-hlfsupport-console-govern-update-msp) for detailed steps.

In order to secure private keys in a production environment, the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric includes the option to configure a [Hardware Security Module](#x6704988){: term} (HSM) to generate and store the private keys for a node. An HSM is an optional hardware appliance that performs cryptographic operations and ensures that the cryptographic keys never leave the HSM. You are responsible for configuring an HSM device that implements the [PKCS #11 standard](http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html){: external}.  PKCS #11 is a cryptographic standard for secure operations, generation, and storage of keys. You will also need to configure a PKCS #11 proxy to communicate with your HSM. The platform supports ECDSA Sign and Verify cryptographic controls and EC Key generation. When an HSM is implemented for a node, the private key for the node is not stored in the browser wallet. Rather, the private key is accessed from the HSM via the proxy. When you register other node admin or client application identities with a CA by using the console, their private keys are not stored inside the HSM because they need the private key to transact on the network. For instructions on how to use HSM with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, see [Configuring a node to use a Hardware Security Module (HSM)](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-cfg-hsm).

You also have the option to bring your own certificates from your own non-{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric CA when you create a peer node or ordering service. If you use your own certificates, you will need to manually build the peer or ordering service MSP definition file that includes those certificates and import the file into the console **Organizations** tab. See [Using certificates from an external CA with your peer or ordering node](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-third-party-ca) for the steps required.

### Membership Service Providers (MSPs)
{: #ibm-hlfsupport-security-ibm-hlfsupport-msp}

Whereas Certificate Authorities generate the certificates that represent identities, turning these identities into roles in the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric is done through the creation of Membership Service Providers (MSPs) in the console. These MSPs, which structurally are comprised of folders containing certificates, are used to represent organizations on the network. Every organization will have one and only one MSP and will always contain at least one **admincert** that identifies an administrator of the organization. When an MSP is associated with a peer, for example, it denotes that the peer belongs to that organization. Later on in the flow for creating a peer (or any node), this same administrator identity can be used to serve as the administrator of the peer as well. In order to perform some actions on a node, an administrator role is required. For example, to be able to install a smart contract on a peer, your public key must exist in the `admincerts` folder of the peer's organization MSP, which therefore makes you an administrator of the peer organization.

MSPs also identify the root CA that generated the certificates for the organization and any other roles beyond administrator that are associated with the organization (for example, members of a sub-organizational group), as well as setting the basis for defining access privileges in the context of a network and channel (e.g., channel admins, readers, writers).

MSP folders for organization members are based on a Fabric defined structure and are used by Fabric components. For more information about Fabric MSPs and their structure, see the  [Membership](https://hyperledger-fabric.readthedocs.io/en/release-2.2/membership/membership.html){: external} and [Membership Service Provider Structure](https://hyperledger-fabric.readthedocs.io/en/release-2.2/membership/membership.html#msp-structure){: external} topics in the Hyperledger Fabric documentation. The Fabric CA establishes this structure by creating the following folders inside the MSP definition:

| MSP folder name | Description |
|-------------------------|-------------|
| `cacerts` | Contains the certificate of the root CA of your network.|
| `intermediatecerts` | These are the certificates of any intermediate CAs in your chain of trust (leading back to a root CA or CAs). Because intermediate CAs are currently not supported by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, this field will be blank.|
| `keystore` | Contains the private key that was generated alongside your public key. This key is used to generate signatures by creating a cryptographic hash that can be verified using the public key known to other users. This key is never shared, and you are responsible for securing and managing it. If this key becomes compromised, it can be used to impersonate your identity, making it crucial to keep this key safe. |
| `signCerts`| Contains the public key that was generated alongside your private key. It is also known as a "signing certificate" because it is used to verify signatures generated by other users.|
| Many Fabric components contain additional information inside their MSP folder. For example, a peer, includes the following folders: ||
| `admincerts` | Contains the admin certificates for this organization or component. |
| `tls` | Contains the TLS certs that you store for communicating with other network components. |
{: caption="Table 1. MSP folders" caption-side="bottom"}

Note that organization MSPs are stored in browser storage and must be exported to a local file system and secured by the customer.

### Access control lists (ACLs)
{: #ibm-hlfsupport-security-ibm-hlfsupport-acls}

Hyperledger Fabric allows for finer grained control over user access to specified resources through the use of access control lists (ACLs). ACLs allow access to a channel resource to be restricted to an organization and a role within that organization. The available set of ACLs are from the underlying Fabric architecture and are selected during channel creation or update. Note that access control lists are restrictive, rather than additive. If access to a resource is specified to an organization, it means that **only that organization** will have access to the resource. For example, if the default access to a particular resource is the Readers of all organizations, and that access is changed to the Admin of Org1, then only the Admin of Org1 will have access to the resource. Because access to certain resources is fundamental to the smooth operation of a channel, it is highly recommended to make access control decisions carefully. If you decide to limit access to a resource, make sure that the access to that resource is added, as needed, for each organization.

You can use the blockchain console to select which ACLs to apply to resources on a channel. See this information under [Creating a channel](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network-channels-create) for instructions on how to configure access control for a channel.

### API authentication
{: #ibm-hlfsupport-security-ibm-hlfsupport-apis}

In order to use the blockchain [APIs](https://cloud.ibm.com/apidocs/blockchain){: external} to create and manage network components, your application needs to be able to authenticate and connect to your network.   See this topic on how to [Connect to your console using API keys](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-apis#console-icp-manage-api-key) for more details.

## Best practices for security on the customer Kubernetes cluster
{: #ibm-hlfsupport-security-Kubernetes}

**Audience:** Tasks in this section are typically performed by **Kubernetes infrastructure managers**.

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console allows you to deploy and manage nodes on a Kubernetes cluster that you operate. The previous section addressed the security of the console. The following sections detail the best practices you can use to secure your Kubernetes cluster and the nodes of your network:

- [Kubernetes cluster security](#ibm-hlfsupport-security-Kubernetes-security)
- [Network security](#ibm-hlfsupport-security-Kubernetes-network)
- [Cluster and Operating System (OS)](#ibm-hlfsupport-security-Kubernetes-container-os)
- [Keys and cluster access information](#ibm-hlfsupport-security-Kubernetes-keys)
- [Membership Service Providers (MSPs)](#ibm-hlfsupport-security-kubernetes-msp)
- [Storage](#ibm-hlfsupport-security-kubernetes-storage)
- [Data privacy](#ibm-hlfsupport-security-kubernetes-privacy)
- [GDPR](#ibm-hlfsupport-security-kubernetes-gdpr)

### Kubernetes cluster security
{: #ibm-hlfsupport-security-Kubernetes-security}

The best place to start is to learn about the security features of the underlying Kubernetes infrastructure. The open source documentation provides a review of recommended practices for [securing a Kubernetes cluster](https://Kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/){: external}.




For OpenShift Container Platform security considerations, you should review the [Red Hat Container Security Guide Service](https://access.redhat.com/documentation/en-us/openshift_container_platform/3.11/html-single/container_security_guide/index){: external}. You will need to use [security context constraints](https://access.redhat.com/documentation/en-us/openshift_container_platform/3.11/html-single/architecture/index#security-context-constraints){: external} (SCCs) to define a set of conditions that a pod must run with in order to be accepted into the system. Details are included in the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment instructions.  

If you are running on Azure Kubernetes Service, Amazon Elastic Kubernetes Service, or IBM Kubernetes Service, then you need to set up the NGINX Ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}


### Network security
{: #ibm-hlfsupport-security-Kubernetes-network}




The Kubernetes Container Platform provides the underlying network, including the networks and routers, over which the customer's VLAN resides. The customer needs to configure their servers and use gateways and firewalls to route traffic between servers to protect workloads from network threats. Protecting your cloud network by using firewalls and intrusion prevention system devices is imperative for protecting your cloud-based workloads.




### Cluster and Operating System security
{: #ibm-hlfsupport-security-Kubernetes-container-os}

- **Sensitive data:** Cluster configuration data is stored in the `etcd` component of your Kubernetes master. Data in `etcd` is stored on the local disk of the Kubernetes master and is backed up to {{site.data.keyword.cos_full_notm}}. Data is encrypted during transit to {{site.data.keyword.cos_full_notm}}. If you are using OpenShift, you can choose to enable encryption for your etcd data on the local disk of your Kubernetes master by [Encrypting Data at the datastore layer](https://docs.openshift.com/container-platform/3.11/admin_guide/encrypting_data.html){: external} for your cluster.


- **UBI Linux:** The Fabric Docker images use [Red Hat UBI images](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/building_running_and_managing_containers/index#how_are_ubi_images_different/){: external}, which is a smaller, lighter, and more secure version of Linux.

### Keys and cluster access information
{: #ibm-hlfsupport-security-Kubernetes-keys}


| Type | Description |Storage | Access |
|------|-------------|--------|--------|
|**Customer Kubernetes cluster access information**| In order for the service components to access and manage the components that are deployed into this cluster, the service components generate a Kubernetes secret for **cluster** access. Each Kubernetes secret is tied to that specific customer cluster. | After generation, these secrets never leave the IBM Kubernetes cluster where the service components are running and are deleted whenever a customer deletes the associated cluster. | Accessed only by the service component code and only in response to customer requests to deploy or manage components via the blockchain console. Developers of the service do not have access to the information in the secrets. |
{: caption="Table 2. Types of keys and information used to access the customer Kubernetes cluster" caption-side="top"}

### Membership Service Providers (MSPs)
{: #ibm-hlfsupport-security-kubernetes-msp}

Organizations in a blockchain network are represented by [MSP](/docs/hlf-support?topic=hlf-support-glossary#glossary-msp) definitions. You can use the blockchain console to add a new organization to the network by creating a new MSP definition in the **Organizations** tab. If you are the admin of an ordering service, you can use the console to add that organization MSP to a consortium using the **Ordering service** tab. Finally, if you are an administrator of a channel, you can add that organization to an existing channel so the organization can transact on the network using the **Channels** tab (this task might require the signature of other organizations). See the topic on [Join the consortium hosted by the ordering service](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network-add-org) for detailed steps.

### Storage
{: #ibm-hlfsupport-security-kubernetes-storage}

When the blockchain console deploys a node, storage is dynamically provisioned from the default storageclass for that node from persistent storage. You have the option of encrypting the persistent volume but there may be some performance implications with encryption to consider.  

Customers are responsible for encrypting their own storage and the encryption must occur before any blockchain components are deployed to the cluster.
{: important}




- For more information on securing your persistent storage on OpenShift, see this topic on [Volume Security](https://docs.openshift.com/container-platform/3.11/install_config/persistent_storage/pod_security_context.html){: external}.


### Data privacy
{: #ibm-hlfsupport-security-kubernetes-privacy}

When you have data privacy requirements, [Private Data collections](https://hyperledger-fabric.readthedocs.io/en/release-2.2/private-data/private-data.html#what-is-private-data){: external} provide a way to further isolate specific data from the rest of the channel members. The combination of the use of channels and private data offer various solutions for achieving [Data Residency](/docs/hlf-support?topic=hlf-support-console-icp-about-data-residency).

### GDPR
{: #ibm-hlfsupport-security-kubernetes-gdpr}

In order to be GDPR compliant, it is recommended that you store PII data off chain.

## Hyperledger Fabric Security
{: #ibm-hlfsupport-security-kubernetes-fabric}

Because {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric is based on Hyperledger Fabric, you can leverage the secure features included in a Fabric network.  

- **TLS v1.2 communications** TLS is embedded in the trust model of Hyperledger Fabric. By default, server-side TLS is enabled for all communications using TLS certificates. TLS is used to encrypt the communication between your nodes and between your nodes and your applications. TLS prevents man-in-the-middle and session hijacking attacks. All {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric components use TLS to communicate with each other.

- **Transaction integrity:** Fabric uses the cryptographic ECDSA standard to guarantee transaction integrity. With ECDSA, the transaction originator, such as a client application, signs their message by using their private key, and the recipient, such as a peer, uses the originator’s public key to verify the authenticity of the message. If a transaction is tampered with on its way to the recipient, the signature verification fails.

- **Channels:** While Fabric channels are a powerful mechanism for partitioning and isolating data, they also provide the primary foundation for data privacy. Only members of the same channel can access the data of this channel. To ensure channel security, the channel configuration update policy is configured to define the number of channel operators who need to agree on the channel update request before a channel can be updated. Therefore, a clear process must exist for defining the organizations that are allowed to join and update channel.

- **Ledger data:** Implicit in the blockchain permissioned network is the notion that an agreed upon policy of multiple endorsers is required to sign (approve) a transaction before it can be committed to the ledger. Before any information can be added to the ledger, a clear and well-established process for defining the ledger information must exist. Data on the ledger is immutable.

- **Smart contracts:** All smart contracts should be reviewed by channel members before they are installed and executed on peers in their organization. Likewise, all updates to smart contract should be reviewed before the updates are applied to a peer. See this topic on [Upgrading a smart contract](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v14#ibm-hlfsupport-console-smart-contracts-upgrade) for the steps that are required.

- **HSM integration:** After you have configured an HSM for your environment, you have the option of configuring your nodes to use the HSM to generate and store private keys. See [Configuring a CA, peer, or ordering node to use the HSM](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-cfg-hsm) for more information.
