---

copyright:
  years: 2022
lastupdated: "2022-01-31"

keywords: pricing model, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}



# Pricing
{: #ibm-hlfsupport-sw-pricing}



This guide helps you understand the pricing model for {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, and how much you will pay when you develop and grow your blockchain network of Certificate Authorities, peers, and ordering service components, that are based on Hyperledger Fabric v2.2.3.
{: shortdesc}

## License
{: #ibm-hlfsupport-software-pricing-license}

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric delivers the components that are needed to run a blockchain network on your own infrastructure using Kubernetes. It also provides support for Hyperledger Fabric images and the entitlement to a certified image of the Fabric Operations Console and IBM Operator to make deploying and managing Hyperledger Fabric easier. You can access your [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering which is required in order to deploy the release. With this offering, you are responsible for procuring and provisioning your own Kubernetes cluster on one of the [Supported Platforms](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-prerequisites).

## Pricing
{: #ibm-hlfsupport-software-pricing-pricing}

The pricing of {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric is based on the number of Virtual Processor Cores (VPCs) used. A VPC, or vCPU, can be either a virtual core that is assigned to a virtual server, or a physical processor core in a non-partitioned server. You must obtain a licensed entitlement for each VPC made available to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric. The number of vCPU or CPUs that are required can vary depending on your infrastructure, network design and performance requirements. You are only charged for the VPCs used by your Certificate Authority (CA), peer, and ordering nodes. Specifically, for the peer, you are only charged for the VPCs used by the peer container itself. You are not charged for the VPCs used by the CouchDB, chaincode launcher (and associated chaincode pod), and gRPC proxy containers. Similarly, for an ordering service node, you are not charged for the gRPC proxy container.

If you are using the Red Hat OpenShift Container Platform, you can read more about  VPCs (CPUs) in that platform, see  [Allocating Node Resources](https://docs.openshift.com/container-platform/4.4/nodes/nodes/nodes-nodes-resources-configuring.html){: external}.
Note that 1 `core` in OpenShift is equivalent to 1 vCPU in {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric offering also includes the ability to download and run just the peer, orderer, CA, and smart contract container images without the console. This is an advanced option for customers who are experienced with the Hyperledger Fabric processes for deploying and running the nodes and includes limited support for those images.

Various licensing options are available. For more information, please [Contact us](https://www.ibm.com/account/reg/us-en/signup?formid=MAIL-blockchain ){: external}.
