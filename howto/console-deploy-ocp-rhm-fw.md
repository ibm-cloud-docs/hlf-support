---

copyright:
  years: 2021
lastupdated: "2021-09-10"

keywords: OpenShift, IBM Blockchain Platform console, deploy, Red Hat Marketplace, subscription, operators, on-prem, firewall, airgap environment, container registry, portable storage, Bastion server

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





# Deploy from Red Hat Marketplace (airgap installation)
{: #deploy-ocp-rhm-fw}

The Red Hat Marketplace can be used to deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 operator onto an OpenShift Container Platform 4.4+ cluster behind a firewall. This operator can then be used to deploy an instance of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console that can be used to deploy and manage the blockchain components on your network. These deployment instructions are only for installing {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric in an airgap environment.
{: shortdesc}

There are three ways to deploy the platform in an airgap environment:
1. **[Preferred] Deploy using Bastion server**  
  A Bastion server is a device that has access to both the public internet and an internal registry on an OpenShift Container Platform cluster that resides behind a firewall. Using the Bastion server, the blockchain images can be replicated through the Bastion server directly to the internal registry behind the firewall.

2. **Install using portable compute device** (an intermediate server)  
  A portable compute device, such as a laptop, can be used to download the blockchain images from the entitled registry to a local container registry on the device. Then, the device can be brought behind the firewall and the images can be copied from the local registry on the device to the internal registry behind the firewall.

3. **Install using portable storage device**  
  A portable storage device, such as a hard drive, can be connected to a compute device external to firewall to download the images. This portable storage can then be connected to a device behind the firewall so that the images can be loaded to the internal registry.

This process is only available for x86_64 infrastructure.
{: note}

## Before you begin
{: #deploy-ocp-rhm-fw-before}

- **OpenShift cluster** - These instructions assume you already have a Kubernetes cluster available in OpenShift Container Platform v4.4+ and that you have created a project for your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment. Unsure how to create a project? See [Create a project for your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment](#deploy-ocp-rhm-fw-project).

- **Obtain your entitlement key** - Browse to the [Red Hat Marketplace](https://marketplace.redhat.com/en-us){: external} and log in or create a new account. Click your account then **Pull secrets**. Click **Create pull secret** and give it a name. Copy the pull secret, secure it, and then click **Save** to be used in a later step.

- **Internal container registry** - You need an internal container registry that can host images on your airgap cluster.

- **Local registry** - If not using a Bastion server, you need a local container registry available with access to the public internet on an intermediate server.

- **CLI configured** - The following CLIs must be configured:
    - [oc](https://docs.openshift.com/container-platform/4.6/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli){: external}
    - [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external}
    - [cloudctl](https://github.com/IBM/cloud-pak-cli#download){: external}
    - [Docker](https://docs.docker.com/get-docker/){: external} (only if using an intermediate server)
    - [OpenSSL](https://www.openssl.org/source/){: external}
    - [htpasswd](http://httpd.apache.org/docs/2.4/programs/htpasswd.html){: external}

Choose your deployment option:
- [Deploy {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric in an airgap environment by using a Bastion server](#deploy-ocp-rhm-fw-bastion)
- [Deploy {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric in an airgap environment by using a portable compute device](#deploy-ocp-rhm-fw-portable)
- [Deploy {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric in an airgap environment by using a portable storage device](#deploy-ocp-rhm-fw-storage)

## Deploy using a Bastion server
{: #deploy-ocp-rhm-fw-bastion}

Use these steps when you have configured a Bastion server between the public internet and your airgap environment.

### Overview
{: #deploy-ocp-rhm-fw-bastion-overview}

- [Set up environment variables](#deploy-ocp-rhm-fw-bastion-env)
- [Download the case image inventory](#deploy-ocp-rhm-fw-bastion-case)
- [Mirror the images](#deploy-ocp-rhm-fw-bastion-mirror)
- [Configure global pull secret and insecure registry on the cluster](#deploy-ocp-rhm-fw-bastion-global)
- [Install {{site.data.keyword.blockchainfull_notm}} operator catalog](#deploy-ocp-rhm-fw-bastion-operator)

### Set up environment variables
{: #deploy-ocp-rhm-fw-bastion-env}

Review the following parameters for your environment and then run the following commands to set up the environment.

```sh
export NS=cp # Namespace of target installation on OpenShift cluster
export ITEM=ibmBlockchainPlatformOperatorSetup
export CASE_NAME=ibm-blockchain
export CASE_VERSION=2.4.0
export CASE_ARCHIVE=${CASE_NAME}-${CASE_VERSION}.tgz
export CASEPATH="https://github.com/IBM/cloud-pak/raw/master/repo/case/${CASE_ARCHIVE}"

export OFFLINEDIR=${HOME}/offline         # Where you want to store the images if not using Bastion server
export OFFLINECASE=${OFFLINEDIR}/ibm-blockchain

# Details of the source registry to copy from
export EXTERNAL_REGISTRY=cp.icr.io        # Source registry url
export EXTERNAL_REGISTRY_USER=username    # Actual username goes here
export EXTERNAL_REGISTRY_PASSWORD="actualkey" # Red Hat Marketplace image pull secret (entitlement key)

# Details of the intermediate registry if not using a Bastion server
export PORTABLE_REGISTRY_HOST=localhost
export PORTABLE_REGISTRY_PORT=5000
export PORTABLE_REGISTRY=${PORTABLE_REGISTRY_HOST}:${PORTABLE_REGISTRY_PORT}
export PORTABLE_REGISTRY_USER="user"      # Actual username goes here
export PORTABLE_REGISTRY_PASSWORD="key"   # Actual API Key goes here
export PORTABLE_REGISTRY_PATH=${OFFLINEDIR}/registry
export PORTABLE_STORAGE_LOCATION=""       # Override

# Details of the target registry to copy to
export INTERNAL_REGISTRY_HOST=""          # Internal registry host
export INTERNAL_REGISTRY_PORT=5000
export INTERNAL_REGISTRY=${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}
export INTERNAL_REGISTRY_USER="user"      # Actual username goes here
export INTERNAL_REGISTRY_PASSWORD="key"   # Actual API Key goes here
```
{: codeblock}

You need to customize the following variables:  
- `NS` - Provide the name of your OpenShift project.
- `OFFLINEDIR` - Provide the folder where you want to store the case inventory images.
- `EXTERNAL_REGISTRY_USER` - Provide the user name associated with your Red Hat Marketplace account.
- `EXTERNAL_REGISTRY_PASSWORD` - Generate and paste your Red Hat Marketplace account pull secret.
- `PORTABLE_REGISTRY_HOST` - (Optional) Provide the hostname of your intermediate server, if using.
- `PORTABLE_REGISTRY_USER` - (Optional) Provide the name of the user with access to the portable registry, if using an intermediate server.
- `PORTABLE_REGISTRY_PASSWORD` - (Optional) Provide the password for the portable registry, if using an intermediate server. 
- `INTERNAL_REGISTRY_HOST` - Provide the hostname of the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PORT` - Provide the port of the internal registry in the airgap environment .
- `INTERNAL_REGISTRY_USER` - Provide the name of the user with access to the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PASSWORD` - Provide the password for user with access to the internal registry in the airgap environment.

The `OFFLINEDIR` folder (`${HOME}/offline`) is not automatically generated. You need to create this folder in your local environment before proceeding with the rest of the instructions.
{: note}

**Set the source and target registries.**

For this option, the source registry is the entitled registry `cp.icr.io` and the target registry is the registry behind the firewall. The Bastion server pulls the images from the entitled registry and pushes them to the container registry behind the firewall. Run the following command to set up the environment variables:

```sh
export SOURCE_REGISTRY=${EXTERNAL_REGISTRY}
export SOURCE_REGISTRY_USER=${EXTERNAL_REGISTRY_USER}
export SOURCE_REGISTRY_PASS=${EXTERNAL_REGISTRY_PASSWORD}

export TARGET_REGISTRY=${INTERNAL_REGISTRY}
export TARGET_REGISTRY_USER=${INTERNAL_REGISTRY_USER}
export TARGET_REGISTRY_PASS=${INTERNAL_REGISTRY_PASSWORD}
```
{: codeblock}

### Download the case image inventory
{: #deploy-ocp-rhm-fw-bastion-case}

Download and unpack the case image inventory by running the following commands:

```sh
cloudctl case save \
--case ${CASEPATH} \
--outputdir ${OFFLINEDIR}

tar -xvf ${OFFLINEDIR}/${CASE_ARCHIVE} --directory ${OFFLINEDIR}
```
{: codeblock}

### Mirror the images
{: #deploy-ocp-rhm-fw-bastion-mirror}

1. Set up the source registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${SOURCE_REGISTRY} --user ${SOURCE_REGISTRY_USER} --pass ${SOURCE_REGISTRY_PASS}"
  ```
  {: codeblock}
2. Set up the target registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${TARGET_REGISTRY} --user ${TARGET_REGISTRY_USER} --pass ${TARGET_REGISTRY_PASS}"
  ```
  {: codeblock}
3. Mirror the images by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action mirror-images \
  --args "--registry ${TARGET_REGISTRY} --inputDir ${OFFLINEDIR}"
  ```
  {: codeblock}

You can expect this last step to take a few minutes while it downloads the images.

### Configure global pull secret and insecure registry on the cluster
{: #deploy-ocp-rhm-fw-bastion-global}

1. Set up a global image pull secret by running the following command:
    ```sh
    cloudctl case launch \
    --case ${OFFLINECASE} \
    --inventory ${ITEM} \
    --action configure-cluster-airgap \
    --namespace ${NS} \
    --args "--registry ${INTERNAL_REGISTRY} --user ${INTERNAL_REGISTRY_USER} --pass ${INTERNAL_REGISTRY_PASSWORD} --inputDir ${OFFLINEDIR}"
    ```
    {: codeblock}
2. (Optional) Add the local registry to cluster insecureRegistries list if your local registry is not secured by a certificate. All the nodes will restart one at a time after the following command:
    ```sh
    oc patch image.config.openshift.io/cluster --type=merge -p "{\"spec\":{\"registrySources\":{\"insecureRegistries\":[\"${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}\", \"${INTERNAL_REGISTRY_HOST}\"]}}}"
    ```
    {: codeblock}
3. Wait until all the nodes of your cluster are restarted before you proceed with the next step.

### Install {{site.data.keyword.blockchainfull_notm}} operator catalog
{: #deploy-ocp-rhm-fw-bastion-operator}

1. Install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric catalog to your cluster by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action install-catalog \
  --namespace ${NS} \
  --args "--registry ${INTERNAL_REGISTRY}"
  ```
  {: codeblock}  
2. Verify that the IBM Blockchain catalog is installed successfully in the cluster by running the following commands:
  ```sh
  # Following command should show ibm-blockchain catalog pod
  oc get pods -n openshift-marketplace
  # Following command should show the ibm-blockchain catalog source installed
  oc get catalogsource -n openshift-marketplace
  ```
  {: codeblock}  
  The output of this command looks similar to:
  ```
  # oc get pods -n openshift-marketplace
  NAME                                                              READY   STATUS      RESTARTS   AGE
  certified-operators-868d6dbfcf-66kvc                              1/1     Running     0          95m
  community-operators-94bf6c85b-48mjv                               1/1     Running     0          5h35m
  eed49b0f70894a9e070ba29eb2604d8030764df3298f45f2d484f1311cxlvxq   0/1     Completed   0          42s
  ibm-blockchain-catalog-gkdsc                                      1/1     Running     0          90s
  marketplace-operator-78f6f57c5b-cvf78                             1/1     Running     0          16h
  redhat-marketplace-64ff89cf7d-g5bnh                               1/1     Running     0          16h
  redhat-operators-f8bd8d49-99h64                                   1/1     Running     0          3h35m
  ```
  It can take a few minutes for the catalog to load. When the command is successful, you should see `ibm-blockchain-catalog-nnnnn` with a **STATUS** of **Running**. The catalog should now be available under the **Operators** tab of your OpenShift dashboard.
3. You are now ready to deploy the blockchain console from the OpenShift dashboard. Skip to [Deploy the IBM Blockchain Platform console](#console-deploy-ocp-rhm-fw-console) to continue the deployment in your airgap environment.

## Deploy using a portable compute device
{: #deploy-ocp-rhm-fw-portable}

Use these steps if you plan to download the images to a portable compute device that is connected to the public internet and then bring the device behind firewall where the images can then be copied to the internal container registry in the airgap environment.

### Overview
{: #deploy-ocp-rhm-fw-portable-overview}

- [Copy the images to the local container registry on portable compute device](#deploy-ocp-rhm-fw-portable-compute)
- [Copy the images to the container registry behind the firewall](#deploy-ocp-rhm-fw-portable-compute)
- [Configure global pull secret and insecure registry on the cluster](#deploy-ocp-rhm-fw-portable-global)
- [Install {{site.data.keyword.blockchainfull_notm}} operator catalog](#deploy-ocp-rhm-fw-portable-operator)

### Copy the images to the local container registry on portable compute device
{: #deploy-ocp-rhm-fw-portable-compute}

#### Set up environment variables
{: #deploy-ocp-rhm-fw-portable-env}

Review the following parameters for your environment and then run the following commands to set up the environment.

```sh
export NS=cp # Namespace of target installation on OpenShift cluster
export ITEM=ibmBlockchainPlatformOperatorSetup
export CASE_NAME=ibm-blockchain
export CASE_VERSION=2.4.0
export CASE_ARCHIVE=${CASE_NAME}-${CASE_VERSION}.tgz
export CASEPATH="https://github.com/IBM/cloud-pak/raw/master/repo/case/${CASE_ARCHIVE}"

export OFFLINEDIR=${HOME}/offline         # Where you want to store the images if not using Bastion server
export OFFLINECASE=${OFFLINEDIR}/ibm-blockchain

# Details of the source registry to copy from
export EXTERNAL_REGISTRY=cp.icr.io        # Source registry url
export EXTERNAL_REGISTRY_USER=username    # Actual username goes here
export EXTERNAL_REGISTRY_PASSWORD="actualkey" # Red Hat Marketplace image pull secret (entitlement key)

# Details of the intermediate registry if not using a Bastion server
export PORTABLE_REGISTRY_HOST=localhost
export PORTABLE_REGISTRY_PORT=5000
export PORTABLE_REGISTRY=${PORTABLE_REGISTRY_HOST}:${PORTABLE_REGISTRY_PORT}
export PORTABLE_REGISTRY_USER="user"      # Actual username goes here
export PORTABLE_REGISTRY_PASSWORD="key"   # Actual API Key goes here
export PORTABLE_REGISTRY_PATH=${OFFLINEDIR}/registry
export PORTABLE_STORAGE_LOCATION=""       # Override

# Details of the target registry to copy to
export INTERNAL_REGISTRY_HOST=""          # Internal registry host
export INTERNAL_REGISTRY_PORT=5000
export INTERNAL_REGISTRY=${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}
export INTERNAL_REGISTRY_USER="user"      # Actual username goes here
export INTERNAL_REGISTRY_PASSWORD="key"   # Actual API Key goes here
```
{: codeblock}

You need to customize the following variables:  
- `NS` - Provide the name of your OpenShift project.
- `OFFLINEDIR` - Provide the folder where you want to store the case inventory images.
- `EXTERNAL_REGISTRY_USER` - Provide the user name associated with your Red Hat Marketplace account.
- `EXTERNAL_REGISTRY_PASSWORD` - Generate and paste your Red Hat Marketplace account pull secret.
- `PORTABLE_REGISTRY_HOST` - (Optional) Provide the hostname of your intermediate server, if using.
- `PORTABLE_REGISTRY_USER` - (Optional) Provide the name of the user with access to the portable registry, if using an intermediate server.
- `PORTABLE_REGISTRY_PASSWORD` - (Optional) Provide the password for the portable registry, if using an intermediate server. 
- `INTERNAL_REGISTRY_HOST` - Provide the hostname of the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PORT` - Provide the port of the internal registry in the airgap environment .
- `INTERNAL_REGISTRY_USER` - Provide the name of the user with access to the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PASSWORD` - Provide the password for user with access to the internal registry in the airgap environment.

The `OFFLINEDIR` folder (`${HOME}/offline`) is not automatically generated. You need to create this folder in your local environment before proceeding with the rest of the instructions.
{: note}

**Set the source and target registries**  

The source container registry is the entitled registry `cp.icr.io` and the destination is the local registry on the compute device, for example `localhost:5000`. You need to set up the environment variables so that you can mirror the images to a portable compute device by running the following command:

```sh
export SOURCE_REGISTRY=${EXTERNAL_REGISTRY}
export SOURCE_REGISTRY_USER=${EXTERNAL_REGISTRY_USER}
export SOURCE_REGISTRY_PASS=${EXTERNAL_REGISTRY_PASSWORD}

export TARGET_REGISTRY=${PORTABLE_REGISTRY}
export TARGET_REGISTRY_USER=${PORTABLE_REGISTRY_USER}
export TARGET_REGISTRY_PASS=${PORTABLE_REGISTRY_PASSWORD}
```
{: codeblock}

#### Download the case image inventory
{: #deploy-ocp-rhm-fw-portable-case}

Download and unpack the case image inventory by running the following commands:

```sh
cloudctl case save \
--case ${CASEPATH} \
--outputdir ${OFFLINEDIR}

tar -xvf ${OFFLINEDIR}/${CASE_ARCHIVE} --directory ${OFFLINEDIR}
```
{: codeblock}

#### Start local registry
{: #deploy-ocp-rhm-fw-portable-local1}

1. Initialize the prerequisites to run the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action init-registry \
   --args "--registry $PORTABLE_REGISTRY_HOST --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}
2. Start the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action start-registry \
   --args "--registry $PORTABLE_REGISTRY --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}

#### Mirror the images
{: #deploy-ocp-rhm-fw-portable-mirror}

1. Set up the source registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${SOURCE_REGISTRY} --user ${SOURCE_REGISTRY_USER} --pass ${SOURCE_REGISTRY_PASS}"
  ```
  {: codeblock}
2. Set up the target registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${TARGET_REGISTRY} --user ${TARGET_REGISTRY_USER} --pass ${TARGET_REGISTRY_PASS}"
  ```
  {: codeblock}
3. Mirror the images by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action mirror-images \
  --args "--registry ${TARGET_REGISTRY} --inputDir ${OFFLINEDIR}"
  ```
  {: codeblock}

You can expect this last step to take a few minutes while it downloads the images.

### Copy the images to the container registry behind the firewall
{: #deploy-ocp-rhm-fw-portable-compute}

After you have connected the portable compute device to the network behind the firewall, you can complete the following steps on the device.

#### Set up environment variables
{: #deploy-ocp-rhm-fw-portable-env-fw}

Review the following parameters for your environment and then run the following commands to set up the environment.

```sh
export NS=cp # Namespace of target installation on OpenShift cluster
export ITEM=ibmBlockchainPlatformOperatorSetup
export CASE_NAME=ibm-blockchain
export CASE_VERSION=2.4.0
export CASE_ARCHIVE=${CASE_NAME}-${CASE_VERSION}.tgz
export CASEPATH="https://github.com/IBM/cloud-pak/raw/master/repo/case/${CASE_ARCHIVE}"

export OFFLINEDIR=${HOME}/offline         # Where you want to store the images if not using Bastion server
export OFFLINECASE=${OFFLINEDIR}/ibm-blockchain

# Details of the source registry to copy from
export EXTERNAL_REGISTRY=cp.icr.io        # Source registry url
export EXTERNAL_REGISTRY_USER=username    # Actual username goes here
export EXTERNAL_REGISTRY_PASSWORD="actualkey" # Red Hat Marketplace image pull secret (entitlement key)

# Details of the intermediate registry if not using a Bastion server
export PORTABLE_REGISTRY_HOST=localhost
export PORTABLE_REGISTRY_PORT=5000
export PORTABLE_REGISTRY=${PORTABLE_REGISTRY_HOST}:${PORTABLE_REGISTRY_PORT}
export PORTABLE_REGISTRY_USER="user"      # Actual username goes here
export PORTABLE_REGISTRY_PASSWORD="key"   # Actual API Key goes here
export PORTABLE_REGISTRY_PATH=${OFFLINEDIR}/registry
export PORTABLE_STORAGE_LOCATION=""       # Override

# Details of the target registry to copy to
export INTERNAL_REGISTRY_HOST=""          # Internal registry host
export INTERNAL_REGISTRY_PORT=5000
export INTERNAL_REGISTRY=${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}
export INTERNAL_REGISTRY_USER="user"      # Actual username goes here
export INTERNAL_REGISTRY_PASSWORD="key"   # Actual API Key goes here
```
{: codeblock}

You need to customize the following variables:  
- `NS` - Provide the name of your OpenShift project.
- `OFFLINEDIR` - Provide the folder where you want to store the case inventory images.
- `EXTERNAL_REGISTRY_USER` - Provide the user name associated with your Red Hat Marketplace account.
- `EXTERNAL_REGISTRY_PASSWORD` - Generate and paste your Red Hat Marketplace account pull secret.
- `PORTABLE_REGISTRY_HOST` - (Optional) Provide the hostname of your intermediate server, if using.
- `PORTABLE_REGISTRY_USER` - (Optional) Provide the name of the user with access to the portable registry, if using an intermediate server.
- `PORTABLE_REGISTRY_PASSWORD` - (Optional) Provide the password for the portable registry, if using an intermediate server. 
- `INTERNAL_REGISTRY_HOST` - Provide the hostname of the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PORT` - Provide the port of the internal registry in the airgap environment .
- `INTERNAL_REGISTRY_USER` - Provide the name of the user with access to the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PASSWORD` - Provide the password for user with access to the internal registry in the airgap environment.

The `OFFLINEDIR` folder (`${HOME}/offline`) is not automatically generated. You need to create this folder in your local environment before proceeding with the rest of the instructions.
{: note}

**Set the source and target registries**  

The source container registry is now the local registry on compute, for example `localhost:5000` and the destination is the registry behind the firewall, for example `10.10.4.6:5000`, or the host and port in your airgap environment. You need to set up the environment variables, mirror the images, and then install the catalog. Run the following command to set up the environment variables:

```sh
export SOURCE_REGISTRY=${PORTABLE_REGISTRY}
export SOURCE_REGISTRY_USER=${PORTABLE_REGISTRY_USER}
export SOURCE_REGISTRY_PASS=${PORTABLE_REGISTRY_PASSWORD}

export TARGET_REGISTRY=${INTERNAL_REGISTRY}
export TARGET_REGISTRY_USER=${INTERNAL_REGISTRY_USER}
export TARGET_REGISTRY_PASS=${INTERNAL_REGISTRY_PASSWORD}
```
{: codeblock}

#### Start local registry
{: #deploy-ocp-rhm-fw-portable-local2}

1. Initialize the prerequisites to run the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action init-registry \
   --args "--registry $PORTABLE_REGISTRY_HOST --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}
2. Start the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action start-registry \
   --args "--registry $PORTABLE_REGISTRY --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}

#### Mirror the images
{: #deploy-ocp-rhm-fw-portable-mirror2}

1. Set up the source registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${SOURCE_REGISTRY} --user ${SOURCE_REGISTRY_USER} --pass ${SOURCE_REGISTRY_PASS}"
  ```
  {: codeblock}
2. Set up the target registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${TARGET_REGISTRY} --user ${TARGET_REGISTRY_USER} --pass ${TARGET_REGISTRY_PASS}"
  ```
  {: codeblock}
3. Mirror the images by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action mirror-images \
  --args "--registry ${TARGET_REGISTRY} --inputDir ${OFFLINEDIR}"
  ```
  {: codeblock}

You can expect this last step to take a few minutes while it downloads the images.

### Configure global pull secret and insecure registry on the cluster
{: #deploy-ocp-rhm-fw-portable-global}

1. Set up a global image pull secret by running the following command:
    ```sh
    cloudctl case launch \
    --case ${OFFLINECASE} \
    --inventory ${ITEM} \
    --action configure-cluster-airgap \
    --namespace ${NS} \
    --args "--registry ${INTERNAL_REGISTRY} --user ${INTERNAL_REGISTRY_USER} --pass ${INTERNAL_REGISTRY_PASSWORD} --inputDir ${OFFLINEDIR}"
    ```
    {: codeblock}
2. (Optional) Add the local registry to cluster insecureRegistries list if your local registry is not secured by a certificate. All the nodes will restart one at a time after the following command:
    ```sh
    oc patch image.config.openshift.io/cluster --type=merge -p "{\"spec\":{\"registrySources\":{\"insecureRegistries\":[\"${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}\", \"${INTERNAL_REGISTRY_HOST}\"]}}}"
    ```
    {: codeblock}
3. Wait until all the nodes of your cluster are restarted before you proceed with the next step.

### Install {{site.data.keyword.blockchainfull_notm}} operator catalog
{: #deploy-ocp-rhm-fw-portable-operator}

1. Install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric catalog to your cluster by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action install-catalog \
  --namespace ${NS} \
  --args "--registry ${INTERNAL_REGISTRY}"
  ```
  {: codeblock}  
2. Verify that the IBM Blockchain catalog is installed successfully in the cluster by running the following commands:
  ```sh
  # Following command should show ibm-blockchain catalog pod
  oc get pods -n openshift-marketplace
  # Following command should show the ibm-blockchain catalog source installed
  oc get catalogsource -n openshift-marketplace
  ```
  {: codeblock}  
  The output of this command looks similar to:
  ```
  # oc get pods -n openshift-marketplace
  NAME                                                              READY   STATUS      RESTARTS   AGE
  certified-operators-868d6dbfcf-66kvc                              1/1     Running     0          95m
  community-operators-94bf6c85b-48mjv                               1/1     Running     0          5h35m
  eed49b0f70894a9e070ba29eb2604d8030764df3298f45f2d484f1311cxlvxq   0/1     Completed   0          42s
  ibm-blockchain-catalog-gkdsc                                      1/1     Running     0          90s
  marketplace-operator-78f6f57c5b-cvf78                             1/1     Running     0          16h
  redhat-marketplace-64ff89cf7d-g5bnh                               1/1     Running     0          16h
  redhat-operators-f8bd8d49-99h64                                   1/1     Running     0          3h35m
  ```
  It can take a few minutes for the catalog to load. When the command is successful, you should see `ibm-blockchain-catalog-nnnnn` with a **STATUS** of **Running**. The catalog should now be available under the **Operators** tab of your OpenShift dashboard.
3. You are now ready to deploy the blockchain console from the OpenShift dashboard. Skip to [Deploy the IBM Blockchain Platform console](#console-deploy-ocp-rhm-fw-console) to continue the deployment in your airgap environment.

## Deploy using a portable storage device
{: #deploy-ocp-rhm-fw-storage}

Use these steps when you want to download the images to portable storage device connected to a compute device external to the firewall. This portable storage can then be connected to a device behind the firewall and the images can be loaded from the storage device to the internal registry in the airgap environment.

### Overview
{: #deploy-ocp-rhm-fw-storage-overview}

- [Copy the images to the local container registry on the external compute](#deploy-ocp-rhm-fw-storage-compute)
- [Copy the images to the container registry behind the firewall](#deploy-ocp-rhm-fw-storage-fw)
- [Configure global pull secret and insecure registry on the cluster](#deploy-ocp-rhm-fw-storage-global)
- [Install {{site.data.keyword.blockchainfull_notm}} operator catalog](#deploy-ocp-rhm-fw-storage-operator)

### Copy the images to the local container registry on the external compute
{: #deploy-ocp-rhm-fw-storage-compute}

Mount the storage volume on the external compute.

#### Configure environment variables
{: #deploy-ocp-rhm-fw-storage-env}

Review the following parameters for your environment and then run the following commands to set up the environment.

```sh
export NS=cp # Namespace of target installation on OpenShift cluster
export ITEM=ibmBlockchainPlatformOperatorSetup
export CASE_NAME=ibm-blockchain
export CASE_VERSION=2.4.0
export CASE_ARCHIVE=${CASE_NAME}-${CASE_VERSION}.tgz
export CASEPATH="https://github.com/IBM/cloud-pak/raw/master/repo/case/${CASE_ARCHIVE}"

export OFFLINEDIR=${HOME}/offline         # Where you want to store the images if not using Bastion server
export OFFLINECASE=${OFFLINEDIR}/ibm-blockchain

# Details of the source registry to copy from
export EXTERNAL_REGISTRY=cp.icr.io        # Source registry url
export EXTERNAL_REGISTRY_USER=username    # Actual username goes here
export EXTERNAL_REGISTRY_PASSWORD="actualkey" # Red Hat Marketplace image pull secret (entitlement key)

# Details of the intermediate registry if not using a Bastion server
export PORTABLE_REGISTRY_HOST=localhost
export PORTABLE_REGISTRY_PORT=5000
export PORTABLE_REGISTRY=${PORTABLE_REGISTRY_HOST}:${PORTABLE_REGISTRY_PORT}
export PORTABLE_REGISTRY_USER="user"      # Actual username goes here
export PORTABLE_REGISTRY_PASSWORD="key"   # Actual API Key goes here
export PORTABLE_REGISTRY_PATH=${OFFLINEDIR}/registry
export PORTABLE_STORAGE_LOCATION=""       # Override

# Details of the target registry to copy to
export INTERNAL_REGISTRY_HOST=""          # Internal registry host
export INTERNAL_REGISTRY_PORT=5000
export INTERNAL_REGISTRY=${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}
export INTERNAL_REGISTRY_USER="user"      # Actual username goes here
export INTERNAL_REGISTRY_PASSWORD="key"   # Actual API Key goes here
```
{: codeblock}

You need to customize the following variables:  
- `NS` - Provide the name of your OpenShift project.
- `OFFLINEDIR` - Provide the folder where you want to store the case inventory images.
- `EXTERNAL_REGISTRY_USER` - Provide the user name associated with your Red Hat Marketplace account.
- `EXTERNAL_REGISTRY_PASSWORD` - Generate and paste your Red Hat Marketplace account pull secret.
- `PORTABLE_REGISTRY_HOST` - (Optional) Provide the hostname of your intermediate server, if using.
- `PORTABLE_REGISTRY_USER` - (Optional) Provide the name of the user with access to the portable registry, if using an intermediate server.
- `PORTABLE_REGISTRY_PASSWORD` - (Optional) Provide the password for the portable registry, if using an intermediate server. 
- `INTERNAL_REGISTRY_HOST` - Provide the hostname of the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PORT` - Provide the port of the internal registry in the airgap environment .
- `INTERNAL_REGISTRY_USER` - Provide the name of the user with access to the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PASSWORD` - Provide the password for user with access to the internal registry in the airgap environment.

The `OFFLINEDIR` folder (`${HOME}/offline`) is not automatically generated. You need to create this folder in your local environment before proceeding with the rest of the instructions.
{: note}

**Set the source and target registries**  

The source container registry is the entitled registry `cp.icr.io` and the destination is the local registry on the compute device, for example `localhost:5000`. You need to set up the environment variables so that you can mirror the images to a portable compute device by running the following command:

```sh
export SOURCE_REGISTRY=${EXTERNAL_REGISTRY}
export SOURCE_REGISTRY_USER=${EXTERNAL_REGISTRY_USER}
export SOURCE_REGISTRY_PASS=${EXTERNAL_REGISTRY_PASSWORD}

export TARGET_REGISTRY=${PORTABLE_REGISTRY}
export TARGET_REGISTRY_USER=${PORTABLE_REGISTRY_USER}
export TARGET_REGISTRY_PASS=${PORTABLE_REGISTRY_PASSWORD}
```
{: codeblock}

#### Download the case image inventory
{: #deploy-ocp-rhm-fw-storage-case}

Download and unpack the case image inventory by running the following commands:

```sh
cloudctl case save \
--case ${CASEPATH} \
--outputdir ${OFFLINEDIR}

tar -xvf ${OFFLINEDIR}/${CASE_ARCHIVE} --directory ${OFFLINEDIR}
```
{: codeblock}

#### Start local registry
{: #deploy-ocp-rhm-fw-storage-local1}

1. Initialize the prerequisites to run the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action init-registry \
   --args "--registry $PORTABLE_REGISTRY_HOST --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}
2. Start the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action start-registry \
   --args "--registry $PORTABLE_REGISTRY --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}

#### Mirror the images
{: #deploy-ocp-rhm-fw-storage-mirror}

1. Set up the source registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${SOURCE_REGISTRY} --user ${SOURCE_REGISTRY_USER} --pass ${SOURCE_REGISTRY_PASS}"
  ```
  {: codeblock}
2. Set up the target registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${TARGET_REGISTRY} --user ${TARGET_REGISTRY_USER} --pass ${TARGET_REGISTRY_PASS}"
  ```
  {: codeblock}
3. Mirror the images by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action mirror-images \
  --args "--registry ${TARGET_REGISTRY} --inputDir ${OFFLINEDIR}"
  ```
  {: codeblock}

You can expect this last step to take a few minutes while it downloads the images.

#### Copy case and registry data to portable storage
{: #deploy-ocp-rhm-fw-storage-copy}

Run the following command to copy the offline case inventory images and registry data folder to the portable storage device:

```sh
cp -r ${OFFLINEDIR} ${PORTABLE_STORAGE_LOCATION}
```
{: codeblock}

### Copy the images to the container registry behind the firewall
{: #deploy-ocp-rhm-fw-storage-fw}

After you have connected the portable storage to a compute node behind the firewall, you can complete the following steps on that node.

#### Configure environment variables
{: #deploy-ocp-rhm-fw-storage-env-fw}

Review the following parameters for your environment and then run the following commands to set up the environment.

```sh
export NS=cp # Namespace of target installation on OpenShift cluster
export ITEM=ibmBlockchainPlatformOperatorSetup
export CASE_NAME=ibm-blockchain
export CASE_VERSION=2.4.0
export CASE_ARCHIVE=${CASE_NAME}-${CASE_VERSION}.tgz
export CASEPATH="https://github.com/IBM/cloud-pak/raw/master/repo/case/${CASE_ARCHIVE}"

export OFFLINEDIR=${HOME}/offline         # Where you want to store the images if not using Bastion server
export OFFLINECASE=${OFFLINEDIR}/ibm-blockchain

# Details of the source registry to copy from
export EXTERNAL_REGISTRY=cp.icr.io        # Source registry url
export EXTERNAL_REGISTRY_USER=username    # Actual username goes here
export EXTERNAL_REGISTRY_PASSWORD="actualkey" # Red Hat Marketplace image pull secret (entitlement key)

# Details of the intermediate registry if not using a Bastion server
export PORTABLE_REGISTRY_HOST=localhost
export PORTABLE_REGISTRY_PORT=5000
export PORTABLE_REGISTRY=${PORTABLE_REGISTRY_HOST}:${PORTABLE_REGISTRY_PORT}
export PORTABLE_REGISTRY_USER="user"      # Actual username goes here
export PORTABLE_REGISTRY_PASSWORD="key"   # Actual API Key goes here
export PORTABLE_REGISTRY_PATH=${OFFLINEDIR}/registry
export PORTABLE_STORAGE_LOCATION=""       # Override

# Details of the target registry to copy to
export INTERNAL_REGISTRY_HOST=""          # Internal registry host
export INTERNAL_REGISTRY_PORT=5000
export INTERNAL_REGISTRY=${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}
export INTERNAL_REGISTRY_USER="user"      # Actual username goes here
export INTERNAL_REGISTRY_PASSWORD="key"   # Actual API Key goes here
```
{: codeblock}

You need to customize the following variables:  
- `NS` - Provide the name of your OpenShift project.
- `OFFLINEDIR` - Provide the folder where you want to store the case inventory images.
- `EXTERNAL_REGISTRY_USER` - Provide the user name associated with your Red Hat Marketplace account.
- `EXTERNAL_REGISTRY_PASSWORD` - Generate and paste your Red Hat Marketplace account pull secret.
- `PORTABLE_REGISTRY_HOST` - (Optional) Provide the hostname of your intermediate server, if using.
- `PORTABLE_REGISTRY_USER` - (Optional) Provide the name of the user with access to the portable registry, if using an intermediate server.
- `PORTABLE_REGISTRY_PASSWORD` - (Optional) Provide the password for the portable registry, if using an intermediate server. 
- `INTERNAL_REGISTRY_HOST` - Provide the hostname of the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PORT` - Provide the port of the internal registry in the airgap environment .
- `INTERNAL_REGISTRY_USER` - Provide the name of the user with access to the internal registry in the airgap environment.
- `INTERNAL_REGISTRY_PASSWORD` - Provide the password for user with access to the internal registry in the airgap environment.

The `OFFLINEDIR` folder (`${HOME}/offline`) is not automatically generated. You need to create this folder in your local environment before proceeding with the rest of the instructions.
{: note}

**Set the source and target registries**  

The source container registry is now the local registry on compute, for example `localhost:5000` and the destination is the registry behind the firewall, for example `10.10.4.6:5000`, or the host and port in your airgap environment. You need to set up the environment variables, mirror the images, and then install the catalog. Run the following command to set up the environment variables:

```sh
export SOURCE_REGISTRY=${PORTABLE_REGISTRY}
export SOURCE_REGISTRY_USER=${PORTABLE_REGISTRY_USER}
export SOURCE_REGISTRY_PASS=${PORTABLE_REGISTRY_PASSWORD}

export TARGET_REGISTRY=${INTERNAL_REGISTRY}
export TARGET_REGISTRY_USER=${INTERNAL_REGISTRY_USER}
export TARGET_REGISTRY_PASS=${INTERNAL_REGISTRY_PASSWORD}
```
{: codeblock}

Run the following command to override the registry storage location to point to the location of the portable storage:
```
export PORTABLE_STORAGE_LOCATION=#Provide external storage path here
```
{: codeblock}

Run the following command to copy the offline case inventory images and registry data folder from the portable storage device to the node.

```
cp -r ${PORTABLE_STORAGE_LOCATION} ${OFFLINEDIR}
```
{: codeblock}

#### Start local registry
{: #deploy-ocp-rhm-fw-storage-local2}

1. Initialize the prerequisites to run the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action init-registry \
   --args "--registry $PORTABLE_REGISTRY_HOST --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}
2. Start the Docker registry by running the following command:

   ```sh
   cloudctl case launch \
   --case ${OFFLINECASE} \
   --inventory ${ITEM} \
   --action start-registry \
   --args "--registry $PORTABLE_REGISTRY --user $PORTABLE_REGISTRY_USER --pass $PORTABLE_REGISTRY_PASSWORD --dir $PORTABLE_REGISTRY_PATH"
   ```
   {: codeblock}

#### Mirror the images
{: #deploy-ocp-rhm-fw-storage-mirror2}

1. Set up the source registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${SOURCE_REGISTRY} --user ${SOURCE_REGISTRY_USER} --pass ${SOURCE_REGISTRY_PASS}"
  ```
  {: codeblock}
2. Set up the target registry credentials by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action configure-creds-airgap \
  --args "--registry ${TARGET_REGISTRY} --user ${TARGET_REGISTRY_USER} --pass ${TARGET_REGISTRY_PASS}"
  ```
  {: codeblock}
3. Mirror the images by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action mirror-images \
  --args "--registry ${TARGET_REGISTRY} --inputDir ${OFFLINEDIR}"
  ```
  {: codeblock}

You can expect this last step to take a few minutes while it downloads the images.

### Configure global pull secret and insecure registry on the cluster
{: #deploy-ocp-rhm-fw-storage-global}

1. Set up a global image pull secret by running the following command:
    ```sh
    cloudctl case launch \
    --case ${OFFLINECASE} \
    --inventory ${ITEM} \
    --action configure-cluster-airgap \
    --namespace ${NS} \
    --args "--registry ${INTERNAL_REGISTRY} --user ${INTERNAL_REGISTRY_USER} --pass ${INTERNAL_REGISTRY_PASSWORD} --inputDir ${OFFLINEDIR}"
    ```
    {: codeblock}
2. (Optional) Add the local registry to cluster insecureRegistries list if your local registry is not secured by a certificate. All the nodes will restart one at a time after the following command:
    ```sh
    oc patch image.config.openshift.io/cluster --type=merge -p "{\"spec\":{\"registrySources\":{\"insecureRegistries\":[\"${INTERNAL_REGISTRY_HOST}:${INTERNAL_REGISTRY_PORT}\", \"${INTERNAL_REGISTRY_HOST}\"]}}}"
    ```
    {: codeblock}
3. Wait until all the nodes of your cluster are restarted before you proceed with the next step.

### Install {{site.data.keyword.blockchainfull_notm}} operator catalog
{: #deploy-ocp-rhm-fw-storage-operator}

1. Install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric catalog to your cluster by running the following command:
  ```sh
  cloudctl case launch \
  --case ${OFFLINECASE} \
  --inventory ${ITEM} \
  --action install-catalog \
  --namespace ${NS} \
  --args "--registry ${INTERNAL_REGISTRY}"
  ```
  {: codeblock}  
2. Verify that the IBM Blockchain catalog is installed successfully in the cluster by running the following commands:
  ```sh
  # Following command should show ibm-blockchain catalog pod
  oc get pods -n openshift-marketplace
  # Following command should show the ibm-blockchain catalog source installed
  oc get catalogsource -n openshift-marketplace
  ```
  {: codeblock}  
  The output of this command looks similar to:
  ```
  # oc get pods -n openshift-marketplace
  NAME                                                              READY   STATUS      RESTARTS   AGE
  certified-operators-868d6dbfcf-66kvc                              1/1     Running     0          95m
  community-operators-94bf6c85b-48mjv                               1/1     Running     0          5h35m
  eed49b0f70894a9e070ba29eb2604d8030764df3298f45f2d484f1311cxlvxq   0/1     Completed   0          42s
  ibm-blockchain-catalog-gkdsc                                      1/1     Running     0          90s
  marketplace-operator-78f6f57c5b-cvf78                             1/1     Running     0          16h
  redhat-marketplace-64ff89cf7d-g5bnh                               1/1     Running     0          16h
  redhat-operators-f8bd8d49-99h64                                   1/1     Running     0          3h35m
  ```
  It can take a few minutes for the catalog to load. When the command is successful, you should see `ibm-blockchain-catalog-nnnnn` with a **STATUS** of **Running**. The catalog should now be available under the **Operators** tab of your OpenShift dashboard.
3. You are now ready to deploy the blockchain console from the OpenShift dashboard. Skip to [Deploy the IBM Blockchain Platform console](#console-deploy-ocp-rhm-fw-console) to continue the deployment in your airgap environment.

## Deploy the IBM Blockchain Platform console
{: #console-deploy-ocp-rhm-fw-console}

Congratulations! You have deployed the IBM Blockchain Platform operator. The only thing left to do is to deploy the console user interface (UI).

1. Log in to your OpenShift dashboard.
2. Click on **Installed Operators** and under **Provider Type** select **{{site.data.keyword.blockchainfull_notm}} Operator Catalog**.
3. Click the **{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric** tile.
4. Click **Install**.
5. Be sure to select the correct namespace to install the operator in.

When it is finished, you are ready to install the **IBPConsole**.

There are four instances available listed under "Provided APIs":


![Blockchain instances available in Red Hat Marketplace](../images/rhm-operators.png "Blockchain instances available in Red Hat Marketplace"){: caption="Figure 1. Blockchain instances available in Red Hat Marketplace" caption-side="bottom"}

- **IBP CA** - (Advanced users) Deploys an instance of an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric CA.
- **IBP Console** - The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console UI, or "console", is an award-winning user interface for building your blockchain network.
- **IBP Orderer** - (Advanced users) Deploys an instance of an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric ordering service.
- **IBP Peer** - (Advanced users) Deploys an instance of an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric peer.

**Which instance should I choose?**

All customers need to deploy an instance of the **IBP Console** to simplify the deployment and management of their blockchain networks. The console itself is free. You are only billed for the blockchain nodes that you create by using the console.

Note that this tutorial only includes instructions for deploying an instance of the **IBP Console**. Currently, you cannot deploy Certificate Authorities (CAs), peers, and ordering nodes directly, you should use the console to deploy those nodes instead.
{: important}

Click **Create Instance** on the **IBPConsole** tile.
![Blockchain instances available in Red Hat Marketplace](../images/IBPConsole.png "Create instance on the IBPConsole tile"){: caption="Figure 2. Click Create Instance on the IBP Console tile" caption-side="bottom"}

The YAML view shows a sample **console** specification of parameters that you need to customize. The spec is abbreviated to _only the required parameters_.  Be aware that some fields can show up differently based on your configuration. Before you install the console, you should also review the Advanced deployment options in the next section in case any of the other options are relevant to your configuration. For example, if you are deploying your console on a multizone cluster, you need to configure that before you install the console.
{: important}


```yaml
apiVersion: ibp.com/v1beta1
kind: IBPConsole
metadata:
  name: ibpconsole
  namespace: example
  labels:
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibp"
    app.kubernetes.io/managed-by: "ibm-ibp"
spec:
  email: <EMAIL>
  password: <PASSWORD>
  imagePullSecrets:
  - regcred
  registryURL: cp.icr.io/cp
  license:
    accept: <ACCEPT>
  networkinfo:
    domain: <DOMAIN>
  storage:
    console:
      class: ''
      size: 5Gi
  serviceAccountName: ibm-blockchain
  version: 1.0.0
```
{: codeblock}

- Accept the IBM Blockchain Platform license by replacing `<ACCEPT>` with the text `true`.
- Replace `<EMAIL>` with the email address that you want to use for the console administrator.
- Replace `<PASSWORD>` with the password of your choice. This password becomes the default password for the console administrator but they are required to change it the first time they log in.
- Replace `<DOMAIN>` with the name of your cluster domain. You can find this value from your OpenShift web console URL. Examine the URL for the current page. It is similar to: `https://console-openshift-console.pa-0803-ocp43-0defdaa0c51bd4a2dcb024eab4bf04a1-0000.us-south.containers.appdomain.cloud/k8s/ns/pa0804/clusterserviceversions/ibm-blockchain.v1.0.0/ibp.com~v1beta1~IBPConsole/~new`. The value of the domain then would be `pa-0803-ocp43-0defdaa0c51bd4a2dcb024eab4bf04a1-0000.us-south.containers.appdomain.cloud`, after you remove `console-openshift-console` and `/k8s/ns/pa0804/clusterserviceversions/ibm-blockchain.v1.0.0/ibp.com~v1beta1~IBPConsole/~new`.  


You also need to make additional edits to the file depending on your choices in the deployment process. For example, if you created a new storage class for your network, provide the storage class that you created to the `class:` field.

If you are running OpenShift on Azure, you also need to change the storage class from `default` to `azure-standard`, unless you created your own storage class.
{: tip}


When you are satisfied with your edits to the specification, click **Create**.

The console can take a few minutes to deploy.
{: note}

After the console has been created, you can verify that the console deployment succeeded. See [Verify the console installation](#console-deploy-ocp-rhm-fw-verify) for instructions on verifying and accessing the console.

### Advanced deployment options
{: #console-deploy-ocp-rhm-fw-advanced}

Before you deploy the console, you can edit console specification to allocate more resources to your console or use zones for high availability in a multizone cluster. To take advantage of these deployment options, you can use the console resource definition with the `resources:` and `clusterdata:` sections added:

```yaml
apiVersion: ibp.com/v1beta1
kind: IBPConsole
metadata:
  name: ibpconsole
  namespace: your-project
  labels:
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibp"
    app.kubernetes.io/managed-by: "ibm-ibp"
spec:
  email: <EMAIL>
  password: <PASSWORD>
  imagePullSecrets:
  - regcred
  registryURL: cp.icr.io/cp
  license:
    accept: <ACCEPT>
  networkinfo:
    domain: <DOMAIN>
  storage:
    console:
      class: default
      size: 5Gi
  serviceAccountName: ibm-blockchain
  version: 1.0.0
  clusterdata:
    zones:
  resources:
    console:
      requests:
        cpu: 500m
        memory: 1000Mi
      limits:
        cpu: 500m
        memory: 1000Mi
    configtxlator:
      limits:
        cpu: 25m
        memory: 50Mi
      requests:
        cpu: 25m
        memory: 50Mi
    couchdb:
      limits:
        cpu: 500m
        memory: 1000Mi
      requests:
        cpu: 500m
        memory: 1000Mi
    deployer:
      limits:
        cpu: 100m
        memory: 200Mi
      requests:
        cpu: 100m
        memory: 200Mi
```
{: codeblock}


- You can use the `resources:` section to allocate more resources to your console. The values in the example file are the default values allocated to each container. Allocating more resources to your console allows you to operate a larger number of nodes or channels.

- If you plan to use the console with a multizone Kubernetes cluster, you need to add the zones to the `clusterdata.zones:` section of the file. When zones are provided to the deployment, you can select the zone that a node is deployed to using the console or the APIs. As an example, if you are deploying to a cluster across the zones of dal10, dal12, and dal13, you would add the zones to the file by using the format below.
  ```yaml
  clusterdata:
    zones:
      - dal10
      - dal12
      - dal13
  ```
  {: codeblock}

When you finish editing the file, click **Create**.

Unlike the resource allocation, you cannot add zones to a running network. If you have already deployed a console and used it to create nodes on your cluster, you will lose your previous work. After the console restarts, you need to deploy new nodes.
{: important}

### Use your own TLS Certificates (Optional)
{: #console-deploy-ocp-rhm-fw-tls}

The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console uses TLS certificates to secure the communication between the console and your blockchain nodes and between the console and your browser. You have the option of creating your own TLS certificates and providing them to the console by using a Kubernetes secret. If you skip this step, the console creates its own self-signed TLS certificates during deployment.

This step needs to be performed before the console is deployed.
{: important}

You can use a Certificate Authority or tool to create the TLS certificates for the console. The TLS certificate needs to include the hostname of the console and the proxy in the subject name or the alternative domain names. The console and proxy hostname are in the following format:

**Console hostname:** ``<PROJECT_NAME>-ibpconsole-console.<DOMAIN>``  
**Proxy hostname:** ``<PROJECT_NAME>-ibpconsole-proxy.<DOMAIN>``  

- Replace `<PROJECT_NAME>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment project.
- Replace `<DOMAIN>` with the name of your cluster domain. You can find this value from your OpenShift web console URL. Examine the URL for the current page. It is similar to: `https://console-openshift-console.pa-0803-ocp43-0defdaa0c51bd4a2dcb024eab4bf04a1-0000.us-south.containers.appdomain.cloud/k8s/ns/pa0804/clusterserviceversions/ibm-blockchain.v2.5.0/ibp.com~v1alpha2~IBPConsole/~new`. The value of the domain then would be `pa-0803-ocp43-0defdaa0c51bd4a2dcb024eab4bf04a1-0000.us-south.containers.appdomain.cloud`, after you remove `console-openshift-console` and `/k8s/ns/pa0804/clusterserviceversions/ibm-blockchain.v2.5.0/ibp.com~v1alpha2~IBPConsole/~new`.

Navigate to the TLS certificates that you plan to use on your local system. Name the TLS certificate `tlscert.pem` and the corresponding private key `tlskey.pem`. Run the following command to create the Kubernetes secret and add it to your OpenShift project. The TLS certificate and key need to be in PEM format.
```
kubectl create secret generic console-tls-secret --from-file=tls.crt=./tlscert.pem --from-file=tls.key=./tlskey.pem -n <PROJECT_NAME>
```
{: codeblock}
Replace `<PROJECT_NAME>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment project.

After you create the secret, add the `tlsSecretName` field to the `spec:` section with one indent added, at the same level as the `resources:` and `clusterdata:` sections of the advanced deployment options. You must provide the name of the TLS secret that you created to the field. The following example deploys a console with the TLS certificate and key that is stored in a secret named `"console-tls-secret"`:

```yaml
apiVersion: ibp.com/v1beta1
kind: IBPConsole
metadata:
  name: ibpconsole
  namespace: your-project
  labels:
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibp"
    app.kubernetes.io/managed-by: "ibm-ibp"
spec:
  email: <EMAIL>
  password: <PASSWORD>
  imagePullSecrets:
  - regcred
  registryURL: cp.icr.io/cp
  license:
    accept: <ACCEPT>
  networkinfo:
    domain: <DOMAIN>
  storage:
    console:
      class: default
      size: 5Gi
  serviceAccountName: ibm-blockchain
  version: 1.0.0
  tlsSecretName: "console-tls-secret"
  clusterdata:
    zones:
      - dal10
      - dal12
      - dal13
```
{: codeblock}

## Verify the console installation
{: #console-deploy-ocp-rhm-fw-verify}

## Log in to the console
{: #console-deploy-ocp-rhm-fw-login}

You can find the URL of your blockchain console from the OpenShift cluster dashboard.

1. Click **Workloads** in the left navigation.
2. Click **Config Maps**.
3. You see several config maps, including `<CONSOLE_NAME>`, `<CONSOLE_NAME>-console`, and a `<CONSOLE_NAME>-deployer`. Click the one that's just the `<CONSOLE_NAME>`, for example, `ibpconsole`.
4. In the **Data** section, you see the **HOST_URL** field. This is the URL of your console that you can now use to log in. It looks similar to the following example:

  ```
  https://blockchain-project-ibpconsole-console.xyz.abc.com
  ```
5. Paste your URL in your browser and login to the console.

In your browser, you can see the console login screen:
- For the **User ID**, use the value you provided for the `email:` field in the console specification in step two.
- For the **Password**, use the value you encoded for the `password:` field in the console specification in step two. This password becomes the default password for the console that all new users use to log in to the console. After you log in for the first time, you are asked to provide a new password that you can use to log in to the console.

Ensure that you are not using the ESR version of Firefox. If you are, switch to another browser such as Chrome and log in.
{: important}

The administrator who provisions the console can grant access to other users and restrict the actions they can perform. For more information, see [Managing users from the console](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-users){: external} in the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 documentation.

## Next steps
{: #console-deploy-ocp-rhm-fw-next}

When you access your console, you can use the **Nodes** tab of your console UI to create CAs, peers, or an ordering service. See the [Build a network tutorial](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network){: external} to get started with the console.

To learn how to manage the users that can access the console, view the logs of your console and your blockchain components, see [Administering your console](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage){: external}.



## Remove your deployment
{: #console-deploy-ocp-rhm-fw-remove-deployment}

If you want to delete the IBM Blockchain Platform operator or any instances you have deployed, the simplest way is through the OpenShift UI.

If you want to delete the operator, make sure to delete any instances associated with it first.
{: important}

To delete an instance, navigate to the **Operators** tab and then click  **Installed Operators**. From here, click **IBM Blockchain** and then the **All instances** tab. Find the instance you that want to delete and click the settings button to the right of the instance. Then, click **Delete** (the name of your instance).

To delete the operator, navigate back to the **Installed Operators** page and click the settings button to the right of the IBM Blockchain Platform operator. Then, click **Uninstall Operator**.

Alternatively, you can use the CLI to switch to the OpenShift project that you created for your blockchain network:

  ```
  oc project <PROJECT_NAME>
  ```
  {: codeblock}

And then remove any instances or the IBM Blockchain operator by using the OpenShift CLI. For example, this command would delete the operator:

  ```
  kubectl delete deployment ibm-hlfsupport-operator
  ```
  {: codeblock}

## Create a project for your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment
{: #deploy-ocp-rhm-fw-project}

Before you can deploy the operator, you need to create a project for your deployment of {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric. You can create a new project by using the OpenShift web console or OpenShift CLI. The new project needs to be created by a cluster administrator.

If you are using the CLI, create a new project by the following command:
```
oc new-project <PROJECT_NAME>
```
{: codeblock}

Replace `<PROJECT_NAME>` with the name that you want to use for your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment project.

It is required that you create a new OpenShift project for each blockchain network that you deploy with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric. For example, if you plan to create different networks for development, staging, and production, then you need to create a unique project for each environment. Each project creates a new Kubernetes namespace. Using a separate namespace provides each network with separate resources and allows you to set unique access policies for each network. You need to follow these deployment instructions to deploy a separate operator and console for each project.
{: important}

When you create a new project, a new namespace is created with the same name as your project. You can verify that the existence of the new namespace by using the `oc get namespace` command:
```
$ oc get namespace
NAME                                STATUS    AGE
blockchain-project                  Active    2m
```

You can also use the CLI to find the available storage classes for your namespace. If you created a new storage class for your deployment, that storage class must be visible in the output in the following command:
```
kubectl get storageclasses
```
{: codeblock}
