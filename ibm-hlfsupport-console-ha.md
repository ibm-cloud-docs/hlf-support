---

copyright:
  years:  2022
lastupdated: "2022-07-08"

keywords: high availability, HA, failures, zone failure, region failure, component failure, worker node failure, multicloud

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}





Documentation for this on-prem product has been moved from IBM Cloud to IBM Documentation at [https://www.ibm.com/docs/en/SSGWM34_1.0.0/ibm-hlfsupport-console-ha.html](https://www.ibm.com/docs/en/SSGWM34_1.0.0/ibm-hlfsupport-console-ha.html){: external}. Update your bookmarks.
{: important}

# High availability (HA)
{: #ibm-hlfsupport-console-ha}



Use the built-in Kubernetes features along with {{site.data.keyword.blockchainfull}} Platform component deployment strategies to make your blockchain networks more highly available and protect your network from downtime when a failure occurs in your cluster.
{: shortdesc}

**Target audience:** This topic is designed for architects and system administrators who are responsible for planning and configuring {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric on a Kubernetes cluster.

High availability is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure. For example, you can prepare for the failure of one system by adding redundancy and setting up failover mechanisms.

You can achieve high availability on different levels in your IT infrastructure and within different layers of your cluster. The level of availability that is right for you depends on several factors, such as your business requirements, the Service Level Agreements that you have with your organizations, and the cost of redundancy.


Before proceeding, we recommend that you review the platform-specific guidance for HA:

- If you are running on OpenShift Container Platform, you should review the HA recommendations in the OpenShift Container Platform documentation.


Then, you can use this topic for details on blockchain-specific HA guidance along with the recommendations from the platform-specific topic above.

## Overview of potential points of failure in {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric
{: #ibm-hlfsupport-console-ha-points-of-failure-overview}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric architecture is designed to ensure reliability, low latency processing, and a maximum uptime of the service. However, failures can happen. {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric provides several approaches to add more availability to your cluster by adding redundancy and [anti-affinity](https://www.ibm.com/blogs/cloud-archive/2016/07/ibm-containers-anti-affinity/){: external} policies. Anti-affinity ensures that blockchain components of the same type and organization are deployed across different worker nodes.  By adding redundancy across your blockchain network, you can avoid failures or downtime.  

To achieve maximum high availability, it is recommended that you build redundancy by provisioning peers and orderers in Kubernetes clusters in multiple regions. When the components are spread across regions and the blockchain ledger is distributed across those components, a failure in any single region will not impact processing of transactions. CAs are less critical for daily transaction processing. After all the users have been registered and enrolled with the CA, it is no longer required until the next time those services are required.

### Peer considerations
{: #ibm-hlfsupport-console-ha-peers}

HA for peers means always having redundant peers, that is at least two peers available for each organization on the same channel to process requests from client applications. Multiple peers can be deployed to a single worker node, or spread across worker nodes, zones, or even regions. Whenever you deploy multiple peers and join them to the same channel, the peers act as HA pairs because the channel and the data are automatically synchronized across all peers in the channel.  By design, a blockchain network is meant to have multiple organizations that transact on the same channels.  Therefore, the common deployment model is that for any given channel, there are redundant peers for each organization spread across several organization account clusters that are all synchronizing data between each other.  Each organization can have a peer in their own cluster in any region. 

For even more robust HA coverage, you can stand up multiple clusters in multiple regions and deploy peers in all of them. However, if high performance is desired, care must be taken when distributing peers to ensure the latency and bandwidth between them is sufficient to achieve your performance targets.

**Anchor peers** on a channel facilitate cross-organization communication that is required for private data, [gossip](#x9829550){: term}, and service discovery to work. If only one anchor peer exists on a channel, and that peer becomes unavailable, the organizations are no longer connected and the cross-organization gossip is no longer possible. Therefore, when you create redundant peers for an organization, be sure to add redundant [anchor peers on the channel](/docs/blockchain?topic=blockchain-ibm-hlfsupport-console-govern#ibm-hlfsupport-console-govern-channels-anchor-peers) as well.

Finally, your peer redundancy strategy needs to take into account your smart contract endorsement policies to ensure that you always have enough peers available to satisfy the [endorsement policy](#x8911635){: term} requirements. For example, if an endorsement policy requires a specific number of endorsements, your peer HA strategy needs to ensure that there are always that number of peers available. Alternatively, if the endorsement policy requires a `MAJORITY` of peers to endorse the transactions, then you need to ensure that a majority of the peers are always available in order for transactions to continue to be processed.

### Ordering service considerations
{: #ibm-hlfsupport-console-ha-ordering-service}

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 is built upon Hyperledger Fabric v2.2.4 and v2.2.5 that includes the Raft [ordering service](#x9826021){: term}. Raft is a crash fault tolerant (CFT) ordering service based on an implementation of [Raft protocol](https://raft.github.io/raft.pdf){: external}. By design, Raft ordering nodes automatically synchronize data between them using Raft-based consensus. In {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, an organization network operator can choose to stand up either a single node Raft-based orderer, with no HA, or five orderers in a single region or across multiple regions that are automatically configured for HA via Raft.


### Certificate Authority (CA) considerations
{: #ibm-hlfsupport-console-ha-ca}

Because [CA](#x2016383){: term} nodes process user registration and enrollment requests for your blockchain network, they are typically not high-throughput nodes. However, if HA is important for your CA strategy, it is possible to configure your nodes for maximum availability. Kubernetes includes built in HA by immediately restarting the pod if the CA becomes unavailable.  However, if you cannot tolerate any downtime for your CA, you have the option to configure `replica sets` for your CA. A replica set is a Kubernetes mechanism that is used to guarantee the availability of the pod. The use of replica sets ensures that multiple replicas of the pod are running at all times. If one of the CA replicas becomes unavailable, Kubernetes immediately switches to another CA replica, therefore there is no downtime waiting for a CA to restart.

## HA Checklist
{: #ibm-hlfsupport-console-ha-checklist}


The following table contains a list of options to consider as you plan for increasing degrees of HA.

|  | Single node | Single cluster with multiple nodes | Multizone | Multiple clusters across regions |
|-----|-----|-----|-----|-----|
| Redundant peers | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Redundant anchors peers on a channel| ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg)|
| Anti-affinity (peers) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | |
| Multi-zone (peers) |  |  | ![Checkmark icon](../icons/checkmark-icon.svg) | |
|Raft ordering service | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg)| ![Checkmark icon](../icons/checkmark-icon.svg)| ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ordering nodes | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) + anti-affinity | ![Checkmark icon](../icons/checkmark-icon.svg) + anti-affinity | ![Checkmark icon](../icons/checkmark-icon.svg) |
| CA replica sets | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | |
| Anti-affinity (CA replica sets) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | |
|Development or Test environment | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | | |
| Production environment | | | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: row-headers}
{: class="comparison-table"}
{: caption="Table 1. Comparison of deployment scenarios to increase your network HA" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify the deployment scenarios. The column headers identify available options in each scenario to increase your HA."}


The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployer attempts to spread peers, ordering nodes, and CA replica sets across different worker nodes but cannot guarantee that it will happen due to resource limitations. You can also use the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs or the blockchain console to deploy peers or ordering nodes to specific zones in order to ensure that they are resilient to a zone failure. For more information see [Multizone HA](#ibm-hlfsupport-console-ha-multi-zone).
{: tip}

## Potential points of failure
{: #ibm-hlfsupport-console-ha-points-of-failure}

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric offers several approaches to add more availability to your network by adding redundancy and using anti-affinity policies. Review the following diagrams to learn more about potential points of failure and how to eliminate them. You can select a model based on your application criticality, service levels, and costs. As general rule, you can implement the redundancy to meet your service levels. All of these scenarios must be weighed against the cost of implementing greater resiliency.

### Single region HA
{: #ibm-hlfsupport-console-ha-single-region}

![Blockchain HA single region options](images/HA_Diagram-1.svg "Blockchain HA options"){: caption="Figure 1. Blockchain HA single region options" caption-side="bottom"}


1. **Component failure.**

    **Single-zone cluster**:  

    Every time that you deploy a blockchain component, such as a peer, ordering node, or CA, a new pod is created for the component in a worker node. Containers and pods are, by design, short-lived and can fail unexpectedly. For example, a container or pod might crash if an error occurs in your component. So, to make your node highly available, you must ensure that you have enough instances of it to handle the workload plus extra instances in case of a failure.

    **Peers** How many peers are required? In a production scenario, the recommendation is to deploy three peers from the same organization to each channel. This configuration allows one peer to go down (for example, during a maintenance cycle) and still maintain two highly available peers. Therefore, to compensate for a peer failure, and for the most basic level of HA, you can achieve peer redundancy by simply deploying three peers per organization on a channel on your worker node. Note that you need to ensure that you have adequate resources available on your node to support these components.

    **Ordering service** As mentioned above, the HA ordering service is based on Raft, and contains five ordering nodes by default. Because the system can sustain the loss of nodes, including leader nodes, as long as there is a majority of ordering nodes (what’s known as a “quorum”) remaining, Raft is said to be “crash fault tolerant” (CFT). In other words, if you have five nodes in a channel, you can lose two nodes (leaving three remaining nodes). When you deploy an ordering service from the console, choose the five node service for HA.

    **CA** You can configure replica sets, which are represented as shaded CA boxes in the diagram above, for your CA. Replica sets guarantee that if the CA node goes down, the CA replica immediately begins processing requests. You must provision an instance of a PostgreSQL database if you plan to use CA replica sets. See these instructions for more information about [how to configure CA replica sets](/docs/blockchain?topic=blockchain-ibm-hlfsupport-console-build-ha-ca).

    This scenario uses redundant peers, ordering nodes, and CAs on a single worker node, which protects against component failure, but cannot protect from node failure. Therefore, it is only suitable for development and testing purposes.

2. **Worker node failure.**  

    **Single-zone cluster with multiple worker nodes and anti-affinity**:

    A worker node is a VM that runs on a physical hardware. Worker node failures include hardware outages, such as power, cooling, or networking, and issues on the VM itself. You can account for a worker node failure by setting up multiple worker nodes when you provision your cluster. When blockchain components are distributed across multiple worker nodes, you are protected from a worker node failure.

    **Peers** The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployer anti-affinity policy distributes redundant peers, that is peers from the same organization, across the worker nodes in their cluster.

    **Ordering service** Whenever you deploy a Raft ordering service, the five ordering nodes are automatically distributed across the worker nodes in your cluster, using the anti-affinity policy and based on resource availability on the nodes.

    **CAs** Like peers and ordering nodes, if replica sets are chosen for a CA, an anti-affinity policy automatically distributes the CA replica sets across worker nodes in the cluster, based on resource availability.

    This scenario uses redundant peers, ordering nodes, and CA replica sets, across multiple worker nodes in a single cluster or zone, which protects against node failure, but cannot protect from a cluster or zone failure. Therefore, it is not recommended for production.

### Multizone HA
{: #ibm-hlfsupport-console-ha-multi-zone}

![Blockchain HA single zone options](images/HA_Diagram_2.svg "Blockchain HA options"){: caption="Figure 2. Blockchain HA single zone options" caption-side="bottom"}

**Zone failure.**  

**Multizone clusters with multiple worker nodes and anti-affinity**:

Think of a zone as a data center. A zone failure affects all physical compute hosts and NFS storage. Failures include power, cooling, networking, or storage outages, and natural disasters, like flooding, earthquakes, and hurricanes. To protect against a zone failure, you must have clusters in at least two different zones that are load balanced by an external load balancer.

A single zone is sufficient for a development and test environment if you can tolerate a zone outage. Therefore, to leverage the HA benefits of multiple zones,  when you provision your cluster, ensure that multiple zones are selected. Two zones are better than one, but three are recommended for HA to increase the likelihood that the two additional zones can absorb the workload of any single zone failure.  When redundant peers from the same organization and channel, and ordering nodes, are spread across multiple zones, a failure in any one zone should not affect the ability of the network to process transactions because the workload will shift to the blockchain nodes in the other zones.

You can use the Fabric Operations Console to specify the zone where a CA, peer, or ordering node is created. When you deploy a CA, peer, or ordering service (or a single ordering node), check the Advanced deployment option that is labeled **Deployment zone selection** to see the list of zones that is currently configured for your Kubernetes cluster.

If you're deploying a CA, peer, or ordering service, you have the option to select the zone from the list of zones available to your cluster or to let your Kubernetes cluster decide for you by leaving the default selected. For a five node ordering service, these nodes will be distributed into multiple zones by default, depending on the relative space available in each zone. You also have the ability to distribute a five node ordering service yourself by unselecting the default option to have the zones chosen for you and distributing these nodes into the zones you have available. If you are deploying a redundant node (that is, another peer when you already have one), it is a best practice to deploy this node into a different zone. You can check which zone the other node was deployed to by opening the tile of the node and looking under the **Node location**. Alternatively, you can use the APIs to deploy a peer or orderer to a specific zone. For more information on how to do this with the APIs, see [Creating a node within a specific zone](/docs/blockchain?topic=blockchain-ibm-hlfsupport-v2-apis#ibm-hlfsupport-v2-apis-zone).

The CA zone selection is only available when the default database type SQLite is used and your cluster is configured with multiple zones.
{: note}

**Other CA considerations**:
If you have multiple zones configured for your Kubernetes cluster, when you create a new CA with a PostgreSQL database and replica sets, an anti-affinity policy ensures that the CA replica sets are automatically configured across the zones. Replica sets are represented as shaded CA boxes in the diagram above. Adequate resources must exist in the other zones in order for the anti-affinity policy to be used.  

#### Multizone-capable storage:
{: #ibm-hlfsupport-console-ha-multi-zone-storage}

Before you deploy any nodes, you have the additional option to configure your Kubernetes cluster to use [multizone-capable storage](/docs/containers?topic=containers-storage_planning#persistent_storage_overview) as your persistent storage.  Without multizone-capable storage, if an entire zone goes down, any blockchain nodes in that zone cannot automatically come up in another zone because their associated persistent storage is unavailable in the failed zone. When multizone-capable storage is configured, if a zone failure occurs, peers and ordering nodes can come up in another zone, with their associated storage intact, ensuring high-availability. In order to leverage this capability with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you need to configure your cluster to use **SDS (Portworx)** storage. When you deploy a peer, ordering service, or ordering node, select the advanced deployment option labeled **Deployment zone selection** and then select **Across all zones**.

This scenario uses redundant peers, ordering nodes, and CAs across multiple worker nodes and multiple zones, which protect against zone failure, but does not protect from an unlikely entire region failure.

If you choose to use a multi-zone configuration for CA, peers, or ordering nodes, then you are responsible for configuring the storage for each zone and set the node affinity to zones.
{: important}

### Multi-region HA
{: #ibm-hlfsupport-console-ha-multi-region}

This scenario offers the highest level of HA possible.

![Blockchain HA multi-region options](images/HA-Diagram3.png "Blockchain HA options"){: caption="Figure 3. Blockchain HA multi-region options" caption-side="bottom"}

**Region failure.**

**Multi-region clusters with multiple work nodes and anti-affinity**:

The likelihood of a full regional failure is low. However, to account for this failure, you can set up multiple clusters in different regions where each cluster has its own linked console. If an entire region fails, redundant peers or ordering nodes in the other regions can service the work load. For production environments, configuring your blockchain **peers** and **ordering nodes** across multiple regions provides the maximum HA coverage available.

This scenario uses redundant peers and ordering nodes across multiple worker nodes in multiple regions, which provide the highest degree of HA. This approach is also a recommended scenario for a production network when your resiliency requirements merit the investment. The five ordering nodes are spread across three clusters in a 2-1-2 pattern, meaning two nodes in Region 1, one node in Region 2, and two nodes in Region 3. This configuration allows any single region, or all of the ordering nodes in a region to go down, while still maintaining a quorum of nodes in the Raft cluster.

See the topics on setting up multi-region HA deployments for steps to configure your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric [peers](/docs/blockchain?topic=blockchain-ibm-hlfsupport-console-hadr-mr) and [ordering nodes](/docs/blockchain?topic=blockchain-ibm-hlfsupport-console-hadr-mr-os) across multiple regions.

## Disaster recovery (DR)
{: #ibm-hlfsupport-console-ha-dr}

The most important step for disaster recovery is to configure your environment across multiple regions as documented in the previous section. Multi-region configuration ensures that your environment can handle a datacenter or regional disaster with zero downtime or data loss. However, this does not protect against data corruption or accidental deletion. To do this, you may want to also take periodic backups.

It is recommended that you regularly back up the storage associated with every deployed component. Because the ledger is shared across all the peers and ordering nodes, taking regular backups is critical. For example, if incorrect data is accidentally propagated to a peer's ledger or if data is mistakenly deleted, the incorrect data might spread to the ledgers of some other peers. This would require the restoration of the ledgers of all the peers from an established backup point to ensure synchronicity. You can decide how often to perform the backups based your recovery needs, but a general guideline would be to take daily backups.

All nodes must be [stopped](#ibm-hlfsupport-console-ha-stop-nodes) in order to ensure a reliable backup.
{: important}


| Storage solution provider | Guidance |
|----------|---------|
| OpenShift Container Platform | You can leverage the capability that is provided by the OpenShift Container platform to [backup persistent volume claims](https://docs.openshift.com/container-platform/4.3/backup_and_restore/disaster_recovery/about-disaster-recovery.html){: external}. |
| Portworx | While a [snapshot capability](https://docs.portworx.com/portworx-install-with-kubernetes/cloud/ibm/#prerequisites){: external} is available for taking backups without stopping the nodes, in order to get a reliable backup, the nodes must be stopped.|
{: caption="Table 2. Backup recommendations for storage" caption-side="top"}


When you need to restore a backup, the backups would need to be restored on every component across your network.

If you are using CA replica sets and your PostgreSQL database resides in {{site.data.keyword.cloud_notm}}, backups are included in the service. See the topic on [Managing Backups](/docs/databases-for-postgresql?topic=cloud-databases-dashboard-backups){: external} for more information. Otherwise, you need to work with your third-party PostgreSQL database provider to manage the database backups according to your DR needs.

### Backup and recovery
{: #ibm-hlfsupport-console-ha-stop-nodes}

For information about backing up your components and how to recover corrupted components or networks, see [Backing up and restoring components and networks](/docs/blockchain?topic=blockchain-backup-restore).

