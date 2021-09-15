---

 copyright:
  years: 2021
lastupdated: "2021-09-15"

keywords: ansible playbooks, docker image, blockchain network, APIs, ansible galaxy

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


# Deploy the service from an Ansible playbook
{: #ansible-install-ibp}

Customers can use an Ansible playbook to automate their installation of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network deployments on a Kubernetes distribution.
{: shortdesc}

**Target audience:** This topic is designed for system administrators or network creators who are responsible for installing {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric networks on a Kubernetes cluster and are new to Ansible playbooks.

This playbook can be used to deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric on any [supported Kubernetes distribution](/docs/hlf-support?topic=hlf-support-console-ocp-about#console-ocp-about-prerequisites). It is not available for Kubernetes clusters that are running in {{site.data.keyword.cloud_notm}}.
{: important}

In just a few minutes, you can have an instance of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 up and running on your cluster.

## Prerequisites
{: #ansible-install-ibm-hlfsupport-prereqs}

Before using the playbook, you must have completed the following steps:
- Purchase an entitlement to {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0. When you configure the playbook, you will need to provide your entitlement key and the email address of your IBMid account that you use to access the My IBM dashboard.
- Review the topic on [getting started with using {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric with Ansible](/docs/hlf-support?topic=hlf-support-ansible#ansible-getting-started).
- [Install the prerequisites](https://ibm-blockchain.github.io/ansible-collection/installation.html#requirements) on the system where you will run Ansible.
- If you are running on Azure Kubernetes Service, Amazon Web Services, Rancher, Amazon Elastic Kubernetes Service, or Google Kubernetes Engine then you need to set up the NGINX Ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}.

### Gather your Kubernetes cluster details
{: #ansible-install-ibm-hlfsupport-k8s-cluster}

Before you can deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric to your Kubernetes cluster, you need to gather the following information:

- The name of your Kubernetes `namespace`, or `project`, if you are using OpenShift Container Platform.
- The domain name of your Kubernetes cluster. This domain name is used as the base domain name for all ingress or routes that are created by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.
  - If you are running OpenShift Container Platform, you can find this value by using the OpenShift web console. After logging in to the console, use the drop-down menu next to **OpenShift Container Platform** at the upper left of the page to switch from Service Catalog to Cluster Console. Examine the URL for that page. It will be similar to `console.xyz.abc.com/k8s/cluster/projects`. The value of the domain then would be `xyz.abc.com`, after you remove `console` and `/k8s/cluster/projects`.
  - If you are running any other supported Kubernetes distribution, follow instructions from the distribution documentation to retrieve the domain name after configuring the NGINX Ingress controller.

## Build the Ansible playbook
{: #ansible-install-ibm-hlfsupport-playbook}

You are now ready to complete instructions in the [Ansible Collection](https://ibm-blockchain.github.io/ansible-collection/tutorials/installing.html#installing-the-ibm-blockchain-platform){: external} to install the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.

When the installation completes successfully you see the message:
```
TASK [console : Print console URL] *************************************************************************************************************************************
ok: [localhost] => {
    "msg": "{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console available at https://xm0507-ibm-hlfsupport-console-console.ibp20openshifttestcluster-0defdaa0c51bd4a2dcb024eab4bf04a1-0001.us-south.containers.appdomain.cloud"
}
```

Congratulations! You have successfully installed the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric on your cluster. Record your console URL from the playbook output and verify the deployment by logging in to the console. Specify the `<console_email>` and `<console_default_password>` that you provided in the `install-ibp.yml` file.

The first time you log in, you are required to change your password, and then log in again.
{: note}

## Next steps
{: #ansible-install-ibm-hlfsupport-playbook-next}

If you are simply interested in automating the process of installing the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you are done. You can use the console UI or APIs to create your blockchain components. If you are not familiar with the process to build a network by using the console, check out the [tutorial](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network) that walks you through the steps of building a sample network.

Already familiar with how to build a network and want to continue to learn more about the available Ansible playbooks? Then check out the tutorial on [Building an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network using Ansible playbooks](/docs/hlf-support?topic=hlf-support-ansible-build).


