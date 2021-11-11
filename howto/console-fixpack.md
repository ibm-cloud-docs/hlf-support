---

copyright:
  years: 2021
lastupdated: "2021-11-11"

keywords: Kubernetes, {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console, deploy, resource requirements, storage, parameters, fix pack, multicloud

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}




# Installing the 1.0.0 fix pack
{: #install-fixpack}

Use these instructions if you have already installed or upgraded to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 before June 17, 2021 and want to apply the latest 1.0.0 fix pack. This fix pack is cumulative, which means that it includes all of the fixes from previous 1.0.0 fixpacks. It contains important bug fixes and should be applied to your network as soon as possible.  If you install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 after June 17, 2021, the platform will contain all the bug fixes and improvements included in this fix pack, and you do not need to apply it.
{: shortdesc}

You can install the fix pack by updating the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment on your Kubernetes cluster to pull the latest images from the {{site.data.keyword.IBM_notm}} entitlement registry. You can apply the fix pack by using the following steps:

1. [Update the webhook](#install-fixpack-webhook)
1. [Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator](#install-fixpack-operator)
1. [Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console](#install-fixpack-console)
1. [Update your blockchain nodes](#install-fixpack-nodes)

You can use these steps if you deployed the platform on the OpenShift Container Platform, open source Kubernetes, or distributions such as Rancher.  If you have multiple networks deployed on your cluster, you will need to repeat steps 2-4 to update each 1.0.0 network because they run on separate namespaces. If you experience any problems, see the instructions for [rolling back the fix pack installation](#install-fixpack-rollback).  If you deployed your network behind a firewall, without access to the external internet, see the separate set of instructions for [Installing the 1.0.0 fix pack behind a firewall](#install-fixpack-firewall). You can install the fix pack without disrupting a running network. However, you cannot use the console to deploy new nodes, deploy smart contracts, or create new channels during the process.

## What this fix pack contains
{: #install-fixpack-contents}

This cumulative fix pack contains security patches for the Fabric images and miscellaneous bug fixes.  See the [Release notes](/docs/hlf-support?topic=hlf-support-release-notes-saas-20) for more details.

## Before you begin
{: #install-fixpack-begin}

To upgrade your network, you need to [retrieve your entitlement key](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-entitlement-key) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

## Step one: Update the webhook
{: #install-fixpack-webhook}

The process of updating your network begins with updating the webhook that you created when you initially deployed or upgraded to the 1.0.0 blockchain service. Run the following command to update the webhook in the `ibm-hlfsupport-infra` namespace or project:

```
kubectl set image deploy/ibm-hlfsupport-webhook -n ibm-hlfsupport-infra ibm-hlfsupport-webhook=cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211026-amd64
```
{: codeblock}

When the webhook update is successful, you see something similar to:
```
deployment.apps/ibm-hlfsupport-webhook image updated
```
{: codeblock}

Before you proceed with the next step, wait for the webhook to restart by running the following command:
```
kubectl get po -n ibm-hlfsupport-infra -w | grep ibm-hlfsupport-webhook
```
{: codeblock}

When the webhook is successfully restarted, it looks similar to:

```
NAME                              READY   STATUS    RESTARTS   AGE
ibm-hlfsupport-webhook-5fd96f6c7d-gpwhr      1/1     Running   0          1m
```
{: codeblock}

## Step two: Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator
{: #install-fixpack-operator}

You can start applying the fix pack to your network by updating the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator. Log in to your cluster by using the kubectl CLI. You will need to provide the name of the Kubernetes namespace that you created to deploy your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network. If you deployed your network on Kubernetes, you can use the `kubectl get namespace` command to find the name of your namespace. If you deployed the platform on the OpenShift Container Platform, log in to your cluster using the oc CLI. You can find the name of your OpenShift project using the `oc get project` command. Use the project name as the value for `<namespace>`.

Run the following command to download the operator deployment spec to your local file system. The default name of the operator deployment is `ibm-hlfsupport-operator`. If you changed the name during the deployment process, you can use the `kubectl get deployment -n <namespace>` command to get the name of the deployments on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl set image deploy/ibm-hlfsupport-operator -n <namespace> ibm-hlfsupport-operator=cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211026-amd64
```
{: codeblock}

After you update the operator, it will restart and pull the latest operator image. While the operator is restarting, you can still access your console UI. However, you cannot use the console to deploy smart contracts, or use the console or the APIs to create or remove a node.

Verify that the operator was successfully updated by running the command:

```
kubectl get deployment ibm-hlfsupport-operator
```
{: codeblock}

When the upgrade is successful, the output looks similar to:

```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibm-hlfsupport-operator   1/1       1            1           1m
```
{: codeblock}

If you experience a problem while you are updating the operator, go to this [troubleshooting topic](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-troubleshooting#ibm-hlfsupport-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems.

## Step three: Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console
{: #install-fixpack-console}

After you update to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator, you need to apply the fix pack to your console. You can update your console by removing the original ConfigMap and deployment spec that was created when the console was deployed. Removing these artifacts will allow your console to pull the latest configuration and images from the updated operator.

You can start by running the following command to delete the ConfigMap used by the console deployer. The default name of the ConfigMap is `ibpconsole-deployer`. If you changed the name of the console during the deployment process, you can use the `kubectl get configmap -n <namespace>` command to get the list of ConfigMaps on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl delete configmap -n <namespace> ibpconsole-deployer
```
{: codeblock}

You can then edit the console custom resource and delete the versions from spec. The default name of the console is `ipconsole`.  You can find the list of names for your console by using the `kubectl get ibpconsole -n <namespace>` command. Edit the custom resource using the following command:
```
kubectl edit ibpconsole ibpconsole -n <namespace>
```
{: codeblock}

Following with the console custom resource editing, you can now delete the console deployment using the following command:
```
kubectl delete deployment -n <namespace> ibpconsole
```
{: codeblock}

After you edit the custome resource, and delete the deployment and ConfigMap, the console will restart and download the new images and configuration settings provided by the 1.0.0 fix pack from the updated operator. You can use the following commands to confirm that the console has been updated with the latest images and configuration. The new images used by the console and your blockchain nodes will have the tags with the date `20211026`.
```
kubectl get deployment -n <namespace> ibpconsole -o yaml
kubectl get configmap -n <namespace> ibpconsole-deployer -o yaml
```
{: codeblock}

You can check that the upgrade is complete by running `kubectl get deployment ibpconsole`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibpconsole     1/1       1            1           1m
```
{: codeblock}


## Step four: Apply fixes to your blockchain nodes
{: #install-fixpack-nodes}

After you apply the fix to your console, you need to use your console UI to update the nodes of your blockchain network. Browse to the console UI and open the nodes overview tab. To upgrade a node, open the node tile and click the **Upgrade available** button. You cannot upgrade nodes that you imported to the console.

Apply upgrades to nodes one at a time. Your nodes are unavailable to process requests or transactions while the patch is being applied. Therefore, to avoid any disruption of service, you need to ensure that another node of the same type is available to process requests whenever possible. Installing upgrades on a node takes about a minute to complete and when it is complete, the node is ready to process requests.
{: important}

## Rolling back the fix pack installation
{: #install-fixpack-rollback}

When you apply the fix pack to your operator, it saves the secrets, deployment spec, and network information of your deployment before it restarts and attempts to apply the fixes to the console. If the fix pack installation fails for any reason, {{site.data.keyword.IBM_notm}} Support can roll back your upgrade and restore your previous deployment by using the information on your cluster. If you need to roll back your upgrade, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/node/1072956){: external} page.

You can roll back an upgrade after you use the console to operate your network. However, after you use the console to upgrade your blockchain nodes, you can no longer roll back your console to a previous version of the platform.

## Installing the 1.0.0 fix pack behind a firewall
{: #install-fixpack-firewall}

If you deployed the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric behind a firewall, without access to the external internet, you can install the  1.0.0 fix pack by using the following steps:

1. [Pull the latest {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images](#install-fixpack-images-firewall)
1. [Update the webhook](#install-fixpack-webhook-firewall)
1. [Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator](#install-fixpack-operator-firewall)
1. [Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console](#install-fixpack-console-firewall)
1. [Update your blockchain nodes](#install-fixpack-nodes-firewall)

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, deploy smart contracts, or create new channels during the upgrade process. If you have multiple networks deployed on your cluster, you will need to repeat steps 3-5 to update each 1.0.0 network because they run on separate namespaces.

### Before you begin
{: #install-fixpack-begin-firewall}

To upgrade your network, you need to [retrieve your entitlement key](/docs/hlf-support?topic=hlf-support-deploy-k8-firewall#deploy-k8-entitlement-key-firewall) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

### Step one: Pull the latest {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images
{: #install-fixpack-images-firewall}

The platform recommends that you use the `skopeo` utility to download and copy your images to your local container registry. [skopeo](https://www.redhat.com/en/blog/skopeo-10-released){: external} is a tool for moving container images between different types of container storages. In order to download the platform images and copy them to your container registry behind a firewall, you first need to [install skopeo](https://github.com/containers/skopeo/blob/master/install.md){: external}.

Before attempting these instructions, you need to have your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric entitlement key and container registry user id and password available. After you purchase the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. You can then use this key to access the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images.

Run the following set of commands to download the images and push them to your registry.  

Replace
- `<ENTITLEMENT_KEY>` with your entitlement key.
- `<LOCAL_REGISTRY_USER>` with the userid with access to your container registry.
- `<LOCAL_REGISTRY_PASSWORD>` with the password to your container registry.
- `amd64` - with `s390x` if you are installing on LinuxONE.

The following commands only work with a Docker container registry. Depending on the level of permissions required for the target location for the images, you might need to prefix each command with `sudo`.
{: note}

```
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-init:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-init:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-console:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-console:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-grpcweb:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-grpcweb:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-deployer:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-deployer:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-fluentd:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-fluentd:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:3.1.1-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:3.1.1-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ca:1.5.1-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-ca:1.5.1-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-dind:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-dind:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-chaincode-launcher:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-chaincode-launcher:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-enroller:1.0.0-20211026 docker://cp.icr.io/cp/ibm-hlfsupport-enroller:1.0.0-20211026 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
```
{: codeblock}

After you complete these steps, you can use the following instructions to deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric with the images in your registry.

### Step two: Update the webhook
{: #install-fixpack-webhook-firewall}

First, you need to update the webhook that you created when you initially deployed or upgraded to the 1.0.0 blockchain service. Run the following command to update the webhook in the `ibm-hlfsupport-infra` namespace or project:

```
kubectl set image deploy/ibm-hlfsupport-webhook -n ibm-hlfsupport-infra ibm-hlfsupport-webhook=cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211026-amd64
```
{: codeblock}

When the webhook update is successful, you see something similar to:
```
deployment.apps/ibm-hlfsupport-webhook image updated
```
{: codeblock}

Before you proceed with the next step, wait for the webhook to restart by running the following command:
```
kubectl get po -n ibm-hlfsupport-infra -w | grep ibm-hlfsupport-webhook
```
{: codeblock}

When the webhook is successfully restarted, it looks similar to:

```
NAME                              READY   STATUS    RESTARTS   AGE
ibm-hlfsupport-webhook-5fd96f6c7d-gpwhr      1/1     Running   0          1m
```
{: codeblock}

### Step three: Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator
{: #install-fixpack-operator-firewall}

You can start applying the fix pack to your network by updating the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator. Log in to your cluster by using the kubectl CLI. You will need to provide the name of the Kubernetes namespace that you created to deploy your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network. If you deployed your network on Kubernetes, you can use the `kubectl get namespace` command to find the name of your namespace. If you deployed the platform on the OpenShift Container Platform, log in to your cluster using the `oc` Cli. You can find the name of your OpenShift project using the `oc get project` command. Use the project name as the value for `<namespace>`.

Run the following command to download the operator deployment spec to your local file system. The default name of the operator deployment is `ibm-hlfsupport-operator`. If you changed the name during the deployment process, you can use the `kubectl get deployment -n <namespace>` command to get the name of the deployments on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl set image deploy/ibm-hlfsupport-operator -n <namespace> ibm-hlfsupport-operator=cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211026-amd64
```
{: codeblock}

After you update the operator, it will restart and pull the latest operator image. While the operator is restarting, you can still access your console UI. However, you cannot use the console to deploy smart contracts, or use the console or the APIs to create or remove a node.

Verify that the operator was successfully updated by running the command:

```
kubectl get deployment ibm-hlfsupport-operator
```
{: codeblock}

When the upgrade is successful, the output looks similar to:

```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibm-hlfsupport-operator   1/1       1            1           1m
```
{: codeblock}

If you experience a problem while you are updating the operator, go to this [troubleshooting topic](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-troubleshooting#ibm-hlfsupport-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems.

### Step four: Update the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console
{: #install-fixpack-console-firewall}

After you update to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator, you need to apply the fix pack to your console. You can update your console by removing the original ConfigMap and deployment spec that was created when the console was deployed. Removing these artifacts will allow your console to pull the latest configuration and images from the updated operator.

You can start by running the following command to delete the ConfigMap used by the console deployer. The default name of the deployer ConfigMap is `ibpconsole-deployer`. If you changed the name of the console during the deployment process, you can use the `kubectl get configmap -n <namespace>` command to get the list of ConfigMaps on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl delete configmap -n <namespace> ibpconsole-deployer
```
{: codeblock}

You can then edit the console custom resource and delete the versions from spec. The default name of the console is `ipconsole`.  You can find the list of names for your console by using the `kubectl get ibpconsole -n <namespace>` command. Edit the custom resource using the following command:
```
kubectl edit ibpconsole ibpconsole -n <namespace>
```
{: codeblock}

Following with the console custom resource editing, you can now delete the console deployment using the following command:
```
kubectl delete deployment -n <namespace> ibpconsole
```
{: codeblock}

After you edit the custome resource, and delete the deployment and ConfigMap, the console will restart and download the new images and configuration settings provided by the 1.0.0 fix pack from the updated operator. You can use the following commands to confirm that the console has been updated with the latest images and configuration. The new images used by the console and your blockchain nodes will have the tags with the date `20211026`.
```
kubectl get deployment -n <namespace> ibpconsole -o yaml
kubectl get configmap -n <namespace> ibpconsole-deployer -o yaml
```
{: codeblock}

You can check that the upgrade is complete by running `kubectl get deployment ibpconsole`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibpconsole     1/1       1            1           1m
```
{: codeblock}

If your console experiences an image pull error, you may need to update the console CR spec with local registry that you used to download the images. Run the following command to download the CR spec of the console:
```
kubectl get ibpconsole ibpconsole -o yaml > console.yaml
```
{: codeblock}

Then add the URL of your local registry to the `spec:` section of `console.yaml`. Replace `<LOCAL_REGISTRY>` with the URL of your local registry:
```
spec:
  registryURL: <LOCAL_REGISTRY>
```
{: codeblock}

Save the updated file as `console-upgrade.yaml` on your local system. You can then issue the following command upgrade your console:
```
kubectl apply -f console-upgrade.yaml
```
{: codeblock}

### Step five: Update your blockchain nodes
{: #install-fixpack-nodes-firewall}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. For more information, see [Upgrade your blockchain nodes](#install-fixpack-nodes).


