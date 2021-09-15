---

copyright:
  years: 2021
lastupdated: "2021-09-10"

keywords: create identities, manage identities, Certificate Authorities, register, enroll, TLS CA, wallet, certificate expiration, delete user

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




# Creating and managing identities
{: #ibm-hlfsupport-console-identities}


The nodes of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric are based on Hyperledger Fabric and builds permissioned blockchain networks. This means that all participants of the blockchain consortium need to have identities that are continuously verified by Public Key Infrastructure. The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console allows you to create these identities by using your organization's Certificate Authorities (CAs). You need to store these identities in your console wallet so you can use them to operate your network.
{: shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network.




## Managing Certificate Authorities (CAs)
{: #ibm-hlfsupport-console-identities-manage-ca}

A CA is similar to a publicly trusted notary that acts as an anchor of trust among multiple parties, with each organization in a consortium maintaining their own CA. Your CA creates the identities that belong to your organization and issue each identity a signing certificate and private key. These keys are what allow all of your nodes and applications to sign and verify their actions. For more information about how CAs are used to establish identity, see [the identity topic](https://hyperledger-fabric.readthedocs.io/en/release-2.2/identity/identity.html){: external} in the Hyperledger Fabric documentation.

When you create a CA by using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console, two CAs are created with the same endpoint URL: a root CA and a TLS CA. The root CA provides keys to your nodes and applications. The TLS CA provides certificates that are used to protect the communication within your network. To learn more about TLS on your network, see [Using your TLS CA](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-tlsca).

When you create your nodes, you need to use your root CA to create the following identities that are required to deploy, operate, and interact with your network:
- **CA admin:** Your CA contains a default CA admin identity. This identity has an enroll ID and secret, which are analogous to a username and password, that you specify during the deployment of your CA. You can use this admin username and password to operate your CA and create new identities. The CA admin of your root CA is also the admin of the TLS CA.
- **Peers or orderers:** You need to register an identity for each of the peers and orderers that belong to your organization. Your nodes will then use these identities during deployment to generate the keys they need to participate in the network. For security, create a unique enroll ID and secret for each node you deploy.
- **Org admins:** When you join a consortium that is hosted by an ordering service, you provide the signing certificates of identities that will become the administrators for your organization. You can use these identities to create or edit channels.
- **Peer or orderer admins:** {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric nodes are deployed with the signing certificates of component administrators identities inside of them. These certificates allow the admins to operate the component from a remote client or by using the console.
- **Applications:** Your applications need to sign their transactions before submitting them to be validated by the network. You need to create identities you can use to sign transactions from your client applications.

You can use the console to create these identities by using the [registration process](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-register). After you register your admin identities, you need to issue each identity a signing certificate and private key, provide the signing certificate to your organization MSP definition, and add the identity to your console wallet. You can complete these steps for one admin identity when you [create your organization MSP](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-organizations#console-organizations-create-msp). You can use separate identities as org admins or node admins, or you can use one identity to do both tasks. The [Build a network tutorial](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network) uses one identity to be an admin for each organization created in the tutorial.


## Associating the identity of the CA admin
{: #ibm-hlfsupport-console-identities-ca-identity}

Before you can create identities, you need to associate the identity of the CA admin. Open your CA on the **Nodes** tab. If you are using the CA for the first time, you can click the **Associate identity** button to generate the CA admin identity and import it into your console wallet. On the **Associate identity** side panel, provide the Enroll ID and Enroll secret of the CA admin that you provided when you created the CA.

You can view the enroll ID of the associated admin on the left side of the CA panel, below the CA name, Node location, and Fabric version. You can use this identity to create other identities by using the **Register** button. The CA admin also allows you to get the list of registered identities, and then use these identities to generate certificates by using the **Enroll** button.

To use a different identity as the CA admin, click the identity that is associated with the CA admin. This opens the **Associate identity** side panel. You can use the **Enroll an identity** tab to provide the enrollID and secret of another CA admin. You can specify an identity that exists in the wallet by using the **Select an existing identity** tab.

You can only use admin identities to register new users. You can create new admins with restrictions on the type of users they are allowed to create. As a result, you can let another user operate your CA by providing them an admin that is not able to create admins or deploy nodes. This allows other members or departments of your organization to access your CA without compromising your network. For more information, see [Creating new CA admins](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-ca-admin).
{: note}


## Registering identities
{: #ibm-hlfsupport-console-identities-register}

The first step in creating an identity is known as **registration**. During registration, an enroll ID and secret are created which can be used by a node or an org administrator to **enroll** this identity by generating a signing certificate and private key.  

You need to enter the following information when you register a new identity with your CA.

- **Enroll ID** and **Enroll Secret**: This identity has an ID and secret, which is analogous to a username and password, that you specify during the deployment of your CA. You can use this admin ID and secret to create certificates with this identity by using this console or the Fabric CA client.
- **Type**: When the CA was deployed, the administrator specified the types of identities issued by the CA. Common examples of identity types include peer, orderer, admin, and client (applications). Select the identity type for this user from the list of available types.
- **Affiliation**: (Optional) Advanced users only. This field is only visible if affiliations are defined for your CA. An affiliation is the part of an organization that you want to associate with this user. For example, this could be a department or unit that operates an application or a peer. It is possible to restrict a particular CA admin to be able to register only users within their own department within an organization by setting their affiliation. CA affiliations are defined by using the Fabric CA [Affiliation Command](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/clientcli.html#affiliation-command){: external}. The default affiliation that is available when you clear the **Use root affiliation** checkbox is `ibp`.
- **Maximum Enrollments**: Optionally, enter the number of times that you can enroll or generate certificates by using this identity. Specifying a limited number of enrollments helps protect the security of your nodes and applications. It defaults to unlimited enrollments.
- **Attributes**: Optionally, you can specify any [attribute-based access control](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#attribute-based-access-control){: external} attributes for the user. For example, you can use this section to [create another CA admin](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-ca-identity) with authority to register and enroll new identities. You can see a full list of available Fabric CA attributes in the [Registering a new identity](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity){: external} section of the Fabric CA users guide.

Click the **Register user** button on the CA overview panel to create a new user. Be sure that you have [set the identity](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-ca-identity)
to a CA admin that has the ability to register new users before you attempt this task. In general, this is your `admin` user. If the button is gray, you have either not set an identity, or that the identity cannot create new identities.  

Clicking **Register user** opens a series of side panels:
1. On the first side panel, enter the **Enroll ID** and **Enroll Secret** of the new identity. **Save these values**, as they are not stored by the console.
2. Select the identity **Type**. The drop-down list contains the list of types that the CA supports. If you are registering an identity that will serve as an admin of a node, select type `admin`. If you are registering a peer identity select `peer` and likewise for an ordering node identity select `orderer`. When you need to register an identity for a client application select the type `client`.
3. You can associate an affiliation with the user. Check the **Use root affiliation** checkbox for the user if you want them to have the root affiliation and be able to see all other users registered with this CA. When you deselect **Use root affiliation**, you can select a specific affiliation from the list to associate with this user. The platform includes the default affiliation `ibp`.
4. Enter the **Maximum Enrollments** allowed for this identity. If not specified, the value defaults to unlimited enrollments.
5. On the last side panel, add the **Attributes** of the identity you are creating.


After you click **Register**, the new identity will be added to the list of **Authenticated users** that have been created by your CA. The identities are listed by their **Enroll ID**, along with their **Type** and **Affiliation**. Clicking on an identity in the table opens a side panel that displays the number of **Maximum Enrollments** and **Attributes** that were created during registration.


**Deleting a user**  
If you need to delete a registered user, click the action menu next to any user and select
**Delete user**. If that option is not available, it can be enabled on your CA by overriding the CA configuration. See an example of how to enable this feature in [Modifying a CA configuration after deployment](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-ca-modify-json). Note this action does not revoke the associated certificates for the user. If you need to do that you would need to insert the associated signed certificate into the organization MSP under the `"revocation_list":` section. And then update that MSP definition everywhere that it occurs on the network.

### Creating new CA admins
{: #ibm-hlfsupport-console-identities-ca-admin}

By default, only the CA admin that is created during deployment has the ability to register new identities. You can create identities with the ability the register new users by using the **Attributes** panel of the registration process.

On the second side panel, click the **Add Attribute** button. Provide an **attribute name** of `hf.Registrar.Roles`. Enter an **attribute value** of `*`. You can also use this panel to create an identity that can register only certain identity types, such as clients or peers, or within a certain affiliation. You can also create an identity that has the ability to revoke an identity and all the certificates that the identity has been issued. You can see a full list of the attributes in the [Registering a new identity](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity){: external} section of the Fabric CA users guide.

## Enrolling an identity
{: #ibm-hlfsupport-console-identities-enroll}

You can generate the signing certificate and private key for each user that is registered with your CA. If you registered additional admin identities with your CA, you can generate the certificate and key for the admin identity and then include the certificate when you [create your organization MSP](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-organizations#console-organizations-create-msp).

Before you enroll an identity, you need to [Associate the identity of the CA admin](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-ca-identity) to be able to operate the CA. If the identity is not set, you will not be able to view the **Registered Users** table on the CA panel. In the Registered users table, navigate to the action menu ![action menu](../../icons/overflow-menu.svg) for the user and click **Enroll identity**  to generate the certificate and key for any user registered with the CA.

- Under the **Certificate Authorities** dropdown, select **Root Certificate Authority**.
- Enter the user's **Enroll secret**.
- (Optional) In the **CSR Hostname** field, enter the `Subject Alternative Names (SAN)` to embed in the generated certificate. Specify the host names or custom domain name where the certificate is valid. You can specify a comma separated list of hosts as well as include a wildcard in the domain. For example, `'host1, host2, *.example.com`. If you leave this field blank, the SAN inside the generated certificate is empty.
- On the next step, the generated keys are displayed.
  - The signing certificate is displayed in the **Certificate** field. This certificate is also referred to as your enrollment certificate, signing certificate, or signCert.
  - You can find the corresponding private key in the **Private Key** field. Again, you need to export the private key to your local system for use with a client application created with the VS Code extension.
  - The certificate and private key that are created by clicking **Enroll Identity** is only generated once and not stored by the console or your browser. Clicking the **Enroll Identity** button is counted against the maximum number of enrollments that you set for the identity. You should store the signing certificate and private key by downloading the identity to your local file system or adding it to your console wallet. Enter a new name for this signing certificate and private key into the **Name** field in order to retrieve them.
- **Important:** Click **Export identity** to download the certificate and key to your local file system as a single file in JSON format. You are responsible for securing and managing these keys.
- Click **Add identity to wallet** to add these certificates to the console wallet. You can then find the name and keys of this identity in a new tile in your [wallet tab](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-wallet)

You can also use the Fabric CA client or Fabric SDKs to enroll the identities you create in the console. The console provides you with all of the information that need to compete these steps. Ensure that you have saved the **Enroll ID** and **Enroll secret** that you specified during registration.

## Using your TLS CA
{: #ibm-hlfsupport-console-identities-tlsca}

The communication within your network is secured by TLS certificates. TLS encrypts the communication between your nodes and between your nodes and your applications. Using TLS prevents attackers from disrupting the communication between your nodes or reading the transactions that are submitted from your applications. The keys and certificates that are used for TLS are different than the certificates used to sign and validate your transactions and are issued by a separate Certificate Authority.

Each CA created by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console contains a root CA and a TLS CA. Both of these CAs use the same database and share the list of registered users. As a result, each identity that is registered with your root CA is automatically registered with your TLS CA as well. The CA admin of your root CA is also the admin for your TLS CA.

You can use each identity in the Registered Users table to generate TLS certificates. Follow the same process as you used to [enroll an identity](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-enroll) by using your root CA. However, on the **Enroll identity** panel, select the **TLS Certificate Authority** from the **Certificate Authorities** drop-down list.

Each peer or orderer node that you deploy needs to generate a public TLS certificate. When you create peer or orderer nodes, you can use the same enroll ID and secret that you used to generate the peer or orderer identity as the TLS enroll ID and secret because the TLS CA uses the same user repository as the organization CA. The node then uses this identity to generate its TLS certificate during deployment. This certificate is required by any application that needs to communicate with the orderer or peer. You can find the TLS certificate of a node by navigating to the node overview panel and clicking Settings. You can also find the TLS Certs of your peers and orderers in the connection profile that can be downloaded [from the organization MSP tile on the organizations tab](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-organizations#ibm-hlfsupport-console-organizations-connx-profile).

When creating a peer or orderer with your console, you can also use the TLS CA to specify an additional domain name for each node. Enter the new domain name in the **TLS CSR hostname** field when deploying your orderer or peer. This hostname will be added to the list of common names in the TLS certificate issued to your node.

## Certificate renewal and expiration
{: #ibm-hlfsupport-console-identities-expiration}

Certificates expire and need to be renewed regularly in a process referred to as "certificate rotation". In a production network, you need to monitor the expiration dates of the various certificates and make plans to renew them before they expire. The platform attempts to automatically renew the enrollment certificates of the peer and ordering nodes 30 days before they expire, but you are responsible for manually renewing the organization admin certificates for your nodes, system channels and application channels. It is recommended that you review the topic on [managing certificates](/docs/hlf-support?topic=hlf-support-cert-mgmt) to learn more about what is required.

## Storing identities in your console wallet
{: #ibm-hlfsupport-console-identities-wallet}

The wallet stores the identities and keys that the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console uses to operate the nodes of your network. You need to add your peer, orderer, and organization admins to this wallet before you can use the console to work with channels and smart contracts. You can also use the wallet to conveniently store the identities you use for your applications. You can use the wallet to export them at any time. Use the left navigation to browse to the wallet overview panel. You can add, update, and export identities from this wallet by using the overview panel.

The wallet is a component of the console and not a separate service. It stores your keys in the local storage of the browser that you use to access the console instead of your local file system. If you access your console from a different browser, or start a private browsing session, you will not be able to access the identities stored in the wallet. **It is recommended that you export your admin identities from the console and store them in a safe place**.
{:important}

### Adding identities
{: #ibm-hlfsupport-console-identities-add}

You can add an admin identity to your wallet when you [create your organization MSP](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-organizations#console-organizations-create-msp). A CA managed by the console can also add an identity to your wallet during the [enrollment process](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities#ibm-hlfsupport-console-identities-enroll).

You can use the **Add identity** button on the overview panel to import an identity directly into the wallet. Clicking this button opens up a side panel that allows you to add an identity's signing certificate and private key.
- **Name:** Enter an identity name that is used for your reference only.
- **Certificate:** Upload a file that contains the identity's signing certificate (in base64 or PEM format) that you generated by using your CA.
- **Private Key:** Upload a file that contains the identity's private key (in base64 or PEM format) that you generated by using your CA.


You can also add an identity by uploading a JSON file in the format below. You can also use the **Upload JSON** button to upload multiple identities at once.

```
{
    "name": "peerAdminCerts",
    "private_key": "LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tDQpNSUdIQWdFQU1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUhCRzB3YXdJQkFRUWdIVk5Kc0tHYnl6eTVZWEQ2DQovaEhKOVZlR0QrZVhmenpxUi9PQWVZYnY5UWloUkFOQ0FBUmZ1WlB1OTRzNnJQSEVCMjJlWFhpSWozd0lKYkg4DQpRYVpaRkJlNFp5SnU5UllHbkxQWUdLRnhETkRVMUZkU1NxUGNLcnczUXpxT3hXNU1NZDVWbEtVdw0KLS0tLS1FTkQgUFJJVkFURSBLRVktLS0tLQ0K",
    "cert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlCNURDQ0FZdWdBd0lCQWdJVUhhUFNNdlcxOEwyaGNTeGlJK1ZqT1d0WnlyZ3dDZ1lJS29aSXpqMEVBd0l3YTHJNM1o5TUZicGt3SGthYnB0MmZBekFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUFmdUhjY2R6WExqZ3BLDQplVFhnNmNNcnRzRUs4ZVlKTVFMNlZLU0xwUXIvN3dJZ1hyK2ZuVjUrb2RHWnNRUU94SjZ1aDVTV0tJVkdZQ2ZGDQp1Ykw4VmJNdnRRZz0NCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0NCg=="
}
```
{: codeblock}

If you did not enroll an identity from the console, but instead by using the Fabric CA client or the Fabric SDKs, your keys need to be converted from PEM format into base64 format. You can convert certificates into base64 format by running the following command on your local machine:
```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-certificate>/cert.pem | base64 $FLAG
```
{: codeblock}

### Viewing and updating identities
{: #ibm-hlfsupport-console-identities-update-identities}

From the **Wallet** tab, click a tile to view, update, or remove an identity from the wallet. It might be necessary to update your identities if their certificates have expired, and they need to be issued new keys from the CA. You can also use this tab to delete keys from your console and your local system.

Clicking an identity opens a side panel that displays its certificates and private keys in base64 format. Click the download button <img src="../images/download.png" alt="Download .pem icon" width="26" style="width:26px; border-style: none"/> to export both the certificate and the private key as a .pem file to your local file system. <br><br>
<img src="../images/export_identity.png" alt="Export identity" style="border-style: none"/><br><br>
Click **Update** to change the identity name in the wallet or paste a new set of keys into the panel. Click **Remove** when you no longer need to use this identity and want to delete its keys.

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric can auto renew the enrolment (signing) certificates for the peer and ordering nodes and the peer TLS certificate. For the other certificates, you can see [certificate types and actions](/docs/hlf-support?topic=hlf-support-cert-mgmt#cert-mgmt-cert-types) to learn on how to maintain them.

A one stop solution is also available for easy management and maintenance on your certificates. You can store them in {{site.data.keyword.cloud_notm}} Certificate Manager and receive notification before they expire. See what [{{site.data.keyword.cloud_notm}} Certificate Manager](/docs/certificate-manager?topic=certificate-manager-getting-started) can do for you in details.

## Storing and managing certificates in IBM Cloud Certificate Manager
After downloading the identity’s certificate and its private key as a .pem file, you can use the {{site.data.keyword.cloud_notm}} Certificate Manager service dashboard to obtain, store and manage all your SSL/TLS certificates. To get started with using {{site.data.keyword.cloud_notm}} Certificate Manager, see the steps in [set up procedures](/docs/certificate-manager?topic=certificate-manager-getting-started#getting-started-step1). To import certificate and private key in certificate manager, select **Import certificate** from the left navigation. Then, input the **Name** for the certificate. Browse and upload the certificate and private key you downloaded to your local system from **Wallet**. Click **Import** to store the certificate. Importing certificate into certificate manager allows you to easily manage and store all the third parties certificates in a centralize repository. In addition, you can receive notifications to renew the certificate before it expires to avoid service disruption. See how to [import certificates in .pem files](/docs/certificate-manager?topic=certificate-manager-managing-certificates-from-the-dashboard#importing-a-certificate) to learn more.
<br><br>
<img src="../images/import_certificate.png" alt="Import certificate" style="border-style: none"/>
<br><br>

It is your company’s responsibility to ensure all certificates are up to date and get renewal before they expire.  Expired certificates may cause service disruption to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric. {{site.data.keyword.cloud_notm}} Certificate Manager is a central repository for your company to easily maintain and store all the certificates but it does not automatically re-order or re-enrol them. To avoid any service downtime, you can set up notifications in {{site.data.keyword.cloud_notm}} Certificate Manager to remind your company to renew the certificates before it is too late. See [configure notifications](/docs/certificate-manager?topic=certificate-manager-configuring-notifications) in {{site.data.keyword.cloud_notm}} Certificate Manager to learn more.
{: important}

## Storing identities in a Hardware Security Module (HSM)
{: #ibm-hlfsupport-console-identities-hsm}
In a production environment you have the option to generate and store private keys in an HSM. An HSM is a hardware appliance that performs cryptographic operations and provides the capability to ensure that the private keys never leave the HSM. The HSM then allows your peers to sign and endorse transactions without exposing their private keys. Fabric supports HSM devices that implement the [PKCS11 standard](http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html){: external}. PKCS11 is a cryptographic standard for secure operations, generation, and storage of keys.

There are three ways you can use an HSM with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric:
1. Utilize the HSM appliance that is available in [{{site.data.keyword.cloud_notm}}](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-hsm-gemalto).
2. Configure your own HSM.
3. For test purposes only, configure open source [SoftHSM](https://github.com/opendnssec/SoftHSMv2){: external}. You can use SoftHSM for cryptographic operations of your peer, ordering node or CA inside an HSM partition.

After you have configured an HSM, you need to configure a HSM proxy that allows the blockchain node to communicate with the HSM. Then when you configure a blockchain node, you will need to provide three pieces of information:
* **Proxy endpoint:** ClusterIP address that is generated for the HSM when the HSM proxy is configured.
* **Label:** Corresponds to the HSM Partition.
* **PIN:** The PIN associated with the HSM slot.

After HSM is configured for a node, the HSM becomes the cryptographic service provider for the node.  When the HSM is subsequently used to generate and store a node's private key, and the node identity is exported or stored in the wallet, the private key is not visible.

For more information about how to deploy the HSM proxy and use HSM with a CA, peer, or ordering node, see [Configuring a node to use HSM](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-cfg-hsm)

## Associating identities
{: #ibm-hlfsupport-console-identities-associate-admin}

You need to provide the signing certificate of your organization and node admins [to your organization MSP definition](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-organizations#console-organizations-create-msp). The nodes or channels that are created by the console uses the certificates from the MSP definition to check who is a valid administrator. As a result, the same signing certificate and private key that you used to add an admin cert to the MSP definition needs to be stored inside your console wallet.

When you use the console to create an orderer or peer, you will encounter an **Associate identity** panel. Select an identity in the wallet whose certificate is also inside your organization MSP definition. You will also need to select an admin identity in the **Associate identity** section when you create or edit a channel. This allows your console to know which identity to use when it communicates with your peers, orderers, and ordering service consortium. The identity that is currently associated with a peer or ordering service is visible on the left side of the node panel, below the name, Node location, and Fabric version.

## Viewing the contents of a signing certificate
{: #ibm-hlfsupport-console-identities-sign-cert}
{: help}
{: support}

As a network operator, there may be situations when you need to view the contents of a signing certificate, or sign cert, to debug a problem.
If you have the certificate `PEM` file, you can run the following openSSL command to print out the certificate contents:

```
openssl x509 -in <certificate.pem> -text -noout
```
{: codeblock}

Replace `<certificate.pem>` with the sign cert file and the output will resemble the following content:
```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            68:b9:6d:d3:00:1e:1b:9d:4b:99:1e:ab:ed:05:48:be:28:51:69:3d
    Signature Algorithm: ecdsa-with-SHA256
        Issuer: C=US, ST=North Carolina, O=Hyperledger, OU=Fabric, CN=fabric-ca-server
        Validity
            Not Before: Nov 20 19:08:00 2019 GMT
            Not After : Nov 19 19:13:00 2020 GMT
        Subject: OU=client, OU=org1, OU=department1, CN=user1
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                pub:
                    04:11:0c:c9:9e:47:d8:38:aa:c7:24:65:90:98:a3:
                    42:a8:b0:18:3e:03:43:13:ae:64:6a:18:93:6e:06:
                    23:30:09:c1:93:66:5f:0d:d6:37:1b:a0:c4:e9:cb:
                    9b:70:63:9a:af:08:20:f2:96:b8:de:3b:c5:6e:58:
                    a9:9e:fa:d6:de
                ASN1 OID: prime256v1
                NIST CURVE: P-256
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature
            X509v3 Basic Constraints: critical
                CA:FALSE
            X509v3 Subject Key Identifier:
                E4:86:14:C1:58:21:D8:6F:B4:79:E5:41:3B:14:9B:3D:C6:36:87:36
            X509v3 Authority Key Identifier:
                keyid:70:CA:7D:2E:5A:75:A2:90:27:8C:5D:7D:C7:AF:F1:2C:E1:25:59:70

            1.2.3.4.5.6.7.8.1:
                {"attrs":{"hf.Affiliation":"org1.department1","hf.EnrollmentID":"user1","hf.Type":"client"}}
    Signature Algorithm: ecdsa-with-SHA256
         30:44:02:20:1a:38:64:47:69:6b:31:e4:96:bc:52:06:71:75:
         9e:01:57:a4:bd:b0:15:f0:19:c4:23:37:29:14:b1:66:e3:ce:
         02:20:7a:34:af:7b:fb:65:6c:b8:22:a2:39:78:5d:30:5c:3c:
         b0:9b:0e:14:d8:76:78:9a:34:2b:bb:d2:97:d4:ce:81
```   
