---

copyright:
  years: 2021
lastupdated: "2021-09-10"

keywords: IBM Support for Hyperledger Fabric, images, multicloud

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



# Using the {{site.data.keyword.blockchainfull_notm}} images
{: #blockchain-images}


For experienced Hyperledger Fabric customers, the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric provides images for peer, CA, ordering service, and smart contract containers that are signed and supported by {{site.data.keyword.IBM_notm}}. These images are the commercial distribution of Hyperledger Fabric v2.x.
{: shortdesc}

A key benefit of using these images over the open source community version is that {{site.data.keyword.IBM_notm}} scans the open source code for security vulnerabilities daily as well as keeps the images up to date with operating system and vulnerability patches. Additionally, {{site.data.keyword.IBM_notm}} provides 24x7x365 support with SLAs appropriate for production environments.  The images are based on [Red Hat Universal Base Image (UBI)](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image){: external} and the blockchain components are enabled for HSM support.   

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images can be purchased through an entitlement to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0. The images are bundled with support from {{site.data.keyword.IBM_notm}}. While it is possible to configure mixed networks that include components (CAs, peers, and ordering nodes) from the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric and Hyperledger Fabric community version, {{site.data.keyword.IBM_notm}} support is limited to {{site.data.keyword.blockchainfull_notm}} components.  {{site.data.keyword.IBM_notm}} does not support blockchain components that are deployed using the open source Hyperledger Fabric Docker images.

## Supported Platforms
{: #blockchain-images-supported-platforms}

The {{site.data.keyword.blockchainfull_notm}} images must be deployed using a container environment on x86_64 or s390x hardware. Refer to the list of [Supported Platforms](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-prerequisites).

Although you can deploy the {{site.data.keyword.blockchainfull_notm}} images on Mac OS for testing purposes, the permissions on Mac OS might prevent you from instantiating a smart contract.
{: note}

## Supported Fabric versions
{: #blockchain-images-supported-fabric}

The {{site.data.keyword.blockchainfull_notm}} Docker images are based on Hyperledger Fabric v2.2.3. You can use this documentation to install and deploy the latest version of Fabric used by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0.

See the [My Support](https://www.ibm.com/support/pages/node/1072956){: external} page for details on what is supported.

## Considerations and limitations
{: #blockchain-images-considerations}

The images do not include the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console or operator. This offering is meant for experienced Fabric users with existing deployments. If you are still exploring Hyperledger Fabric, you can get started with [{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibm-hlfsupport-v2-deploy-iks#ibm-hlfsupport-v2-deploy-iks).
{:important}

- {{site.data.keyword.blockchainfull_notm}} provides support for Hyperledger Fabric only if you purchase {{site.data.keyword.blockchainfull_notm}} 1.0.0 and deploy the commercial distribution of Hyperledger Fabric images that comes with it. You cannot purchase support for the Hyperledger Fabric Docker images that are provided by the Hyperledger community.
- {{site.data.keyword.blockchainfull_notm}} does not support images that have been altered.
- You cannot use the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console to deploy or operate these images. But, if you download the image for the gRPC web proxy and connect the proxy to node that you deploy, you can import the node into an existing {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console. After you import a node into the console, you can operate that node alongside nodes that were deployed by using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.

## License and pricing
{: #blockchain-images-license}

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images can be purchased through an entitlement to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0. After you purchase the platform, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key. You can then use the entitlement key to download the images from the {{site.data.keyword.IBM_notm}} Entitlement Registry.

For more information on the pricing of the images, to the [Pricing](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-sw-pricing) topic.

## Get your entitlement key
{: #blockchain-images-entitlement-key}

When you purchase the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric from PPA, you receive an entitlement key for the software is associated with your MyIBM account. You need to access and save this key to deploy the platform.

1. Log in to [MyIBM Container Software Library](https://myibm.ibm.com/products-services/containerlibrary){: external} with the IBMid and password that are associated with the entitled software.

2. In the Entitlement keys section, select **Copy key** to copy the entitlement key to the clipboard.

## Downloading the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images
{: #blockchain-images-downloading}

The platform recommends that you use the `skopeo` utility to download and copy your images to your local container registry. [skopeo](https://www.redhat.com/en/blog/skopeo-10-released){: external} is a tool for moving container images between different types of container storages. In order to download the platform images and copy them to your container registry behind a firewall, you first need to [install skopeo](https://github.com/containers/skopeo/blob/master/install.md){: external}.

Before attempting these instructions, you need to have your IBM Blockchain Platform entitlement key and container registry user id and password available. After you purchase the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. You can then use this key to access the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images.

Run the following set of commands to download the images and push them to your registry.  

Replace
- `<ENTITLEMENT_KEY>` with your entitlement key.
- `<LOCAL_REGISTRY_USER>` with the userid with access to your container registry.
- `<LOCAL_REGISTRY_PASSWORD>` with the password to your container registry.
- `amd64` - with `s390x` if you are installing on LinuxONE.

The following commands only work with a Docker container registry. Depending on the level of permissions required for the target location for the images, you might need to prefix each command with `sudo`.
{: note}

```
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-init:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-init:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-console:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-console:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-grpcweb:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-grpcweb:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-deployer:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-deployer:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-fluentd:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-fluentd:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:3.1.1-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:3.1.1-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ca:1.5.1-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-ca:1.5.1-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-dind:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-dind:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-chaincode-launcher:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-chaincode-launcher:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-enroller:1.0.0-20210915 docker://cp.icr.io/cp/ibm-hlfsupport-enroller:1.0.0-20210915 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
```
{: codeblock}

## Getting started
{: #getting-started}

To deploy and operate the {{site.data.keyword.blockchainfull_notm}} images, you can download the open source tools that are provided by the Hyperledger community. Follow the steps to [Install the prerequisites](https://hyperledger-fabric.readthedocs.io/en/release-2.2/prereqs.html) and [Download the Fabric Samples, Binaries, and configuration files](https://hyperledger-fabric.readthedocs.io/en/release-2.2/install.html){: external} in the Hyperledger Fabric documentation. These steps might download the open source Fabric images as well.

After you download the Fabric samples and binaries, you can find the configuration files and binaries that you need to set up a network in the `fabric-samples\config` and `fabric-samples\bin` folders. You can also find example artifacts and scripts for how to set up a network by using Docker Compose in the `fabric-samples\first-network` directory. You learn more about these artifacts and the steps that are involved by reading the accompanying [Build Your First Network](http://hyperledger-fabric.readthedocs.io/en/release-2.2/build_network.html){: external} tutorial.

If you are using the open source configuration files, you need to make the following changes to deploy the {{site.data.keyword.blockchainfull_notm}} images:

1. For each component, you need to alter the `image` field to use the {{site.data.keyword.blockchainfull_notm}} image instead of the open source image.

2. You need add the `LICENSE` field to accept the {{site.data.keyword.IBM_notm}} license:
  ```
  LICENSE=accept
  ```

3. If you are deploying a peer node, you need to use the core chaincode variables to instruct the peer to build chaincode using the {{site.data.keyword.blockchainfull_notm}} certified images. For example, if you are using Go chaincode, you need to set the following variables:
  ```
  CORE_CHAINCODE_NODE_RUNTIME=cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20210915-amd64
  CORE_CHAINCODE_GOLANG_DYNAMICLINK=true
  ```

4. You need to mount the configuration files, `core.yaml` or `orderer.yaml`, inside the node container. You also need to set the `FABRIC_CFG_PATH` environment variable to the path where you mounted the configuration files.

  If you are deploying a peer node, you need to comment out or remove the `PKCS11` section of the file unless you are using an HSM. The section of `core.yaml` that needs to be removed is below:
  ```
   Settings for the PKCS#11 crypto provider (i.e. when DEFAULT: PKCS11)
   PKCS11:
       # Location of the PKCS11 module library
       Library:
       # Token Label
      Label:
      # User PIN
      Pin:
      Hash:
      Security:
      FileKeyStore:
          KeyStore:
  ```

5. For security reasons, the {{site.data.keyword.blockchainfull_notm}} images require a different level of permission to access data than the open source images. You need to change the access of the folder that you mount to store your organization MSP and TLS certificates and the folder that is mounted to store your ledger data. You can use the following command to change access to the folders before they are mounted inside the container:
  ```
  chmod -R 777 <folder_name>
  ```

In addition to using the Fabric tools, you can also use the tools that are provided in the `ibm-hlfsupport-utilities` image to operate your network. The utilities image includes the configtxlator, cryptogen, configtxgen binaries.

### Configuring the gRPC web proxy (optional)
{: #getting-started-proxy}

If you want to manage your nodes using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console, you can deploy an instance of the gRPC web proxy and then connect it to a node that you deployed with the {{site.data.keyword.blockchainfull_notm}} images. You can then import the node into a console that was deployed by using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 or {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric for {{site.data.keyword.cloud_notm}}. You need to deploy a separate web proxy for each node.

To deploy the proxy, you need to set the following environment variables inside the container.
```
LICENSE=accept
BACKEND_ADDRESS=<NODE_ENDPOINT_URL>
EXTERNAL_ADDRESS=<PROXY_ENDPOINT_URL>
SERVER_TLS_CERT_FILE=<TLS_CERTIFICATE>
SERVER_TLS_KEY_FILE=<TLS_KEY>
SERVER_TLS_CLIENT_CA_FILES=<ROOT_TLS_CERTIFICATE>
SERVER_BIND_ADDRESS=0.0.0.0
SERVER_HTTP_DEBUG_PORT=8080
SERVER_HTTP_TLS_PORT=7443
BACKEND_TLS=true
SERVER_HTTP_MAX_WRITE_TIMEOUT=5m
SERVER_HTTP_MAX_READ_TIMEOUT=5m
USE_WEBSOCKETS=true
```
- Use the `BACKEND_ADDRESS` variable to point to the endpoint URL of your node.
- Use the `EXTERNAL_ADDRESS` to select the web proxy URL that you will use to access the node from your console.
- Use the `SERVER_HTTP_TLS_PORT` to select the port that you will use the access the web proxy.
- You also need to provide a TLS certificate and key that will be used secure the communication between your console and web proxy. It is recommended that you use certificates that were issued to secure the domain name of your cluster. The common name of the TLS certificates must be the domain name of your web proxy URL. The TLS certificates then need be mounted inside the web proxy container. Use `SERVER_TLS_CERT_FILE` to point to the certificate and `SERVER_TLS_KEY_FILE` to point to the accompanying private key.

The console needs to check the health of any node being imported into the console. As a result, you need to enable the operations service for every node that is connected to a web proxy. For more information, see [The Operations Service](https://hyperledger-fabric.readthedocs.io/en/latest/operations_service.html).

After the web proxy container is deployed, you can import the node into a console by creating a node JSON file. For more information about how to create the node file, see [Importing nodes from a locally deployed network](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-import-nodes#ibm-hlfsupport-console-import-icp). You need to use the following values when you create the file:
- Use the web proxy URL and port for the `"grpcwp_url"` field. You specified these values with the `EXTERNAL_ADDRESS` and `SERVER_HTTP_TLS_PORT` environment variables.
- Use the node endpoint URL and port for the `"api_url"` field. You specified these values with the `BACKEND_ADDRESS` environment variable.
- Encode the public TLS certificate of the node in base64 format and paste it in the`"pem"` field. This certificate was referenced by the `SERVER_TLS_CLIENT_CA_FILES` environment variable.

We deploy an [example web proxy](#example-proxy) using Docker Compose as part of the example provided below. While the example is not a template for deploying a web proxy in production, it can be used for additional context on how you would connect a proxy to a running node.

## Example
{: #blockchain-images-example}

We can provide an example of the changes you need to make by updating the `first-network` sample to run the {{site.data.keyword.blockchainfull_notm}} images instead of the community Docker images. The example is provided for context, and is not a template for deploying a production network.

You need to update the `peer-base.yaml` and `docker-compose-base.yaml` that are in the `fabric-samples/first-network/base` folder. You can find the orderer section of the `peer-base.yaml` below:

```yaml
orderer-base:
  image: hyperledger/fabric-orderer:$IMAGE_TAG
  environment:
    - FABRIC_LOGGING_SPEC=INFO
    - ORDERER_GENERAL_LISTENADDRESS=0.0.0.0
    - ORDERER_GENERAL_GENESISMETHOD=file
    - ORDERER_GENERAL_GENESISFILE=/var/hyperledger/orderer/orderer.genesis.block
    - ORDERER_GENERAL_LOCALMSPID=OrdererMSP
    - ORDERER_GENERAL_LOCALMSPDIR=/var/hyperledger/orderer/msp
    # enabled TLS
    - ORDERER_GENERAL_TLS_ENABLED=true
    - ORDERER_GENERAL_TLS_PRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_TLS_CERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_TLS_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
    - ORDERER_KAFKA_TOPIC_REPLICATIONFACTOR=1
    - ORDERER_KAFKA_VERBOSE=true
    - ORDERER_GENERAL_CLUSTER_CLIENTCERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_CLUSTER_CLIENTPRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_CLUSTER_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
  working_dir: /opt/gopath/src/github.com/hyperledger/fabric
  command: orderer
```
{: codeblock}

To deploy an ordering node by using the {{site.data.keyword.blockchainfull_notm}} image, change the `image` field to the tag of the {{site.data.keyword.blockchainfull_notm}} image, `cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20210915-amd64`. Accept the license by adding the field of `LICENSE=accept`. You then need to add the `FABRIC_CFG_PATH` environment variable and set the path to the folder where you mount the configuration files. Set the `working_dir` variable to the same path. After your changes, the orderer section would look like the example below:

```yaml
orderer-base:
  image: cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20210915-amd64
  environment:
    - LICENSE=accept
    - FABRIC_CFG_PATH=/etc/hyperledger/fabric
    - FABRIC_LOGGING_SPEC=INFO
    - ORDERER_GENERAL_LISTENADDRESS=0.0.0.0
    - ORDERER_GENERAL_GENESISMETHOD=file
    - ORDERER_GENERAL_GENESISFILE=/var/hyperledger/orderer/orderer.genesis.block
    - ORDERER_GENERAL_LOCALMSPID=OrdererMSP
    - ORDERER_GENERAL_LOCALMSPDIR=/var/hyperledger/orderer/msp
    # enabled TLS
    - ORDERER_GENERAL_TLS_ENABLED=true
    - ORDERER_GENERAL_TLS_PRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_TLS_CERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_TLS_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
    - ORDERER_KAFKA_TOPIC_REPLICATIONFACTOR=1
    - ORDERER_KAFKA_VERBOSE=true
    - ORDERER_GENERAL_CLUSTER_CLIENTCERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_CLUSTER_CLIENTPRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_CLUSTER_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
  working_dir: /etc/hyperledger/fabric
  command: orderer
```
{: codeblock}

If you are deploying a peer, you can make the same changes to the peer section of the `peer-base.yaml` file. However, you need add the core chaincode variables and specify images that you downloaded from {{site.data.keyword.IBM_notm}} as the chaincode builder and the runtime environment. You also need to set `DYNAMICLINK=true` After your changes, the peer section would look like the example below:

```yaml
services:
  peer-base:
    image: cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20210915-amd64
    environment:
      - LICENSE=accept
      - FABRIC_CFG_PATH=/etc/hyperledger/fabric
      - CORE_CHAINCODE_BUILDER=cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20210915-amd64
      - CORE_CHAINCODE_GOLANG_RUNTIME=cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20210915-amd64
      - CORE_CHAINCODE_NODE_RUNTIME=cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20210915-amd64
      - CORE_CHAINCODE_GOLANG_DYNAMICLINK=true
      - CORE_CHAINCODE_NODE_DYNAMICLINK=true
      - CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
      # the following setting starts chaincode containers on the same
      # bridge network as the peers
      # https://docs.docker.com/compose/networking/
      - CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=${COMPOSE_PROJECT_NAME}_byfn
      - FABRIC_LOGGING_SPEC=INFO
      #- FABRIC_LOGGING_SPEC=DEBUG
      - CORE_PEER_TLS_ENABLED=true
      - CORE_PEER_GOSSIP_USELEADERELECTION=true
      - CORE_PEER_GOSSIP_ORGLEADER=false
      - CORE_PEER_PROFILE_ENABLED=true
      - CORE_PEER_TLS_CERT_FILE=/etc/hyperledger/fabric/tls/server.crt
      - CORE_PEER_TLS_KEY_FILE=/etc/hyperledger/fabric/tls/server.key
      - CORE_PEER_TLS_ROOTCERT_FILE=/etc/hyperledger/fabric/tls/ca.crt
    working_dir: /etc/hyperledger/fabric
    command: peer node start
```
{: codeblock}

The files that are mounted into the peer and orderer containers are specified in the `docker-compose-base.yaml` file. In the `volumes:` section, you can add the following line ``../../config:/etc/hyperledger/fabric`` to mount the peer and orderer configuration files that are located in the `fabric-samples` directory. You need to remove the `PKCS11` section of the `fabric-samples/config/core.yaml` file. The ledger data folder is mounted in the `/var/hyperledger/production`. We need to change this path to an existing folder to change the permissions of that folder. After your changes, `docker-compose-base.yaml` would look like the example below for an ordering node and one of your peers:

```yaml
orderer.example.com:
  container_name: orderer.example.com
  extends:
    file: peer-base.yaml
    service: orderer-base
  volumes:
      - ../channel-artifacts/genesis.block:/var/hyperledger/orderer/orderer.genesis.block
      - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/msp:/var/hyperledger/orderer/msp
      - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tls/:/var/hyperledger/orderer/tls
      - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com:/var/hyperledger/production/orderer
      - ../../config:/etc/hyperledger/fabric
  ports:
- 7050:7050

peer0.org1.example.com:
  container_name: peer0.org1.example.com
  extends:
    file: peer-base.yaml
    service: peer-base
  environment:
    - CORE_PEER_ID=peer0.org1.example.com
    - CORE_PEER_ADDRESS=peer0.org1.example.com:7051
    - CORE_PEER_LISTENADDRESS=0.0.0.0:7051
    - CORE_PEER_CHAINCODEADDRESS=peer0.org1.example.com:7052
    - CORE_PEER_CHAINCODELISTENADDRESS=0.0.0.0:7052
    - CORE_PEER_GOSSIP_BOOTSTRAP=peer1.org1.example.com:8051
    - CORE_PEER_GOSSIP_EXTERNALENDPOINT=peer0.org1.example.com:7051
    - CORE_PEER_LOCALMSPID=Org1MSP
  volumes:
      - /var/run/:/host/var/run/
      - ../crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/msp:/etc/hyperledger/fabric/msp
      - ../crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls:/etc/hyperledger/fabric/tls
      - ../crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com:/var/hyperledger/production
      - ../../config:/etc/hyperledger/fabric
  ports:
    - 7051:7051
```
{: codeblock}

After you edit the file, you need to change the security permissions of the folders that are mounted to store your certificates and ledger data. Your peer MSP folder and TLS certificates are both stored in the `crypto-config` folder that is created when you run `./byfn.sh generate`. To simplify the deployment, the example also mounts a folder in this directory to store our ledger data. You can run the following commands to generate the crypto material and folders, and then provide those folders the required permissions:
```
./byfn.sh generate
chmod -R 777 crypto-config
```
{: codeblock}

If you want to deploy a raft ordering service, you need to make the same edits to the `docker-compose-etcdraft2.yaml` file that you made to the order section of `docker-compose-base.yaml`. You need to create ledger folders for each ordering node and change the permissions of each folder.

After you update the relevant files, you can test that you can deploy your images by running the following command:
```
sudo ./byfn.sh up -i 1.4.12
```
{: codeblock}

You can use the updates that are provided in this example to deploy the {{site.data.keyword.blockchainfull_notm}} images in the environment of your choice. The Fabric samples that are published by the Hyperledger community are intended to be used only as examples. Do not use the samples as templates for deploying production networks.

### Deploying an {{site.data.keyword.blockchainfull_notm}} Certificate Authority
{: #blockchain-images-example-ca}

The `first-network` sample does not use Certificate Authorities to deploy the network. However, you can find a file below that you can use to deploy an {{site.data.keyword.blockchainfull_notm}} Certificate Authority using Docker Compose. If you want to deploy a CA as part of the example, you should use this file instead of editing the `docker-compose-ca.yaml` file in the `first-network` directory, which deploys a CA using existing crypto material. Save the following file as ``docker-compose-ibm-ca.yaml``:

```yaml
version: '2'

networks:
  byfn:

services:

  ca-org1:
    image: cp.icr.io/cp/ibm-hlfsupport-ca:2.2.3-20210915-amd64
    environment:
      - LICENSE=accept
      - FABRIC_CA_HOME=/etc/hyperledger/fabric-ca-server
      - FABRIC_CA_SERVER_CA_NAME=ca-org1
      - FABRIC_CA_SERVER_TLS_ENABLED=true
      - FABRIC_CA_SERVER_PORT=7054
    ports:
      - "7054:7054"
    command: sh -c 'fabric-ca-server start -b admin:adminpw -d'
    volumes:
      - fabric-ca-config:/etc/hyperledger/fabric-ca-server
    container_name: ca_org1
```
{: codeblock}

You need to mount a copy of the Fabric CA server configuration file in the CA container. Create a folder named `fabric-ca-config` and save a copy of the [Fabric CA server configuration file](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/serverconfig.html) inside as `fabric-ca-server-config.yaml`. Update the sample file with information about your organization and the domain name of your CA. The Docker compose file will use this file to start the Fabric CA server and set the username and password of your CA admin to `admin` and `adminpw`. You can find more information about the Fabric CA server in the [Fabric CA users guide](https://hyperledger-fabric-ca.readthedocs.io/en/latest/users-guide.html). After you have saved `docker-compose-ibm-ca.yaml` and the configuration file, you can deploy the CA using the following command:
```
docker-compose -f docker-compose-ibm-ca.yaml up
```
{: codeblock}

When the CA is deployed, you can use the Fabric CA client or the Fabric SDKs to register new identities and generate certificates. For more information about how you would set up a blockchain network by using Certificate Authorities, see the [Fabric CA Operations guide](https://hyperledger-fabric-ca.readthedocs.io/en/latest/operations_guide.html).

### Deploying the gRPC web proxy
{: #example-proxy}

After we deploy a network, we can use the gRPC web proxy to import a peer or ordering node into an instance of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console. Assuming that you downloaded the gRPC web proxy image, you can use the instructions below to connect a web proxy `peer0.org1.example.com`.

1. You need to enable the operations service for the peer. This will allow the console to check if the peer or ordering node is healthy before it connects to the node. To enable the operations service for `peer0.org1.example.com`, add the following environment variables to the to the `peer0.org1.example.com` section of `docker-composer-base.yaml`:
  ```
  - CORE_OPERATIONS_LISTENADDRESS=0.0.0.0:9443
  - CORE_OPERATIONS_TLS_ENABLED=false
  ```
  {: codeblock}

  You can also need to add the `9443` to the list of ports exposed by the peer.
  ```yaml
  ports:
  - 7051:7051
  - 9443:9443
  ```
  {: codeblock}

2. You need TLS certificates and keys to secure communication between your the console and your node. Create a folder that you will use for the TLS material and change into that directory.
  ```
  mkdir peer0.org1-proxy-certs
  cd peer0.org1-proxy-certs
  ```
  {: codeblock}

  This folder will be mounted in the gRPC web proxy container in a later step.

3. In a production environment, you would use the TLS certificates that secure the domain name of your deployment environment. For this example, we will generate self signed certificates using the OpenSSL tool.

  - Use the following command to generate a private key:

    ```
    openssl genrsa -out private.key 4096
    ```
    {: codeblock}

  - Create a `ssl.conf` file using the template below. Replace the `_default` variables with the values of your choice. Use the `alt_names` section to provide the domain of your cluster. This domain needs to match the public URL that you will select for the web proxy.

    ```
    extensions = extend

    [extend] # openssl extensions
    subjectKeyIdentifier = hash
    authorityKeyIdentifier = keyid:always
    keyUsage = digitalSignature,keyEncipherment #Required by macOS Catalina
    extendedKeyUsage=serverAuth,clientAuth

    [ req ]
    default_bits       = 4096
    distinguished_name = req_distinguished_name
    req_extensions     = req_ext

    [ req_distinguished_name ]
    countryName                 = Country Name (2 letter code)
    countryName_default         = US
    stateOrProvinceName         = State or Province Name (full name)
    stateOrProvinceName_default = North Carolina
    localityName                = Locality Name (eg, city)
    localityName_default        = Durham
    organizationName            = Organization Name (eg, company)
    organizationName_default    = DevCorp
    commonName                  = Common Name (e.g. server FQDN or YOUR name)
    commonName_max              = 64
    commonName_default          = John Doe (development)

    [ req_ext ]
    subjectAltName = @alt_names

    [alt_names]
    DNS.1   = web_proxy.peer0.org1.example.com
    DNS.2   = *.example.com
    ```
    {: codeblock}

  - Use the `ssl.conf` file and your private key to create a certificate signing request. Click enter to accept all the values you provided using the `_default` variables.

    ```
    openssl req -new -sha256 -out private.csr -key private.key -config ssl.conf
    ```
    {: codeblock}

  - Create the certificate:

    ```
    openssl x509 -req -sha256 -days 3650 -in private.csr -signkey private.key -out public.crt -extensions req_ext -extfile ssl.conf
    ```
    {: codeblock}

4. In addition to the TLS certificates used by the proxy, the web proxy also needs the public TLS certificate used by the peer or ordering node. Use the following command to copy the `peer0.org1.example.com` TLS certificate into your the current folder.
  ```
  cp ../crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt ca.crt
  ```
  {: codeblock}

  After copying the TLS certificate, the `peer0.org1-proxy-certs` should contain the peer TLS certificate and the web proxy TLS certificate and key:
  ```
  $ ls
  ca.crt		private.csr	private.key	public.crt	ssl.conf
  ```

5. Change back into the main directory of the BYFN example and save the following Docker Compose file for the gRPC web proxy image as `docker-compose-proxy`:

  ```yaml
  version: '2'

  networks:
    byfn:

  services:
    web_proxy.peer0.org1.example.com:
      container_name: web_proxy.peer0.org1.example.com
      image: cp.icr.io/cp/ibm-hlfsupport-grpcweb:1.0.0-20210915-amd64
      environment:
        - LICENSE=accept
        - BACKEND_ADDRESS=peer0.org1.example.com:7051
        - EXTERNAL_ADDRESS=web_proxy.peer0.org1.example.com:8443
        - SERVER_TLS_CERT_FILE=/certs/tls/public.crt
        - SERVER_TLS_KEY_FILE=/certs/tls/private.key
        - SERVER_TLS_CLIENT_CA_FILES=/certs/tls/ca.crt
        - SERVER_BIND_ADDRESS=0.0.0.0
        - SERVER_HTTP_DEBUG_PORT=8080
        - SERVER_HTTP_TLS_PORT=8443
        - BACKEND_TLS=true
        - SERVER_HTTP_MAX_WRITE_TIMEOUT=5m
        - SERVER_HTTP_MAX_READ_TIMEOUT=5m
        - USE_WEBSOCKETS=true
      ports:
        - "8443:8443"
      volumes:
        - ./peer0.org1-proxy-certs:/certs/tls
      networks:
        - byfn
  ```
  {: codeblock}

  The Docker Compose file mounts the folder where we stored the TLS certificates in the web proxy container inside a folder named `certs/tls`. We need to specify several important environment variables that point to the certificates inside this folder:
  - `SERVER_TLS_CERT_FILE` needs to point to the certificate you created using OpenSSL.
  - `SERVER_TLS_KEY_FILE` needs to point to the private key that you created using OpenSSL.
  - `SERVER_TLS_CLIENT_CA_FILES` needs to point to the node TLS certificate.

  You also need to specify the public URL of the peer and the web proxy:
  - `BACKEND_ADDRESS` needs to point to the public endpoint of your peer that is used by client applications, `peer0.org1.example.com:7051`.
  - Use the `EXTERNAL_ADDRESS` variable select the URL and port that will be used to access the web proxy. This URL and port needs to be accessible by external traffic.

6. Before we bring up the proxy, we need to create a JSON file used to import the peer into our console. Save the following file as `peer0.org1.json`

  ```JSON
  {
    "name": "peer.org1.exaple.com",
    "grpcwp_url": "https://web_proxy.peer0.org1.example.com:8443",
    "api_url": "grpcs://peer0.org1.example.com:7051",
    "operations_url": "https://peer0.org1.example.com:9443",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K",
    "location": "mycluster"
  }
  ```
  {: codeblock}

  Use the following values to complete the file:
  - `"name"`: The display name for the peer in the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console.
  - `"grpcwp_url"`: The external address of the web proxy that you selected using the `EXTERNAL_ADDRESS` variable.
  - `"api_url"`: The public address of the node used by your client applications and is specified in the `BACKEND_ADDRESS` variable.
  - `"operations_url"`: The URL and port that was opened for the operations service. We used the `CORE_OPERATIONS_LISTENADDRESS` variable to specify that the operations service will use port `9443` in step one. As a result, the console can use the address `https://peer0.org1.example.com:9443` to check the health of our peer.
  - `"type"`: select `fabric-peer`, `fabric-orderer`, or `fabric-ca`.
  - `"msp_id"`: The MSPID of the organization that operates the node.
  - `"location"`: Location of your choice.
  - ``"pem"`` The node TLS certificate encoded in Base64 format. We copied the peer TLS certificate into the `peer0.org1-proxy-cert` folder. We can encode the certificate using the following command:

    ```
    export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
    cat peer0.org1-proxy-certs/ca.crt | base64 $FLAG
    ```
    {: codeblock}

7. Bring up the web proxy using the following command:
  ```
  docker-compose -f docker-compose-proxy.yaml up
  ```
  {: codeblock}

  If the web proxy is created successfully, you should see messages in your logs similar to the following:
  ```
  web_proxy.peer0.org1.example.com    | time="2020-01-29T22:18:37Z" level=info msg="listening for http_tls on: [::]:8443"
  web_proxy.peer0.org1.example.com    | time="2020-01-29T22:18:37Z" level=info msg="listening for http on: [::]:8080"
  ```

You can now view the node from a console by importing the ``peer0.org1.json`` file into an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console deployed on {{site.data.keyword.cloud_notm}} or your own cluster using {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0. In order for the console to access your nodes, the URL of the web proxy, the peer, and the operations URL needs be externally accessible.

You need to complete these steps for each node that you want to import into an instance of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console.


## Interoperability
{: #blockchain-images-interop}

Nodes that are deployed by using the {{site.data.keyword.blockchainfull_notm}} images can join the channels of other {{site.data.keyword.blockchainfull_notm}} offerings, such as {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 and {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric for {{site.data.keyword.cloud_notm}}. Unless you connected an instance of the gRPC web proxy to the node, you cannot use the console to operate the images. You need to use Fabric tools to join existing channels that were created with a console or create new channels.

## Upgrading to new versions
{: #blockchain-images-upgrade}

You can upgrade your deployment to the latest version of the {{site.data.keyword.blockchainfull_notm}} images. The latest images normally contain security or stability improvements, or use a higher version Hyperledger Fabric, allowing you to take advantage of the latest Fabric features. To upgrade your network, you need to back up the ledger and MSP data for each node and then manually upgrade the node binaries. For more information about upgrading a network that you deployed using the {{site.data.keyword.blockchainfull_notm}} images, see [Upgrading Your Network Components](https://hyperledger-fabric.readthedocs.io/en/release-2.2/upgrade.html){: external} in the Hyperledger Fabric documentation.

## Getting support
{: #blockchain-images-support}

If you encounter issues that are related to Hyperledger Fabric or the underlying images, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/node/1072956){: external} page. When you open a case you need to select your product, {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0.

{{site.data.keyword.IBM_notm}} provides support for issues that are related to the Hyperledger Fabric code or the steps to download and deploy the images that you can find in this documentation. {{site.data.keyword.IBM_notm}} does not provide support for issues that relate to the operation of the images or your underlying infrastructure. Deployment issues due to the specific circumstances of the customer environment will be the customer's responsibility to investigate. If you need assistance with the deployment and management of a Fabric network, use the [{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about) offering.

You can take advantage of free blockchain developer resources and support forums to troubleshoot problems and get help from {{site.data.keyword.IBM_notm}} and the Fabric community. For more information, see [Resources and support forums](/docs/hlf-support?topic=hlf-support-blockchain-support#blockchain-support-resources).
