---

copyright:
  years: 2022
lastupdated: "2022-01-31"

keywords: Kubernetes, IBM Cloud Private, OCP, OpenShift Container Platform, {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, multicloud

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}



# Getting started with {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0
{: #get-started-console-ocp}


{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric is a support offering that also provides entitlement to IBM certified Hyperledger Fabric images as well as certified images of the Fabric Operations Console and Kubernetes Operator.  These images allow you to deploy blockchain components on many platforms including open source Kubernetes, distributions such as Rancher, OpenShift Container Platform. For details of what is supported, see [Supported Platforms](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-prerequisites). This offering is ideal for the customers who want to deploy their components, store their data, or run their workloads on their own infrastructure or across public and private clouds for security, risk mitigation, preference, or compliance reasons. Clients can build, operate, and grow their blockchain networks with an offering that can be used from development through production.
{: shortdesc}

After provisioning the IBM Operator and Fabric Operations Console, use the console or {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs to build a consortium of organizations to easily transact on the same network, regardless of each client's cloud preference. By installing the Fabric Operations Console, users can create components in their clusters and connect them to components deployed on other clusters, forming a distributed, multi-organizational blockchain network.

Currently, support is not available for environments that were not deployed using the IBM Operator and are not being managed using the certified Fabric Operations Console.  This is because the operator provides a tested and stable deployment and also automates many ongoing tasks.  In addition the Fabric Operations Console makes ongoing management, configuration, and support much easier.

The **{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0** is purchased as a stand-alone entitlement. After purchase, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. This key is required to deploy the release or download the container images unless you are deploying from Red Hat Marketplace.

With this offering, you are responsible for purchasing and provisioning your own Kubernetes cluster. Additionally, you will need to [open ports in your firewall](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-security#ibm-hlfsupport-security-ibm-hlfsupport-ports) if you are using one.
{: important}

## Are you a Red Hat Marketplace customer?
{: #get-started-console-ocp-rhm}

Already using Red Hat Marketplace and ready to try out blockchain? Check out these [instructions](/docs/hlf-support?topic=hlf-support-deploy-ocp-rhm) to get started.

## Is {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric suitable for you?
{: #get-started-console-ocp-suitable}

{{site.data.keyword.IBM_notm}} provides different Blockchain offerings that allow you to deploy your network in the environment of your choice. The information below will help you decide which offering meets your needs.

| |{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric | {{site.data.keyword.IBM_notm}} Blockchain Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Multiple Kubernetes distributions on a private, public or hybrid **multicloud** <br><br> See [Supported Platforms](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-prerequisites) | A Kubernetes cluster on {{site.data.keyword.cloud_notm}} <br><br> See [Supported configuration](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-supported-cfg) |  
| What are my deployment options? | <ul><li> Full platform </li> </ul>| <ul><li> Full platform </li> </ul>
| How is it billed? |Contact us for [pricing](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-sw-pricing) |[$0.29 USD per allocated CPU hour](/docs/blockchain?topic=blockchain-ibm-hlfsupport-saas-pricing)  |
| Can I connect with Peers in other clouds? |  Yes| Yes |
| Can my data center be on-prem and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes |
| Are APIs available for node management? | Yes | Yes |
| Does the product integrate with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric VSCode extension to develop and test my smart contracts?| Yes | Yes |
| I am an experienced Fabric customer. Are peer, CA, orderer, and smart contract images available and supported? | No | No |
| Where can I learn more? |See [About {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-offers) | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |
| Ready to get started? | See [Step one: Install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric](/docs/hlf-support?topic=hlf-support-get-started-console-ocp#get-started-console-ocp-step-two-deploy-console) | See [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks) |
{: caption="Table 1. Which offering is right for your business?" caption-side="bottom"}
** {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric ** images - Your purchase of {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 includes an entitlement to images for peer, CA, ordering service, and smart contract containers that are signed by IBM. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability.


Have questions and want to speak to an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric expert? [Schedule a consult](https://www.ibm.com/cloud/blockchain-platform/developer?schedulerform){: external} now to learn more about how blockchain can transform your business.

### Developer Tools
{: #get-started-console-ocp-dev-tools}

Are you a developer? Check out the [**{{site.data.keyword.IBM_notm}} Blockchain Platform Developer Tools**](/docs/hlf-support?topic=hlf-support-develop-vscode#develop-vscode), a free IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly. Install it locally or run it from the cloud using Red Hat CodeReady Workspaces.

### {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images
{: #get-started-console-ocp-images}

- Your purchase of {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 includes an entitlement to images for [peer](#x2033450){: term}, [Certificate Authority](#x2016383){: term}, [ordering service](#x9826021){: term}, and [smart contract](#x8888420){: term} containers that are signed by {{site.data.keyword.IBM_notm}}. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability.

## Before you begin
{: #get-started-console-ocp-set-up-ocp}

1. Review the [Supported Platforms](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-prerequisites).

2. Install a Kubernetes cluster and log in to your cluster. If you are using OpenShift, see the [OpenShift Container Platform CLI](https://docs.openshift.com/container-platform/4.3/cli_reference/openshift_cli/getting-started-cli.html){: external} to deploy the CLI. If you are using an OpenShift cluster that was deployed with the {{site.data.keyword.IBM_notm}} Kubernetes Service, use these instructions to [Install the OpenShift Origin CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).

3. If you are _not_ running the platform on Red Hat OpenShift Container Platform or Red Hat Open Kubernetes Distribution, you need to set up the NGINX Ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}.

4. If you are not deploying from Red Hat Marketplace, get the entitlement key from your MyIBM account in order to install the platform. For more information, see [Get your entitlement key](/docs/hlf-support?topic=hlf-support-deploy-ocp#deploy-ocp-entitlement-key).

5. Review the persistent storage considerations for [OpenShift](/docs/hlf-support?topic=hlf-support-deploy-ocp-getting-started#deploy-ocp-storage) or [Kubernetes](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-storage) depending on your platform.

## Step one: Install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric
{: #get-started-console-ocp-step-two-deploy-console}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric uses a [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/){: external} to install the Fabric Operations Console on your cluster and manage the deployment and your blockchain nodes. A cluster administrator can use your entitlement key to deploy the platform on any Kubernetes Cluster

-  If you are deploying the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric on the OpenShift Container Platform, you can deploy the platform by using the steps in [Deploy {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 on the OpenShift Container Platform](/docs/hlf-support?topic=hlf-support-deploy-ocp). Or, if you prefer to deploy from the Red Hat Marketplace see [Deploy {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 from Red Hat Marketplace](/docs/hlf-support?topic=hlf-support-deploy-ocp-rhm).

- If you are deploying the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric on open source Kubernetes or a distribution such as Rancher, use the steps described in the [Deploying {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 on Kubernetes](/docs/hlf-support?topic=hlf-support-deploy-k8).


## Step two: Grant console access to other users
{: #get-started-console-ocp-step-four-add-console-admin}

After the platform is deployed, the console administrator can log in to the console with the email address and password that was provided during console deployment. The password that was provided becomes the default password of the console, and is used by all new users to log in to the console for the first time. The administrator can then add new users to the console, allowing others to log in and start working with {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric nodes. The administrator can also set a new default password. To learn more, see [Managing users from the console](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-users).

## Step three: Use the console to create your components
{: #get-started-console-ocp-build-network}

After you deploy the console, you can use it to create, operate, and govern {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric components on your Kubernetes cluster. To get started with building a blockchain network by using the management console, go to the [Build a network tutorial](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network).

## Step four: Connect networks across clouds
{: #get-started-console-ocp-import-nodes}

You can use your console to operate components that are running on other clusters. First, you need to export the component information to a JSON file from the console where the component was originally deployed. Then, you can import the node JSON file into the console that is deployed on your local cluster and manage the components across clouds. For more information, see [Importing nodes](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-import-nodes#ibm-hlfsupport-console-import-nodes).
