---

copyright:
  years: 2021
lastupdated: "2021-11-11"

keywords: OpenShift, {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console, deploy, resource requirements, storage, parameters, multicloud

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}








# Getting started
{: #deploy-ocp-getting-started}

Deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 onto a Kubernetes cluster that is running on OpenShift Container Platform. The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric uses a [Kubernetes Operator](https://www.openshift.com/learn/topics/operators){: external} to install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console on your cluster and manage the deployment and your blockchain nodes. After the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console is running on your cluster, you can use the console to create blockchain nodes and operate a multicloud blockchain network.
{: shortdesc}

If you prefer to automate the installation of the service, check out the [Ansible Playbook](/docs/hlf-support?topic=hlf-support-ansible-install-ibp) that can be used to complete all of these steps for you.
{: tip}

## Resources required
{: #deploy-ocp-resources-required}

Ensure that your OpenShift cluster has sufficient resources for the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console and for the blockchain nodes that you create. The amount of resources that are required can vary depending on your infrastructure, network design, and performance requirements. To help you deploy a cluster of the appropriate size, the default CPU, memory, and storage requirements for each component type are provided in this table. Your actual resource allocations are visible in your blockchain console when you deploy a node and can be adjusted at deployment time or after deployment according to your business needs.

The resources for the CA, peer, and ordering nodes need to be multiplied by the number of these nodes that you require. The operator and console resources are per blockchain deployment. For example, if you deployed development, staging, and test networks in a single cluster, you need to have enough resources for three instances of the operator and console, one for each blockchain deployment. On the other hand, the webhook resources are per OpenShift cluster, only one instance is required, regardless of the number of blockchain networks in the cluster.

| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer (Hyperledger Fabric v2.x)**                       | 0.7           | 2.0                   | 200 (includes 100GB for peer and 100GB for state database)|
| **CA**                         | 0.1           | 0.2                   | 20                     |
| **Ordering node**              | 0.35          | 0.7                   | 100                    |
| **Operator**                   | 0.1           | 0.2                   | 0                      |
| **Console**                    | 1.2           | 2.4                   | 10                     |
| **Webhook**                    | 0.1           | 0.2                   | 0                      |

{: caption="Table 1. Default resource allocations" caption-side="bottom"}

** These values can vary slightly. Actual VPC allocations are visible in the blockchain console when a node is deployed.  

Note that when smart contracts are installed on peers that run a Fabric v2.x image, the smart contract is launched in its own pod instead of a separate container on the peer, which accounts for the smaller amount of resources required on the peer.

If you are deploying the OpenShift platform on {{site.data.keyword.cloud_notm}}, you must create a 4x16 cluster with multiple nodes. This cluster provides sufficient resources to get started by deploying a basic network. Note that one core in OpenShift is equivalent to one CPU (or vCPU) in {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.
{: important}

## Browsers
{: #deploy-ocp-browsers}
The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console has been successfully tested on the following browsers:

- Chrome Version 91.0.4472.114 (Official Build) (64-bit)
- Safari Version 14.1.1 (16611.2.7.1.4)

## Storage
{: #deploy-ocp-storage}

In addition to a small amount of storage (10 GB) required by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console, persistent storage is required for each CA, peer, and ordering node that you deploy. Because blockchain components do not use the Kubernetes node local storage, network-attached remote storage is required so that blockchain nodes can fail over to a different Kubernetes worker node in the event of a node outage.  And because you cannot change your storage type after deploying peer, CA, or ordering nodes, you need to decide the type of persistent storage that you want to use _before_ you deploy any blockchain nodes.

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console uses dynamic provisioning to allocate storage for each blockchain node that you deploy by using a pre-defined storage class. You can choose your persistent storage from the available Kubernetes storage options.

If the storage includes support for the `volumeBindingMode: WaitForFirstCustomer` setting, you should configure it to delay volume binding until the pod is scheduled. Read more in the [Kubernetes Storage Classes documentation](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode){: external}.
{: tip}

Before you deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you must create a storage class with enough backing storage for the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console and the nodes that you create. You can set this storage class to use the default storage class of your Kubernetes cluster or create a new class that is used by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console. If you are using a multizone cluster in {{site.data.keyword.cloud_notm}} and you change the default storage class definition, then you must configure the default storage class for each zone.  
After you create the storage class, run the following command to set the storage class of the multizone region to be the default storage class:
```
kubectl patch storageclass
```
{: codeblock}

If you prefer not to choose a persistent storage option, the default storage class of your namespace or OpenShift project is used. `Host-local` volume storage is not supported.
{: note}

### Considerations when choosing your persistent storage
{: ibm-hlfsupport-storage-considerations}

| **Consideration** | **Recommendations** |
|---------------|------|
| **Type** | Fabric supports three types of storage for your nodes: File, Block, and Portworx.  All three types support snapshots, which are important for backups and disaster recovery. Portworx is also useful when you have multiple zones in your cluster. Object storage is not supported by Fabric but can be used for backups. |
| **Performance**| When you choose your storage, you need to factor in the read/write speed (IOPS/GB). {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric suggests at least 2 IOPS/GB for a development or test environment and 4 IOPS/GB for production networks. Your results may vary depending on your use case and how your client application is written. |
| **Scalability ** | When a node runs out of storage, it ceases to operate. Even if Kubernetes attempts to redeploy the node elsewhere, when the storage is full, the node cannot operate. Because the ledger is immutable and can only grow over time, expandable storage is nice to have for blockchain networks whenever available. At some point, you will run out of storage on your peers and ordering nodes and expandable storage helps to avoid this situation. If the storage is not expandable, when a peer or ordering runs out of storage, you need to provision a new node with a larger storage capacity and then delete the old one. When the new node is deployed, it must replicate the ledger, which can take time depending on the depth of the block history. |
| **Monitoring** | It's critical to monitor the storage consumption by your nodes. Consider the scenario where you deploy five ordering nodes, all with the same amount of storage. They are all replicating the same ledgers so they will all run out of storage at approximately the same time and you will lose consensus, causing the network to stop functioning. Therefore, you might want to consider varying the storage across the nodes and monitoring it as the ledger grows to avoid this situation. Before storage is exhausted on a node, you can expand it or provision a new node. |
| **Encryption** | Fabric does not require storage to be encrypted but it is a best practice for Security. You need to decide whether encryption is important for your business. If you have the option of encrypting the persistent volume, there may be some performance implications with encryption to consider.  |
| **High Availability (HA)** | There should be redundancy in the storage to avoid a single point of failure. |
| **Multi-zone capable storage** | {{site.data.keyword.cloud_notm}} includes the ability to create a single Kubernetes cluster across multiple data centers or "zones". Portworx offers multi-zone capable storage that can be used to potentially reduce the number of peers required. Consider a scenario where you build two zones with two peers for redundancy, one zone can go down and you still have two peers up in another zone. With multi-zone capable storage, you could instead have two zones with one peer each. If one zone goes down, the peer comes up in the other zone with its storage intact, reducing the overall redundant peer requirements. |

### Supported and tested storages
{: ibm-hlfsupport-storage-supported-tested}

The following dynamic storage options for OpenShift clusters are now tested and supported by {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0.
* ibmc-block-bronze
* ibmc-block-custom
* ibmc-block-gold
* ibmc-block-silver
* ibmc-file-bronze
* ibmc-file-custom
* ibmc-file-gold
* ibmc-file-silver
* [portworx](https://portworx.com/)
* [rook-cephfs](https://github.com/rook/rook)

The storages listed above are maintained by respective providers and it is not recommend to use one over the other. It is the user's responsibility to manage its own storages.
{: note}

## Filesystem permissions
{: #deploy-ocp-fs-perm}

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric uses Kubernetes [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/){: externnal} to set the correct filesystem permissions for the mounted volumes. The init containers run as the root user. The volumes are mounted on the init container which then changes the owner of the mounted folder to be the non-root user of the container that will use it. Learn  more about [how to give non-root users write permission on the volume mount path](/docs/containers?topic=containers-nonroot){: external}.

## Choose your deployment option
{: #deploy-ocp-choose}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric can be deployed in four different ways depending on your business goals. Red Hat customers may prefer to deploy the service directly from the Red Hat Marketplace to their OpenShift cluster in the cloud or on-prem. If you prefer to step through the process manually, you can deploy it to your cloud or on-prem  behind a firewall. Finally, an Ansible playbook is available to automate the deployment of the service to your OpenShift cluster.



