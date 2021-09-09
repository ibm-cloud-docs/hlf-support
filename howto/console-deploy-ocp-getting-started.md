---

copyright:
  years: 2018, 2021
lastupdated: "2021-08-11"

keywords: OpenShift, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters, multicloud

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






<style>
<!--
    #tutorials { /* hide the page header */
        display: none !important;
    }
    .allCategories {
        display: flex !important;
        flex-direction: row !important;
        flex-wrap: wrap !important;
    }
    .categoryBox {
        flex-grow: 1 !important;
        width: calc(33% - 20px) !important;
        text-decoration: none !important;
        margin: 0 10px 20px 0 !important;
        padding: 16px !important;
        border: 1px #dfe6eb solid !important;
        box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2) !important;
        text-align: center !important;
        text-overflow: ellipsis !important;
        overflow: hidden !important;
    }
    .solutionBoxContainer {}
    .solutionBoxContainer a {
        text-decoration: none !important;
        border: none !important;
    }
    .solutionBox {
        display: inline-block !important;
        width: 100% !important;
        margin: 0 10px 20px 0 !important;
        padding: 16px !important;
        background-color: #f4f4f4 !important;
    }
    @media screen and (min-width: 960px) {
        .solutionBox {
        width: calc(50% - 3%) !important;
        }
        .solutionBox.solutionBoxFeatured {
        width: calc(50% - 3%) !important;
        }
        .solutionBoxContent {
        height: 350px !important;
        }
    }
    @media screen and (min-width: 1298px) {
        .solutionBox {
        width: calc(33% - 2%) !important;
        }
        .solutionBoxContent {
        min-height: 350px !important;
        }
    }
    .solutionBox:hover {
        border: 1px rgb(136, 151, 162)solid !important;
        box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2) !important;
    }
    .solutionBoxContent {
        display: flex !important;
        flex-direction: column !important;
    }
    .solutionBoxTitle {
        margin: 0rem !important;
        margin-bottom: 5px !important;
        font-size: 14px !important;
        font-weight: 900 !important;
        line-height: 16px !important;
        height: 37px !important;
        text-overflow: ellipsis !important;
        overflow: hidden !important;
        display: -webkit-box !important;
        -webkit-line-clamp: 2 !important;
        -webkit-box-orient: vertical !important;
        -webkit-box-align: inherit !important;
    }
    .solutionBoxDescription {
        flex-grow: 1 !important;
        display: flex !important;
        flex-direction: column !important;
    }
    .descriptionContainer {
    }
    .descriptionContainer p {
        margin: 0 !important;
        overflow: hidden !important;
        display: -webkit-box !important;
        -webkit-line-clamp: 4 !important;
        -webkit-box-orient: vertical !important;
        font-size: 14px !important;
        font-weight: 400 !important;
        line-height: 1.5 !important;
        letter-spacing: 0 !important;
        max-height: 70px !important;
    }
    .architectureDiagramContainer {
        flex-grow: 1 !important;
        min-width: calc(33% - 2%) !important;
        padding: 0 16px !important;
        text-align: center !important;
        display: flex !important;
        flex-direction: column !important;
        justify-content: center !important;
        background-color: #f4f4f4;
    }
    .architectureDiagram {
        max-height: 175px !important;
        padding: 5px !important;
        margin: 0 auto !important;
    }
-->
</style>

# Getting started
{: #deploy-ocp-getting-started}

Deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 onto a Kubernetes cluster that is running on OpenShift Container Platform. The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric uses a [Kubernetes Operator](https://www.openshift.com/learn/topics/operators){: external} to install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console on your cluster and manage the deployment and your blockchain nodes. After the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console is running on your cluster, you can use the console to create blockchain nodes and operate a multicloud blockchain network.
{: shortdesc}

If you prefer to automate the installation of the service, check out the [Ansible Playbook](/docs/hlf-support?topic=hlf-support-ansible-install-ibp) that can be used to complete all of these steps for you.
{: tip}

## Resources required
{: #deploy-ocp-resources-required}

Ensure that your OpenShift cluster has sufficient resources for the {{site.data.keyword.blockchainfull_notm}} console and for the blockchain nodes that you create. The amount of resources that are required can vary depending on your infrastructure, network design, and performance requirements. To help you deploy a cluster of the appropriate size, the default CPU, memory, and storage requirements for each component type are provided in this table. Your actual resource allocations are visible in your blockchain console when you deploy a node and can be adjusted at deployment time or after deployment according to your business needs.

The resources for the CA, peer, and ordering nodes need to be multiplied by the number of these nodes that you require. The operator and console resources are per blockchain deployment. For example, if you deployed development, staging, and test networks in a single cluster, you need to have enough resources for three instances of the operator and console, one for each blockchain deployment. On the other hand, the webhook resources are per OpenShift cluster, only one instance is required, regardless of the number of blockchain networks in the cluster.

| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer (Hyperledger Fabric v1.4)**                       | 1.1           | 2.8                   | 200 (includes 100GB for peer and 100GB for state database)|
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

In addition to a small amount of storage (10 GB) required by the {{site.data.keyword.blockchainfull_notm}} console, persistent storage is required for each CA, peer, and ordering node that you deploy. Because blockchain components do not use the Kubernetes node local storage, network-attached remote storage is required so that blockchain nodes can fail over to a different Kubernetes worker node in the event of a node outage.  And because you cannot change your storage type after deploying peer, CA, or ordering nodes, you need to decide the type of persistent storage that you want to use _before_ you deploy any blockchain nodes.

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console uses dynamic provisioning to allocate storage for each blockchain node that you deploy by using a pre-defined storage class. You can choose your persistent storage from the available Kubernetes storage options.

If the storage includes support for the `volumeBindingMode: WaitForFirstCustomer` setting, you should configure it to delay volume binding until the pod is scheduled. Read more in the [Kubernetes Storage Classes documentation](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode){: external}.
{: tip}

Before you deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you must create a storage class with enough backing storage for the {{site.data.keyword.blockchainfull_notm}} console and the nodes that you create. You can set this storage class to use the default storage class of your Kubernetes cluster or create a new class that is used by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console. If you are using a multizone cluster in {{site.data.keyword.cloud_notm}} and you change the default storage class definition, then you must configure the default storage class for each zone.  
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
| **Scalability **| When a node runs out of storage, it ceases to operate. Even if Kubernetes attempts to redeploy the node elsewhere, when the storage is full, the node cannot operate. Because the ledger is immutable and can only grow over time, expandable storage is nice to have for blockchain networks whenever available. At some point, you will run out of storage on your peers and ordering nodes and expandable storage helps to avoid this situation. If the storage is not expandable, when a peer or ordering runs out of storage, you need to provision a new node with a larger storage capacity and then delete the old one. When the new node is deployed, it must replicate the ledger, which can take time depending on the depth of the block history. |
| **Monitoring** | It's critical to monitor the storage consumption by your nodes. Consider the scenario where you deploy five ordering nodes, all with the same amount of storage. They are all replicating the same ledgers so they will all run out of storage at approximately the same time and you will lose consensus, causing the network to stop functioning. Therefore, you might want to consider varying the storage across the nodes and monitoring it as the ledger grows to avoid this situation. Before storage is exhausted on a node, you can expand it or provision a new node. |
| **Encryption** | Fabric does not require storage to be encrypted but it is a best practice for Security. You need to decide whether encryption is important for your business. If you have the option of encrypting the persistent volume, there may be some performance implications with encryption to consider.  |
| **High Availability (HA)** | There should be redundancy in the storage to avoid a single point of failure. |
| **Multi-zone capable storage** | {{site.data.keyword.cloud_notm}} includes the ability to create a single Kubernetes cluster across multiple data centers or "zones". Portworx offers multi-zone capable storage that can be used to potentially reduce the number of peers required. Consider a scenario where you build two zones with two peers for redundancy, one zone can go down and you still have two peers up in another zone. With multi-zone capable storage, you could instead have two zones with one peer each. If one zone goes down, the peer comes up in the other zone with its storage intact, reducing the overall redundant peer requirements. |

## Filesystem permissions
{: #deploy-ocp-fs-perm}

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric uses Kubernetes [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/){: externnal} to set the correct filesystem permissions for the mounted volumes. The init containers run as the root user. The volumes are mounted on the init container which then changes the owner of the mounted folder to be the non-root user of the container that will use it. Learn  more about [how to give non-root users write permission on the volume mount path](/docs/containers?topic=containers-nonroot){: external}.

## Choose your deployment option
{: #deploy-ocp-choose}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric can be deployed in four different ways depending on your business goals. Red Hat customers may prefer to deploy the service directly from the Red Hat Marketplace to their OpenShift cluster in the cloud or on-prem. If you prefer to step through the process manually, you can deploy it to your cloud or on-prem  behind a firewall. Finally, an Ansible playbook is available to automate the deployment of the service to your OpenShift cluster.

<div class=solutionBoxContainer>
  <div class="solutionBox">
    <a href = "/docs/hlf-support?topic=hlf-support-deploy-ocp-rhm">
      <div>
        <img src="../images/logo_redhat.png" alt="Red Hat icon" width="50" style="width:50px; border-style: none"/>
        <p><strong> Red Hat Marketplace</strong></p>
        <p class="bx--type-caption">Use the Red Hat Marketplace to deploy the service to your OpenShift cluster in the cloud.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "/docs/hlf-support?topic=hlf-support-deploy-ocp-rhm-fw">
      <div>
        <img src="../images/logo_redhat.png" alt="Red Hat icon" width="50" style="width:50px; border-style: none"/>
        <p><strong> Red Hat Marketplace</strong></p>
        <p class="bx--type-caption">(Airgap install) Use the Red Hat Marketplace to deploy the service to your OpenShift cluster behind a firewall.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "/docs/hlf-support?topic=hlf-support-deploy-ocp">
      <div>
        <img src="../images/logo_openshift.svg" alt="OpenShift icon" width="25" style="width:25px; border-style: none"/>
        <p><strong>OpenShift</strong></p>
        <p class="bx--type-caption"> Manually deploy the IBM Blockchain  Platform service to your OpenShift cluster in your cloud.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
      <a href = "/docs/hlf-support?topic=hlf-support-deploy-ocp-firewall">
        <div>
          <img src="../images/logo_openshift.svg" alt="OpenShift icon" width="25" style="width:25px; border-style: none"/>
          <p><strong>OpenShift on-prem</strong></p>
          <p class="bx--type-caption"> Manually deploy the IBM Blockchain Platform to your OpenShift cluster on-prem behind a firewall.</p>
        </div>
      </a>
  </div>    
  <div class="solutionBox">
        <a href = "/docs/hlf-support?topic=hlf-support-ansible-install-ibp">
          <div>
            <img src="../images/ansible.png" alt="Ansible icon" width="25" style="width:25px; border-style: none"/>
            <p><strong>Ansible</strong></p>
            <p class="bx--type-caption"> Automate the deployment of the IBM Blockchain Platform to your OpenShift cluster using an Ansible playbook.</p>
          </div>
        </a>
  </div>
</div>
