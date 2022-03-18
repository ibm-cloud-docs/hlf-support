---

copyright:
  years: 2022
lastupdated: "2022-03-18"

keywords: APIs, build a network, authentication, service credentials, API key, API endpoint, IAM access token, Fabric CA client, import a network, generate certificates

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}



# Using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs
{: #ibm-hlfsupport-v2-apis}


The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric exposes RESTful APIs for you to create, import, edit, and delete your blockchain components, as well as to manage logging, notifications, and console settings. You can use the APIs, and the corresponding SDKs, to develop applications that interact with your blockchain network.
{: shortdesc}

This tutorial introduces the generic flow to build a blockchain network with {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs. For more information about each API, see [{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric API reference doc](/apidocs/blockchain){: external}.

## Swagger
{: #ibm-hlfsupport-v2-apis-swagger}

After you review the instructions in this topic on how to use the APIs, you can alternatively access a Swagger version of the APIs instead of the [APIs in {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/apidocs/blockchain){: external} where the current version is `v2`. If you need to access the `v1` version of the APIs, you can import the `v1` version into the [Swagger editor](http://editor.swagger.io){: external}.

- [v1](https://github.com/ibm-cloud-docs/blockchain/blob/master/reference/ibm-hlfsupport-v1.yaml){: external}
- [v2](https://github.com/ibm-cloud-docs/blockchain/blob/master/reference/ibm-hlfsupport-v2.yaml){: external}

## Prerequisites
{: #ibm-hlfsupport-v2-apis-prereq}

The APIs target your Fabric Operations Console to authenticate calls and communicate with your nodes. Therefore you must deploy the Fabric Operations Console before you can start using the Fabric Operations Console APIs. If you have not yet deployed the console on your cluster, see [Getting started with {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric](/docs/hlf-support?topic=hlf-support-get-started-console-ocp).

To use the APIs, you will need to gather the following information:

- The console **API Endpoint**. This is the URL that you use to access the console using your browser.
- A username and password that you can use to access the console. To create an [API key](#x8051010){: term}, you must have an account with a [manager role](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-users).

## Connect to your console using API keys
{: #console-icp-manage-api-key}

Each console provides its own identity and access management. You can use your console username and password to generate an API key and secret that can authorize your API calls.

Each API key is associated with a role that governs the user's permissions. The API keys use the same access policy roles as exist for users who login to the console using a username and password. Refer to the table in the [Managing users](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-users) section for the list of actions each role is allowed to perform. Because only manager roles can create API keys, a console administrator needs to create API keys for reader and writer users. The API keys never expire. As a result, the console administrator should delete API keys that are not being used.

Before you proceed, it is worth reviewing the topic on [Understanding how the API key works](/docs/containers?topic=containers-users#removing_check_infra) paying particular attention to the question `What happens if the user who set up the API key for a region and resource group leaves the company?`. The impact of this particular action could disable your network.
{: important}  

There are three APIs available to manage your API keys:
- [Create an API key](#console-icp-manage-create-api-key)
- [View API keys](#console-icp-manage-view-api-keys)
- [Delete API keys](#console-icp-manage-delete-api-keys)

Use the request below to create an API key and secret. You can then use this key and secret to use the other APIs. The API secret is not stored by the console and needs to be saved by the user.

### Create an API key
{: #console-icp-manage-create-api-key}

| **Request** |  |
|-------------|-----------|
| PATH | POST `<API_endpoint>`/ak/api/v2/permissions/keys |
| **Request body fields** | |
| <ul><li><code>roles</code></li><li><code>description</code></li></ul>| <ul><li><code>["reader", "writer", "manager"]</code> At least one value is required </li><li><code>string</code> optional</li></ul>|
| **Response body fields** | |
| <ul><li><code>api_key</code></li><li><code>description</code></li><li><code>roles</code></li></ul>| <ul><li><code>string</code></li><li><code>string</code> Save: the key is not stored</li><li><code>["<role>"]</code></li></ul>|
| Authorization required | manager |

### Example curl request: Create API key
{: #console-icp-manage-create-api-key-example}

```bash
curl -X POST \
    https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/permissions/keys \
    -u user@email.com:password \
    -k \
    -H 'Content-Type: application/json' \
    -d '{
        "roles": ["writer", "manager"],
        "description": "newkey"
        }'
```
{: codeblock}

Once you have created an API key and secret, you can use the APIs to view or remove the keys that you have created. The keys and secrets do not expire until they are deleted.

### View API keys
{: #console-icp-manage-view-api-keys}

| **Request** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v2/permissions/keys |
| **Response body fields** | |
| <ul><li><code>api_key</code></li><li><code>roles</code></li><li><code>ts_created</code></li><li><code>description</code></li></ul>| <ul><li><code>string</code></li><li><code>["<role>"]</code></li><li><code>number</code> Unix timestamp in milliseconds</li><li><code>string</code></li></ul>|
| Authorization required | reader |

### Example curl request: view API keys
{: #console-icp-manage-view-api-key-example}

```bash
curl -X GET \
    https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/permissions/keys \
    -k \
    -u <api_key>:<api_secret>
```
{: codeblock}

### Delete API keys
{: #console-icp-manage-delete-api-keys}

| **Request** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v2/permissions/keys/`<api_key>` |
| Authorization required| manager |

### Example curl request: delete API key
{: #console-icp-manage-delete-api-keys-example}


```bash
curl -X DELETE \
    https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/permissions/keys/<api_key> \
    -k \
    -u <api_key>:<api_secret>
```
{: codeblock}

## Managing users using the APIs
{: #console-icp-manage-users-apis}


You can also use the APIs to list, add, or remove users who can log in to the console or access the console APIs. You can also edit the users roles to upgrade a users level of access. The following APIs are available:

- [List users](#console-icp-manage-list-users-api)
- [Edit users](#console-icp-manage-edit-users-api)
- [Add users](#console-icp-manage-add-users-api)
- [Remove users](#console-icp-manage-remove-users-api)

For the APIs that govern the users and settings of your console, and manage the nodes of your network, you need to add a `-k` or ``--insecure`` flag to bypass the requirement for a TLS certificate. You can also download the TLS certificate from your console using your browser.

### List users
{: #console-icp-manage-list-users-api}


| **Request** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v2/permissions/users |
| **Response body fields** | |
| <ul><li><code>uuids</code></li><li><code>email</code></li><li><code>roles</code></li><li><code>created</code></li></ul>| <ul><li><code>string</code> user id</li><li><code>string</code> email address</li><li><code>["<role>"]</code></li><li><code>number</code> Unix timestamp in milliseconds</li></ul>|
| Authorization required | reader |

### Example curl request: list users
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
    https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/permissions/users \
    -u <api_key>:<api_secret> \
    -k
```
{: codeblock}

### Edit users
{: #console-icp-manage-edit-users-api}

| **Request** |  |
|-------------|-----------|
| Path | PUT `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li><code>uuids</code></li><li><code>roles</code></li></ul> | <ul><li><code>array of strings</code> user ids </li><li><code>["reader", "writer", "manager"]</code> At least one value is required</li></ul> |
| **Response body fields** | |
| <ul><li><code>uuids</code></li></ul>| <ul><li><code>array of strings</code> user ids</li></ul>|
| Authorization| manager |

### Example curl request: edit a user
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
    https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/permissions/users \
    -u <api_key>:<api_secret> \
    -k \
    -H 'Content-Type: application/json' \
    -d '{
    "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```
{: codeblock}

### Add users
{: #console-icp-manage-add-users-api}

| **Request** |  |
|-------------|-----------|
| Path | POST `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li><code>roles</code></li><li><code>description</code></li></ul>| <ul><li><code>["reader", "writer", "manager"]</code> At least one value is required </li><li><code>string</code> optional</li></ul>|
| Authorization | manager |

### Example curl request: add a user
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
    https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/permissions/users \
    -u <api_key>:<api_secret> \
    -k \
    -H 'Content-Type: application/json' \
    -d '{
    "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```
{: codeblock}

### Remove users
{: #console-icp-manage-remove-users-api}

| **Request** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v2/permissions/users |
| **Requests query parameter** | |
| <ul><li><code>uuids</code></li></ul>| <ul><li><code>string</code> user id</li></ul>|
| **Response body fields** | |
| <ul><li><code>uuids</code></li></ul>| <ul><li><code>string</code> user id</li></ul>|
| Authorization | manager |

### Example curl request: remove a user
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
    https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/permissions/users \
    -u <api_key>:<api_secret> \
    -k \
    -H 'Content-Type: application/json' \
    -d '{
    "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```
{: codeblock}

## Use the APIs to manage your components

You can also use the API's to create, delete, or edit your blockchain nodes. You can view the complete set of APIs that manage your nodes and console settings in the [{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric API reference](https://cloud.ibm.com/apidocs/blockchain).

Because you are using the APIs to communicate with your console on your Kubernetes, you need to substitute the authentication provided by {{site.data.keyword.cloud_notm}} with the API Key provided by your console. Replace the `Bearer Auth` in the API reference with  `-u <api_key>:<api_secret>`. You also need to add a `-k` or ``--insecure`` flag to the command, or download the console TLS certificate using your browser.

As an example, the API call below will return information about all of the components that are managed by your console.

```
curl -X GET \
    https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v2/components \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
{: codeblock}

The same API call would resemble the request below for a console deployed with {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.

```
curl -X GET \
https://openshiftcluster.us-south.containers.appdomain.cloud:443/ak/api/v2/components \
    -H 'Content-Type: application/json' \
    -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
    -k
```
{: codeblock}

In order to create nodes using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs, you need to use the APIs in a certain sequence in conjunction with Fabric CA client. To learn how to create nodes, see [Build a network by using APIs](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-apis#ibm-hlfsupport-v2-apis-build-with-apis).

You can also use the APIs to import and then operate nodes that reside in other clusters. For more information, see [Import a network by using APIs](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-apis#ibm-hlfsupport-v2-apis-import-with-apis).

## Limitations
{: #ibm-hlfsupport-v2-apis-limitations}

You can only import CA, peer, and ordering nodes that are exported from other instances of Fabric Operations Console running on {{site.data.keyword.cloud_notm}}, OpenShift Container Platform, Red Hat Open Kubernetes Distribution, or any Kubernetes v1.19 - v1.23 container platform on x86_64. The platform is also supported on LinuxONE (s390x) using OpenShift Container Platform.

## Building a network by using APIs
{: #ibm-hlfsupport-v2-apis-build-with-apis}

You can use APIs to create blockchain components in your instance of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric. Use the following steps to build a blockchain network by using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs.

1. Create a Certificate Authority (CA) by calling [`POST /ak/api/v2/kubernetes/components/ca`](/apidocs/blockchain#create-a-ca).

    Remember your input and the response, you will need them later.
    {: tip}

    You need to wait for the CA to start. It might take several minutes depending on environment. You can call `GET <ca_url>/cainfo` API to check your CA status. You will get repeated errors, then a `200` status code, which means you can proceed to the next step. Note that this API call times out after one minute.

2. Use your CA to register your component and administrator identities, and generate the necessary certificates. You can use the Fabric CA client to complete the following steps:

    - [Set up the Fabric CA client](#ibm-hlfsupport-v2-apis-config-fabric-ca-client).
    - [Generate certificates with your CA admin](#ibm-hlfsupport-v2-apis-enroll-ca-admin).
    - [Register the new component with your CA](#ibm-hlfsupport-v2-apis-config-register-component).
    - You also need to [register an organization administrator](#ibm-hlfsupport-v2-apis-config-register-admin) and then [generate certificates for the admin](#ibm-hlfsupport-v2-apis-config-enroll-admin) inside an MSP folder. You do not have to complete this step if you have already registered your admin identity.
    - [Register the new component with your TLS CA](#ibm-hlfsupport-v2-apis-config-register-component-tls).

    You can also complete these steps by using your Fabric Operations Console. For more information,  see [Creating and managing identities](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-identities).

3. [Create an MSP definition for your organization](#ibm-hlfsupport-v2-apis-msp) by calling [`POST /ak/api/v2/components/msp`](/apidocs/blockchain?#import-an-msp).

4. [Build the configuration file](#ibm-hlfsupport-v2-apis-config) that is required to create an ordering service or peer. You must build a unique configuration file for each ordering service or peer that you want to create. If you are deploying multiple ordering nodes, you need to provide a configuration file for each node that you want to create.

5. Create an ordering service by calling [`POST /ak/api/v2/kubernetes/components/orderer`](/apidocs/blockchain#create-an-ordering-service).

6. Create a peer by calling [`POST /ak/api/v2/kubernetes/components/peer`](/apidocs/blockchain#create-a-peer).

7. If you want to use the console to operate your blockchain components, you must import your administrator identity into your console wallet. Use the wallet tab to import the certificate and private key of your node admin into the console and create an identity. You then need to use the console to associate this identity with the components you created. For more information, see [Importing an admin identity into the Fabric Operations Console](#ibm-hlfsupport-v2-apis-admin-console).

8. After you deploy your network, you can use the Fabric SDKs, the Peer CLI, or the console UI to create channels and deploy smart contracts. If you need to programmatically create a channel, you must provide the consortium name. For {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, the consortium name must be set to `SampleConsortium`.




The service credential that is used for API authentication must have the `Manager` role in IAM to be able to create components. See the table on [user roles](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-role-mapping)
for more information.
{: note}


### Creating a node within a specific zone
{: #ibm-hlfsupport-v2-apis-zone}

If you are using a multizone cluster, you can use the APIs to deploy a blockchain component to a specific zone of {{site.data.keyword.cloud_notm}}. This allows your network to maintain availability in the event of a zone failure. You can use the following steps to deploy a peer or ordering node to a specific zone.




1. Find the zones where your worker nodes are located by using your Kubernetes cluster CLI. If you are using OpenShift Container Platform, after you connect to your cluster by using the CLI, use the command `oc get nodes --show-labels` to get the full list of nodes and zones of your cluster. You will be to find the zone that each worker node is located after `failure-domain.beta.kubernetes.io/zone` field under the `LABELS` column.


2. To create a node within a specific zone, provide the zone name to the [Create an ordering service](/apidocs/blockchain#create-an-ordering-service) or [Create a peer](/apidocs/blockchain#create-a-peer) API calls by using the zone field of the request body. The anti-affinity policy of the Fabric Operations Console will automatically deploy your component to different worker nodes within each zone based on the resources available.


## Creating a node with a custom configuration
{: #ibm-hlfsupport-v2-apis-custom}

If you are using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs to deploy a CA, peer, or ordering node, you have the option of customizing the node configuration by using a configuration override JSON string. The nodes that are deployed by the Fabric Operations Console and APIs are configured with the default Fabric values that are provided in the  `fabric-ca-server-config.yaml`, `orderer.yaml`, and `core.yaml` files. You can customize your node settings by providing a configuration override JSON to the APIs that create or update your nodes. You can use the configuration override to deploy a High Availability CA or use a Hardware Security Module (HSM) while using the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs. For more information about the configuration override, High Availability CAs, or HSMs, see [Advanced deployment options](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment).

### Example: Creating a custom Certificate Authority

If you are using the [Create a CA](/apidocs/blockchain#create-a-ca) API to deploy a Certificate Authority, you need to use the configuration override JSON string to set the CA admin enrollID and secret. For example, if you wanted to create a CA with an administrator enrollID and secret of `admin` and `adminpw`, you would issue the following command. You can use the command to create multiple admins. The API would create a CA that uses the default values for all other fields.
```
curl -X POST "https://{API-Endpoint}/ak/api/v2/kubernetes/components/fabric-ca" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer {IAM-Token}" \
-d "{
    \"display_name\": \"My CA\",
    \"config_override\":{
        \"ca\":{
          \"registry\":{
            \"maxenrollments\": -1,
            \"identities\": [{
              \"name\": \"admin\",
              \"pass\": \"password\",
              \"type\": \"client\",
              \"affiliation\": \"\",
              \"attrs\":{
                    \"hf.Registrar.Roles\": \"*\",
                    \"hf.Registrar.DelegateRoles\": \"*\",
                    \"hf.Revoker\": true,
                    \"hf.IntermediateCA\": true,
                    \"hf.GenCRL\": true,
                    \"hf.Registrar.Attributes\": \"*\",
                    \"hf.AffiliationMgr\": true
              }
            }]
          }
        }
        }
    }"
```
{: codeblock}

You use also the `"configoverride"` to create or update a CA with custom settings for your organization. This provides you with more control over the identities and certificates that are created by your CA. For example, you would use the following command to set your organization name and location.
```
curl -X POST \
https://<API endpoint>/ak/api/v2/kubernetes/components/fabric-ca \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <IAM_token>' \
-d "{
    "display_name": "Sample CA",
    "configoverride": {
      "ca": {
        "registry": {
            "maxenrollments": -1,
            "identities": [
              {
                "name": "admin",
                "pass": "adminpw",
                "type": "admin",
                "affiliation": "",
                "attrs": {
                  "hf.Registrar.Roles": "*",
                  "hf.Registrar.DelegateRoles": "*",
                  "hf.Revoker": true,
                  "hf.IntermediateCA": true,
                  "hf.GenCRL": true,
                  "hf.Registrar.Attributes": "*",
                  "hf.AffiliationMgr": true
                }
              }
            }
          },
          "csr": {
            "names": [
              {
                "C": "US",
                "ST": "New York",
                "L": null,
                "O": "Big business",
                "OU": "Big department"
              }
              ],
            }
        }
    }"
```
{: codeblock}

For more information about deploying a customized CA, and which fields you can customize after a CA is created, see [Customizing a CA configuration](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-ca-customization).

### Create a high availability CA

You can use configuration override to deploy a CA with replica sets that share the same database, ensuring that the data is consistent between replicas. This configuration ensures that the CA will be available in the event of a Kubernetes worker node failure. To deploy an HA CA, you need to deploy a PostgreSQL database on {{site.data.keyword.cloud_notm}} or in the environment of your choice. You then need to use the information about your database to create a connection file that will be used by your CA. For more information, see [Building a high availability Certificate Authority](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-ha-ca#ibm-hlfsupport-console-build-ha-ca).

To use the APIs to deploy an HA CA, you need to provide the database connection file to the `"db"` section of the config override JSON string. For example, the API request below will deploy a CA with two replicas that connect to a database located on {{site.data.keyword.cloud_notm}}.
```
curl -X POST \
https://<API endpoint>/ak/api/v2/kubernetes/components/fabric-ca \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <IAM_token>' \
-d '{
    "display_name": "Sample CA",
    "replicas": 2,
    "configoverride": {
      "ca": {  
        "db": {
          "datasource": "host=test.databases.appdomain.cloud port=31941 user=ibm_cloud password=password dbname=ibmclouddb sslmode=verify-full",
        "tls": {
          "certfiles": [
            "<base64 encoded pem>"
],
          "enabled": true
                },
        "type": "postgres"
            }
        },
      "tlsca": {
        "db": {
          "datasource": "host=test.databases.appdomain.cloud port=31941 user=ibm_cloud password=password dbname=ibmclouddb sslmode=verify-full",
        "tls": {
          "certfiles": [
                    "<base64 encoded pem>"
                    ],
        "enabled": true
                },
        "type": "postgres"
              }
        "registry": {
            "maxenrollments": -1,
            "identities": [
              {
                "name": "admin",
                "pass": "adminpw",
                "type": "admin",
                "affiliation": "",
                "attrs": {
                  "hf.Registrar.Roles": "*",
                  "hf.Registrar.DelegateRoles": "*",
                  "hf.Revoker": true,
                  "hf.IntermediateCA": true,
                  "hf.GenCRL": true,
                  "hf.Registrar.Attributes": "*",
                  "hf.AffiliationMgr": true
                }
              }
            }
    }    
    }
}'
```
{: codeblock}

### Deploy a node that uses an HSM

{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric allows you to deploy CA, peer, or orderer nodes that use an HSM to store their private key. To use an HSM with your blockchain network, you need to first set up an HSM on {{site.data.keyword.cloud_notm}} or in your own environment. You can then create a node with the private key that is stored in an HSM partition by providing the HSM endpoint along with the slot key and pin before the node is deployed. For more information, see [Configuring a node to use an HSM](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-adv-deployment#ibm-hlfsupport-console-adv-deployment-cfg-hsm).

If you are using the APIs to deploy a node, you need to provide the HSM endpoint to the HSM field of the API call. You also need to use the config override to provide the label and pin of the HSM slot that you will use. As an example The following API call deploys a peer node with an HSM.
```
curl -X POST "https://{API-Endpoint}/ak/api/v2/kubernetes/components/fabric-peer" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer {IAM-Token}" \
-d "{
    \"display_name\": \"My Peer\",
    \"msp_id\": \"org2\",
    \"config\": {
        \"enrollment\": {
          \"component\": {
            \"cahost\": \"n3a3ec3-myca.ibp.us-south.containers.appdomain.cloud\",
            \"caport\": \"7054\",
            \"caname\": \"ca\",
            \"catls\": {
              \"cacert\": \"LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCkNlcnQgZGF0YSB3b3VsZCBiZSBoZXJlIGlmIHRoaXMgd2FzIHJlYWwKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo\"
            },
            \"enrollid\": \"admin\",
            \"enrollsecret\": \"password\",
            \"admincerts\": [
              \"LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCkFkbWluIGNlcnQgZGF0YSB3b3VsZCBiZSBoZXJlIGlmIHRoaXMgd2FzIHJlYWwKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=\"
            ]
          },
          \"tls\": {
            \"cahost\": \"n3a3ec3-myca.ibp.us-south.containers.appdomain.cloud\",
            \"caport\": \"7054\",
            \"caname\": \"tlsca\",
            \"catls\": {
              \"cacert\": \"LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCkNlcnQgZGF0YSB3b3VsZCBiZSBoZXJlIGlmIHRoaXMgd2FzIHJlYWwKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo\"
            },
            \"enrollid\": \"admin\",
            \"enrollsecret\": \"password\",
            \"admincerts\": [
              \"LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCkFkbWluIGNlcnQgZGF0YSB3b3VsZCBiZSBoZXJlIGlmIHRoaXMgd2FzIHJlYWwKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=\"
            ]
          }
        }
        },
        \"hsm\": {
            \"pkcs11endpoint\": \"tcp://example.com:666\",
        },
        \"config_override\": {
            \"bccsp\": {
            \"default\": \"PKCS11\",
            \"pkcs11\": {
                \"label\": \"blockchain\",
                "pin": \"91927001\"
                }
            }
        }
    }"
```
{: codeblock}

## Import a network by using APIs
{: #ibm-hlfsupport-v2-apis-import-with-apis}

You can also use the APIs to import {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric components that are created by using the APIs or the Fabric Operations Console into another service instance of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.

1. Import a CA by calling [`POST /ak/api/v2/components/ca`](/apidocs/blockchain#import-a-ca).

    Remember your input and the response, you will need them later.
    {: tip}

    You need to wait for the CA to start. It might take several minutes depending on environment. You can call [`GET /components`](/apidocs/blockchain#get-all-components) to check the CA status. You will get repeated errors before you get a `200` status code to go to next step. Note that this API call times out in one minute.

2. Import an organization MSP definition by calling [`POST /ak/api/v2/components/msp`](/apidocs/blockchain#import-an-msp).

3. Import an ordering service by calling [`POST /ak/api/v2/components/orderer`](/apidocs/blockchain#import-an-ordering-service).

4. Import a peer by calling [`POST /ak/api/v2/components/peer`](/apidocs/blockchain#import-a-peer).

5. If you plan to use the Fabric Operations Console to operate your blockchain components, you must import your component administrator identities into your console wallet. For more information, see [Importing an admin identity into the Fabric Operations Console](#ibm-hlfsupport-v2-apis-admin-console).

6. After you deploy your network, you can use the Fabric SDKs, the Peer CLI, or the console UI to create channels and deploy smart contracts. If you need to programmatically create a channel, you must provide the consortium name. For {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, the consortium name must be set to `SampleConsortium`.




The service credential that is used for API authentication must have the `Manager` role in IAM to be able to create components. See the table on [user roles](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-role-mapping)
for more information.
{: note}


## Operating your CA with the Fabric CA client
{: #ibm-hlfsupport-v2-apis-config-fabric-ca-client}

You can use the Fabric CA client to operate your CAs. Run the following Fabric CA client commands to register your component and administrator identities and generate the necessary certificates.

### Set up the Fabric CA client
{: #ibm-hlfsupport-v2-apis-setup-fabric-ca-client}

1. Download the [Fabric CA client](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#fabric-ca-client){: external} to your local file system.

    The easiest way to get the Fabric CA client is to download all of the Fabric tool binaries directly. Navigate to a directory where you would like to download the binaries with your command line, and fetch them by running the following [cURL](https://hyperledger-fabric.readthedocs.io/en/release-2.2/prereqs.html#install-curl){: external} command.

    ```
    curl -sSL http://bit.ly/2ysbOFE | bash -s 1.4.3 1.4.3 -d -s
    ```
    {: codeblock}

    This command installs the binaries in a `bin/` directory.

2. Set the `PATH` path to the directory where you downloaded the Fabric tool binaries:

    ```
    export PATH=$PATH:<full/path/to/fabric-ca-client/bin>
    ```
    {: codeblock}

    For example, if you installed the binaries in your home directory you would set your `PATH` as:

    ```
    export PATH=$PATH:$HOME/bin
    ```
    {: codeblock}

3. Create the folder where you will store the certificates of your CA admin.

    ```
    mkdir -p $HOME/fabric-ca-client/ca-admin
    ```
    {: codeblock}

4. Set the value of the `$FABRIC_CA_CLIENT_HOME` environment variable to be the path where the CA client will store the generated MSP certificates. Ensure that you remove the configuration material that might be created by earlier attempts. If you didn't run the `enroll` command before, the `msp` folder and the `.yaml` file do not exist.

    ```
    export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/ca-admin
    rm -rf $FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml
    rm -rf $FABRIC_CA_CLIENT_HOME/msp
    ```
    {: codeblock}

5. Retrieve the TLS certificate of your CA to be used by the Fabric CA client. If you are using the Fabric Operations Console, open the CA and click **Settings**, and look for the certificate in base64 format in the **TLS Certificate** field. If your are using the APIs, you can call [`GET /ak/api/v2/components`](/apidocs/blockchain#get-all-components) and find the CA TLS certificate in the `"PEM"` field. If you created the CA by using the `Create a Fabric CA` API, you can also find the TLS certificate in the response body.

    You need to convert the certificate from base64 into PEM format to use it to communicate with your CA. Insert the base64 encoded string of the TLS certificate into command below. Ensure that you are in your `$HOME/fabric-ca-client` directory.

    ```
    cd $HOME/fabric-ca-client
    mkdir catls
    export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
    echo <base64_string> | base64 --decode $FLAG > tls.pem
    ```
    {: codeblock}

### Generate certificates with your CA admin
{: #ibm-hlfsupport-v2-apis-enroll-ca-admin}

A **CA admin** identity was automatically registered for you when you created your CA. You can now use that admin name and password to issue an `enroll` command with the Fabric CA client to generate an MSP folder with certificates that can then be used to register other peer or ordering node identities.

1. Ensure that you complete the steps to [set up the Fabric CA client](#ibm-hlfsupport-v2-apis-config-fabric-ca-client) and that `$FABRIC_CA_CLIENT_HOME` is set to the directory where you want to store your CA admin certs.

    ```
    echo $FABRIC_CA_CLIENT_HOME
    $HOME/fabric-ca-client/ca-admin
    ```
    {: codeblock}

2. Run the following command to generate certificates:

    ```
    fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name>  --tls.certfiles   <ca_tls_cert_path>
    ```
    {: codeblock}

    The `<enroll_id>`and `<enroll_password>` in the command are the `enroll_id` and `enroll_secret` you specified when you created the CA. Use the **Certificate Authority Endpoint URL** from your Fabric Operations Console or the `"ca_url"` returned by your API call as the value for `<ca_url_with_port>`. Leave off the `http://` at the beginning. The `<ca_name>` is the **CA Name** from your console, or the `"ca_name"` returned by the APIs.

    The `<ca_tls_cert_path>` is the full path your CA TLS cert.

    A real call might look similar to the following example command:

    ```
    fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
    ```  
    {: codeblock}

**Tip:** If the value of the enrollment URL, which is the `-u` parameter value, contains a special character, you need to either encode the special character or surround the URL with the single quotation marks. For example, `!` becomes `%21`, or the command looks like:

    ```
    ./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname ca
    ```
    {: codeblock}

    The `enroll` command generates a complete set of certificates, which is known as a Membership Service Provider (MSP) folder, that is located inside the directory where you set to `$HOME` path for your Fabric CA client. For example, `$HOME/fabric-ca-client/ca-admin`. For more information about MSPs and what the MSP folder contains, see the [Membership Service Providers](https://hyperledger-fabric.readthedocs.io/en/release-2.2/msp.html){: external} concept topic in the Fabric documentation.

    If the `enroll` command fails, see the [Troubleshooting topic](#ibm-hlfsupport-v2-apis-config-troubleshooting) for possible causes.

    You can run a tree command to verify that you have completed all of the prerequisite steps. Navigate to the directory where you stored your certificates. A tree command should generate a result similar to the following structure:

    ```
    cd $HOME/fabric-ca-client
    tree
    .
    ├── ca-admin
    │   ├── fabric-ca-client-config.yaml
    │   └── msp
    │       ├── cacerts
    │       │   └── 9-30-250-70-30395-SampleOrgCA.pem
    │       ├── IssuerPublicKey
    │       ├── IssuerRevocationPublicKey
    │       ├── keystore
    │       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
    │       ├── signcerts
    │       │   └── cert.pem
    │       └── user
    └── catls
        └── tls.pem    
    ```
    {: codeblock}

### Registering the component identity with the CA
{: #ibm-hlfsupport-v2-apis-config-register-component}

First, you need to register a component identity with your CA. Your component uses this identity to generate the certificates it needs when it is deployed.

1. [Generate certificates with your CA admin](#ibm-hlfsupport-v2-apis-enroll-ca-admin) by using the Fabric CA client. Use these admin certificates to issue the following commands. Ensure that `$FABRIC_CA_CLIENT_HOME` is set to `$HOME/fabric-ca-client/ca-admin`.

    ```
    echo $FABRIC_CA_CLIENT_HOME
    $HOME/fabric-ca-client/ca-admin
    ```
    {: codeblock}

2. Issue the following command to find your affiliation and your organization name.
    ```
    fabric-ca-client affiliation list --caname <ca_name> --tls.certfiles <ca_tls_cert_path>
    ```
    {: codeblock}

    Your command might look like the following example:
    ```
    fabric-ca-client affiliation list --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
    ```
    {: codeblock}

    You should see information that is similar to the following example:
    ```
    affiliation: org1
        affiliation: org1.department1
    ```
    {: codeblock}

    Make a note of the second **affiliation** value, for example, `org1.department1`. You need to use this value in the command below.

    If you created the CA with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric APIs instead of the console, your CA is deployed without affiliations, unless you created affiliations by using the Fabric CA Client. If your CA does not have affiliations, you can skip this step and leave the `--id.affiliation` off future commands.

3. Run the following command to register the ordering node or peer.

    ```
    fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type <component_type> --id.secret <secret> --tls.certfiles <ca_tls_cert_path>
    ```
    {: codeblock}

    Create a name and password for the component and then use them to replace `name` and `secret`.  Make a note of this information. Set the `--id.type` to `orderer` if you are deploying an ordering node, or set it to `peer` if you are deploying a peer. The command might look similar to the following example:

    ```
    fabric-ca-client register --caname ca --id.affiliation org1.department1 --id.name peer1 --id.secret peer1pw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
    ```
    {: codeblock}

    You need to save the `"enrollid"` and `"enrollsecret"` from the command above for when you create your configuration file.
    {: important}

    You can register an identity only once. If you experience a problem, try a command with a new username and password. As a security measure, use each identity, and the accompanying enrollID and secret, to deploy only one peer. While you can use one **admin** identity for several components (this is the recommended deployment strategy), do not reuse peer IDs and passwords.

    When the command completes successfully, you should see information that is similar to the following example:

    ```
    2018/06/18 16:53:00 [INFO] Configuration file location: /fabric-ca-platform/admin/fabric-ca-client-config.yaml
    2018/06/18 16:53:00 [INFO] TLS Enabled
    2018/06/18 16:53:00 [INFO] TLS Enabled
    Password: peerpw
    ```
    {: codeblock}

### Registering your organization administrator
{: #ibm-hlfsupport-v2-apis-config-register-admin}

You also need to create an admin identity that you can use to operate your network. You use this identity to operate specific components, such as by installing a smart contract on your peer. You can also use this identity as an administrator of your organization and use it to create and edit channels.  

You need to register this new identity with your CA, and use it to generate an MSP folder. You can make this identity an organization administrator by adding its signCert to your organization MSP. You also need to add the signCert to your configuration file so that it can be made the admin cert of the ordering node or peer during deployment. You need to create only one admin identity for your organization. As a result, you need to complete these steps only once. You can use the signCert that you generated to deploy many peers or ordering nodes.

Ensure that your `$FABRIC_CA_CLIENT_HOME` is set to the path to the MSP of your CA Admin.

```
echo $FABRIC_CA_CLIENT_HOME
$HOME/fabric-ca-client/ca-admin
```
{: codeblock}

Register the component admin identity with the CA by running the following command:

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type admin --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{: codeblock}

Create a new user identity `name` and `secret` for the admin. Make sure to use different values than for the peer or ordering node identity that you registered. The command looks similar to the following example:

```
fabric-ca-client register --caname ca --id.name peeradmin --id.affiliation org1.department1 --id.type admin --id.secret peeradminpw --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{: codeblock}

Make a note of this information. You will use this `name` and `secret` to generate the MSP folder by using the `enroll` command.
{: important}

### Generating the admin Membership Service Provider (MSP) folder
{: #ibm-hlfsupport-v2-apis-config-enroll-admin}

After your register the component admin, you can generate certificates by running the following command:

```
fabric-ca-client enroll -u https://<peer_admin_enroll_id>:<peer_admin_enroll_password>@<ca_url_with_port> --caname <ca_name> -M $HOME/path/to/peer-admin/msp --tls.certfiles <ca_tls_cert_path>
```
{: codeblock}

For example:

```
fabric-ca-client enroll -u https://peeradmin:peeradminpw@9.30.94.174:30167 --caname ca -M $HOME/fabric-ca-client/peer-admin/msp --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{: codeblock}

After this command completes successfully, it generates a new MSP folder in the directory that you specified by using the `-M` flag. This directory contains the certificates that you need to use to operate your components. You can verify that the enroll command worked by using a tree command. Navigate to the directory where you stored your certificates. A tree command should generate a result similar to the following structure:


```
cd $HOME/fabric-ca-client
tree
.
├── ca-admin
│   ├── fabric-ca-client-config.yaml
│   └── msp
│       ├── IssuerPublicKey
│       ├── IssuerRevocationPublicKey
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── 425947b767936a8f31780390cbc399cae48663b643e906ceb6944500d57b322c_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── catls
│   └── tls.pem
├── orderer-admin
│   └── msp
│       ├── IssuerPublicKey
│       ├── IssuerRevocationPublicKey
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── 30a12af2359cd96ec02ff5f47ad7793d2fefe3cde2be574bace18b24d36ba015_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
└── peer-admin
    └── msp
        ├── IssuerPublicKey
        ├── IssuerRevocationPublicKey
        ├── cacerts
        │   └── 9-30-250-70-30395-SampleOrgCA.pem
        ├── keystore
        │   └── c9c20a57a3afa70541bee6cec52ea1f654a8b116fcca35e1fbf27e60f1f989ec_sk
        ├── signcerts
        │   └── cert.pem
        └── user
```
{: codeblock}

You will need to return to this folder when you create your organization MSP definition and configuration files.
{: important}

### Registering the component identity with the TLS CA
{: #ibm-hlfsupport-v2-apis-config-register-component-tls}

When you created your CA, a TLS CA was deployed along with your default CA. You also need to register the ordering node or peer with your TLS CA. To do this, you will first need to enroll by using the admin of the TLS CA. Change `$FABRIC_CA_CLIENT_HOME` to a directory where you want to store your TLS CA admin certificates.

```
cd $HOME/fabric-ca-client
mkdir tlsca-admin
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/tlsca-admin
```
{: codeblock}

Run the command below to enroll as your admin against the TLS CA. The enroll ID and password of your TLS CA admin are the same as your default CA. As a result, the command below is the same as you used to enroll as your [CA admin](#ibm-hlfsupport-v2-apis-enroll-ca-admin) only with the name of your TLS CA. Your TLS CA name is the **TLS CA Name** value from the CA **settings** panel in your console, or the value of the `"tlsca_name"` returned by the `Create a CA` API.

```
fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <tls_ca_name> --tls.certfiles <ca_tls_cert_path>
```
{: codeblock}

A real call might look similar to the following example:

```
fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname tlsca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{: codeblock}

After you enroll, you have the necessary certificates to register your component with the TLS CA. Run the following command to register the ordering node or peer:

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type peer --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{: codeblock}

This command is similar to the one that you used to register the component identity with the CA, except that you need to use the TLS CA name. If you are deploying an ordering node instead of a peer, set `--id.type` to `orderer` instead of `peer`. You must provide this identity a different username and password than the one that you used against your default CA. A real register might look similar to the command below:

```
fabric-ca-client register --caname tlsca --id.affiliation org1.department1 --id.name peertls --id.secret peertlspw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{: codeblock}

You need to save the `"enrollid"` and `"enrollsecret"` from the command above for when you create your configuration file.
{: important}

### Troubleshooting
{: #ibm-hlfsupport-v2-apis-config-troubleshooting}

#### **Problem:** Error when running the `enroll` command
{: #ibm-hlfsupport-v2-apis-config-enroll-error1}

When running the Fabric CA client enroll command, it is possible the command fails with the following error:

```
Error: Failed to read config file at '/Users/chandra/fabric-ca-client/ca-admin/fabric-ca-client-config.yaml': While parsing config: yaml: line 42: mapping values are not allowed in this context
```
{: codeblock}  

**Solution:**

This error can occur when your Fabric CA client tries to enroll but cannot connect to your CA. This can happen if:   

- Your `enroll` command contains an extra `https://` on the `-u` parameter.
- Your CA name is incorrect.
- Your username or password is incorrect.

Review the parameters that you specified on your `enroll` command and ensure that none of these conditions exist.

#### **Problem:** Error with CA URL when running the `enroll` command
{: #ibm-hlfsupport-v2-apis-config-enroll-error2}

The Fabric CA client enroll command can fail if the enrollment URL, the `-u` parameter value, contains a special character. For example, the following command with the enroll ID and password of `admin:C25A06287!0`,

```
./fabric-ca-client enroll -u https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241 --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname PeerOrg1CA
```
{: codeblock}

will fail and produce the following error:

```
!pw@9.12.19.115: event not found
```  
{: codeblock}

**Solution:**
{: #ibm-hlfsupport-v2-apis-config-enroll-error2-solution}

You need to either encode the special character or surround the URL with the single quotation marks. For example, `!` becomes `%21`, or the command looks like:

```
./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname PeerOrg1CA
```
{: codeblock}

## Creating an organization MSP definition
{: #ibm-hlfsupport-v2-apis-msp}

You can use the APIs to create an organization MSP definition by calling [`POST /ak/api/v2/components/msp`](/apidocs/blockchain#import-an-msp). This MSP contains certificates that define your organization in a blockchain consortium, as well as the admin certificates that you can use to operate your network. If you followed the step above, you have already generated the certificates that are needed to create an organization MSP. Use the following steps to complete the request body of the API call.

1. Select an MSP ID for your organization. The MSP ID is the formal name of your organization within the consortium. The MSP ID used to create the organization MSP needs to be the same that you use to deploy your peers.

2. Select a display name for your MSP.

3. You need to provide the signCert, in base64 format, of your organization administrator that you registered and enrolled by using the Fabric CA client.  

    Navigate to the MSP directory that was created when you [generated certificates by using your organization administrator](#ibm-hlfsupport-v2-apis-config-enroll-admin).
    ```
    cd $HOME/fabric-ca-client/peer-admin/msp
    ```
    {: codeblock}

    In this MSP directory, open the signCert file of the new user and convert it to base64 format by using the following commands:

    ```
    export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
    cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
    ```
    {: codeblock}

    **Note:** It is important that the string generated by using the command above is formatted as a single line. It should look similar to:

    ```
    LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
    ```
    Not like this:

    ```
    LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
    ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
    Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
    VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
    WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
    ```
    {: codeblock}

    For example:

    ```
    export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
    cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
    ```
    {: codeblock}

    This command prints a string that is similar to the following example:

    ```
    LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    ```
    {: codeblock}

    Provide this string to the `admins` field of the API request.

4. You also need to provide the root certificate of your CA. This certificate was created when you [generated certificates by using your CA admin](#ibm-hlfsupport-v2-apis-enroll-ca-admin).

    Navigate to the CA admin MSP directory.
    ```
    cd $HOME/fabric-ca-client/ca-admin/msp
    ```
    {: codeblock}

    In this directory, open the cacerts folder and convert the certificate inside into base64 format by using the following commands:
    ```
    export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
    cat $HOME/<path-to-ca-admin>/msp/cacerts/<ca_root_cert>.pem | base64 $FLAG
    ```
    {: codeblock}

    This prints the root cert as a base64 encoded sting.

    ```
    LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWIyZ0F3SUJBZ0lVQmZnZzcvVnIrL25OVEFNQlQ4UUtHL00wQU8wd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU1TURVd016RXpNamt3TUZvWERUTTBNRFF5T1RFek1qa3dNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUVXMUtvN2lWeVE2VWkwdDVqbU5KaWVuSUwKR3pNM1BDWHlhL2VSQ0NWMmFQb0dTZ1lrVUg2UWN5RjAzbFlMZFU4Y0drNTQ0alViVC9KT1lYeVgzTWc4bHFORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZDK2lJR0NSb2Zvb3FsVkZoU3dOMmk2MXNJaVBNQW9HQ0NxR1NNNDlCQU1DQTBnQU1FVUNJUURTYW9RL1E0QzkKbFl1VGNhVXVHb3d6YmhUZHBuN2F3S2lHN1Nvd2lSQXVld0lnUWlyM3RNR3IvYWo2aU5lRXJFN2NyOVowQ0gvTwp3QnNQcWd4RVR3MjVqZUU9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    ```
    {: codeblock}

    Provide this string to the `root_certs` field of the API request.

5. You should also provide the root certificate of your TLS CA. The TLS root certificate allows your peers to participate in [gossip](#x9829550){: term} on a channel.

    Navigate to the MSP directory that was generated when you [enrolled your TLS CA admin](#ibm-hlfsupport-v2-apis-config-register-component-tls).
    ```
    cd $HOME/fabric-ca-client/tlsca-admin/msp
    ```
    {: codeblock}

    In this directory, open the cacerts folder and convert the certificate inside into base64 format by using the following commands:
    ```
    export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
    cat $HOME/<path-to-tlsca-admin>/msp/cacerts/<tls_root_cert>.pem | base64 $FLAG
    ```
    {: codeblock}

    The command prints the TLS CA root cert as a base64 encoded sting.

    ```
    LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNHRENDQWI2Z0F3SUJBZ0lVWUVQWnprNXV2b3dobEtacG5JMXplODdIQUlnd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEExTURNeE16STVNREJhRncwek5EQTBNamt4TXpJNU1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFRdSs2UnZWd2w5T2dDVlAraEVxbjVxdExRVG9LWkw4a1lic0pOeU1JbERoc3hlNWx6cW1zQkoKbTk2eUR2TVV6OSsxL2pzb1M4M1JqMVAwc3M2TnJNb3FvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVVnUEc4anJEK1BxVjdoelc3WDlsbTFrMS91WjR3CkZRWURWUjBSQkE0d0RJY0VxVGJESW9jRUNwbnBkVEFLQmdncWhrak9QUVFEQWdOSUFEQkZBaUVBenk3cHJZaVMKQmlDVWdYeWRkY09WMm9mZmtqaEI0N091QXFjQWNqZS9SWkVDSUdKZFgzZ1ErTDRIN3duY1RoZkwrenU1ejV1UApGUWhXTmlNS3hQWEYrZnYwCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    ```
    {: codeblock}

    Provide this string to the `tls_root_certs` field of the API request.

## Creating a configuration file
{: #ibm-hlfsupport-v2-apis-config}

You need to complete a configuration file in order to create a peer or ordering node by using the APIs. This file is provided to the API as the `config` object in the request body of the API call. If you are creating multiple ordering nodes, you need to provide a configuration file for each node that you want to create in an array to the API request. For example, for a five node raft ordering service, you need to create an array of five configuration files. You can provide the same file for each node as long as the enrollID's that you provide have a sufficiently high enrollment limit. You need to deploy a CA and follow the steps to register and enroll the required identities before you complete the file.

The template for the configuration file can be found below:
```
{
    "enrollment": {
        "component": {
            "cahost": "",
            "caport": "",
            "caname": "",
            "catls": {
                "cacert": ""
            },
            "enrollid": "",
            "enrollsecret": "",
            "admincerts": [""]
        },
        "tls": {
            "cahost": "",
            "caport": "",
            "caname": "",
            "catls": {
                "cacert": ""
            },
            "enrollid": "",
            "enrollsecret": "",
            "csr": {
                "hosts": [""]
            }
        }
    }
}
```
{: codeblock}

Copy this entire file into a text editor where you can edit it and save it to your local file system as a JSON file. Use the steps below to complete this configuration file and use it to deploy an ordering service or peer.

### Retrieve the CA connection information
{: #ibm-hlfsupport-v2-apis-config-connx-info}

First, we need to provide the connection information of your CA on the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric. You can use the console UI or the APIs to get the necessary information about your CA.

**If you are using the Fabric Operations Console:**
Open the CA in your console and click **Settings**, then the **Export** button to export the CA information to a JSON file. You can use the values from this file to complete your configuration file.

**If your are using the APIs:**
You can call [`GET /ak/api/v2/components`](/apidocs/blockchain#get-all-components) to get the connection information of your CA. If you created the CA with the `Create a Fabric CA` API, you can also find the necessary information in the response body.

- The `"cahost"` and `"caport"` values are visible in the `ca_url` field in the response body or CA JSON file that you exported.  For example, if your `ca_url` is `https://9.30.94.174:30167`, the value of the `"cahost"` would be `9.30.94.174` and the `"caport"` would be `30167`.
- The `"caname"` is the name of the CA that was specified when you deployed the CA. This is the value of the `ca_name` field in the response body or the exported JSON file.
- The `"cacert"` is the base64-encoded TLS certificate of your CA. This is the value of the `pem` field in the response body or the exported JSON file.

- The fields in the `"tls"` section below require the same information as you passed to the component sections above, except you need to use the value of the CA TLS instance name that is specified during CA deployment. Replace `caname` with the value of `tlsca_name` in the response body or the exported JSON file. Use the same TLS cert for the `"cacert"` value.
    ```
    "tls": {
        "cahost": "",
        "caport": "",
        "caname": "",
        "catls": {
            "cacert": ""
    ```
    {: codeblock}

### Provide your component enroll ID and secret

1. Paste the `name` and `secret` you used to [register your component with your default CA](#ibm-hlfsupport-v2-apis-config-register-component) as the `"enrollid"` and `"enrollsecret"` in the configuration file, under the `"component"` section:

    ```
    "component": {...
        },
    "enrollid": "peer1",
    "enrollsecret": "peer1pw",
    ```
    {: codeblock}

2. Paste the `name` and `secret` you used to [register your component with your tls CA](#ibm-hlfsupport-v2-apis-config-register-component-tls) as the `"enrollid"` and `"enrollsecret"` in the configuration file, under the `"tls"` section:

    ```
    "tls": {...
        },
        "enrollid": "peertls",
        "enrollsecret": "peertlspw",
    ```
    {: codeblock}

### Provide the signCert of your organization administrator

Navigate to the MSP directory that was created when your [generated certificates using your organization administrator](#ibm-hlfsupport-v2-apis-config-enroll-admin).
```
cd $HOME/fabric-ca-client/peer-admin/msp
```
{: codeblock}

In this MSP directory, open the signCert file of the new user and convert it to base64 format by using the following commands:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
```
{: codeblock}

**Note:** It is important that the string generated by using the command above is formatted as a single line. It should look similar to:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
```
{: codeblock}

Not like this:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
```
{: codeblock}

For example:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
```
{: codeblock}

This command prints a string that is similar to the following example:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
```
{: codeblock}

Copy this string to the `"admincerts"` field under the component section in the configuration file.

### CSR (Certificate Signing Request) hosts
{: #csr}

You have the option of providing a custom domain to your component by using the `"csr"` section of the components file.

```
"csr": {
    "hosts": [""]
    }
```
{: codeblock}

This section is provided for advanced users to specify a custom hostname that can be used to communicate with the peer. Most users can leave this section blank.

### Completing the configuration file
{: #ibm-hlfsupport-v2-apis-config-file}

After you completed all the steps above, your updated configuration file might look similar to the following example:


```
{
    "enrollment": {
        "component": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "ca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJ0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peer1",
            "enrollsecret": "peer1pw",
            "admincerts": [
                "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMzRENDQW9PZ0F3SUJBZ0lVS2Vib0drZEJqenM5bllRbkVQTWp0VG56YTVrd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE9URTNNRE13TUZvWERURTVNVEV4T1RFM01EZ3dNRm93ZmpFTE1BaKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRXVNQXNHQTFVRUN4TUVkWE5sY2pBTEJnTlZCQXNUQkc5eVp6RXdFZ1lEVlFRTEV3dGtaWEJoCmNuUnRaVzUwTVRFUU1BNEdBMVVFQXhNSFlXUnRhVzR4Y0RCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUgKQTBJQUJLbjUwdEU5TmpZb0RFNDBqalh6RUJ2T2c3Y3RtOElRd0laMnRkS29iNEwwVVhKdSs1Tkt5S2dyUk9vbApWcjBmQmg5cWZWMjl4Nms0T2dmMFNiVklBZWlqZ2ZRd2dmRXdEZ1lEVlIwUEFRSC9CQVFEQWdlQU1Bd0dBMVVkCkV3RUIvd1FDTUFBd0hRWURWUjBPQkJZRUZOYWFkV0VzcGp2Smk1akpiVktiS2M3ZU1wUmhNQjhHQTFVZEl3UVkKTUJhQUZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQ2NHQTFVZEVRUWdNQjZDSEdOb1lXNWtjbUZ6TFcxaQpjQzV5WVd4bGFXZG9MbWxpYlM1amIyMHdhQVlJS2dNRUJRWUhDQUVFWEhzaVlYUjBjbk1pT25zaWFHWXVRV1ptCmFXeHBZWFJwYjI0aU9pSnZjbWN4TG1SbGNHRnlkRzFsYm5ReElpd2lhR1l1Ulc1eWIyeHNiV1Z1ZEVsRUlqb2kKWVdSdGFXNHhjQ0lzSW1obUxsUjVjR1VpT2lKMWMyVnlJbjE5TUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSURGeApDYzE1bDZUZ1dqYnhSZzlmNjczOGV0K0NZZ1I3VVpGR200VHdoQk5jQWlBNmtUMFFwbDV6WnBUN3BzM0dySXlVCmEydDRHSTQ5ZTdjUm5PMmdrSzl6Z3c9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg=="
            ]
        },
        "tls": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "tlsca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peertls",
            "enrollsecret": "peertlspw",
            "csr": {
                "hosts": [
                ]
            }
        }
    }
}
```
{: codeblock}

You can leave the other fields blank. After you complete this file, you can pass this file as the `config` field to the request body of the `Create an ordering service` or `Create a peer` API.

### Importing an admin identity into the Fabric Operations Console
{: #ibm-hlfsupport-v2-apis-admin-console}

If you want to use the Fabric Operations Console to operate your blockchain components, you must import your administrator identity into your console wallet. Open the wallet panel in your console. Click the **Add identity** button on the overview screen. Clicking this button opens up a side panel that allows you to add an identity's signing certificate and private key directly to the console.
- **Name:** Enter a display name for the identity that is used for your reference only.
- **Certificate:** Upload your admin's signing certificate. If you followed the instructions above, you can find this key in the `$HOME/fabric-ca-client/peer-admin/msp/signcerts/` folder.
- **Private Key:** Upload your admins private key. If you followed the instructions above, you can find this key in the `$HOME/fabric-ca-client/peer-admin/msp/keystore/` folder.

After you import your admin identity, you can associate this identity with the components that you create. You can then use the console to operate your network.
