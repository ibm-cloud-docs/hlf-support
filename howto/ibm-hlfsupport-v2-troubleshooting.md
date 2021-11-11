---

copyright:
  years: 2021
lastupdated: "2021-11-11"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

subcollection: hlf-support
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Troubleshooting
{: #ibm-hlfsupport-v2-troubleshooting}


General problems can occur when you use the console to manage nodes, channels, or smart contracts. In many cases, you can recover from these problems by following a few simple steps.
{: shortdesc}

This topic describes common issues that can occur when you use the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console.  

**Issues during Deployment**
- [My deployment fails when I to try to apply the security and access policies to my namespace](#ibm-hlfsupport-v2-troubleshooting-deployment-policies)
- [My deployment fails when I try to apply the custom resource definition of the console or operator](#ibm-hlfsupport-v2-troubleshooting-deployment-cr)
- [Extracting the TLS certificate from the Kubernetes webhook fails](#ibm-hlfsupport-v2-troubleshooting-wh-extract)

**Issues with the Console**
- [Why is my cluster deployed from the Red Hat Marketplace `fail to pull image cp.icr.io/ibm-hlfsupport-init@sha256` during setup?](#ibm-hlfsupport-v2-troubleshooting-pull-image-fails)
- [Why am I not able to log in to the console from my Chrome browser on Mac OS Catalina?](#ibm-hlfsupport-v2-troubleshooting-console-catalina)
- [Why is my channel fail to create or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?](#ibm-hlfsupport-v2-troubleshooting-accept-tls)
- [When I hover over my node, the status is `Status unavailable`, what does this mean?](#ibm-hlfsupport-v2-troubleshooting-status-unavailable)
- [When I hover over my node, the status is `Status undetectable`, what does this mean?](#ibm-hlfsupport-v2-troubleshooting-status-undetectable)
- [When I hover over my node, the status is red, what does this mean?](#ibm-hlfsupport-troubleshooting-status-error)
- [Why did my smart contract installation, instantiation or upgrade fail?](#ibm-hlfsupport-console-smart-contracts-troubleshoot-entry1)
- [Why is my smart contract fail to install with an error on my peer?](#ibm-hlfsupport-v2-troubleshooting-sc-install)
- [Why is my Node.js smart contract fail to instantiate?](#ibm-hlfsupport-v2-troubleshooting-nodejs-instantiate)
- [Why is my Node.js smart contract fail to endorse?](#ibm-hlfsupport-v2-troubleshooting-nodejs-endorsement)
- [Why is the smart contract that I installed on the peer not listed in the UI?](#ibm-hlfsupport-console-build-network-troubleshoot-missing-sc)
- [My channel, smart contracts, and identities have disappeared from the console. How can I get them back?](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-troubleshooting#ibm-hlfsupport-v2-troubleshooting-browser-storage)
- [Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?](#ibm-hlfsupport-v2-troubleshooting-create-msp)
- [Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?](#ibm-hlfsupport-v2-troubleshooting-update-channel)
- [When I log in to my console, why am I getting a 401 unauthorized error?](#ibm-hlfsupport-v2-troubleshooting-console-401)
- [Why am I getting an error “all SubConns are in TransientFailure” on the console?](#ibm-hlfsupport-console-transientfailure)

**Issues with your Nodes**  

- [Why is my first start of a smart contract returns the following error: no suitable peers available to initialize from?](#ibm-hlfsupport-v2-troubleshooting-smart-contract-anchor-peers)
- [Why are my node operations fail to operate after I create my peer or ordering service?](#ibm-hlfsupport-console-build-network-troubleshoot-entry1)
- [Why does my peer or ordering node fail to start?](#ibm-hlfsupport-console-build-network-troubleshoot-entry2)
- [What is the proper way to clean up a failed node deployment?](#ibm-hlfsupport-v2-troubleshooting-cleanup)
- [How can I view my smart contract container logs?](#ibm-hlfsupport-console-smart-contracts-troubleshoot-entry2)
- [Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?](#ibm-hlfsupport-v2-troubleshooting-endorsement-sig-failure)
- [Why are the transactions I submit from VS Code failing with a No endorsement plan available error?](#ibm-hlfsupport-v2-troubleshooting-anchor-peer)
- [Why are the transactions I submit from VS Code failing with an endorsement failure?](#ibm-hlfsupport-v2-troubleshooting-endorsement)
- [Why is my peer unable to communicate with my ordering node?](#ibm-hlfsupport-troubleshooting-peer-bad-connection)
- [How do I disable the network policies in my namespace?](#ibm-hlfsupport-troubleshooting-disable-network-policies)


## My deployment fails when I try to apply the security and access policies to my namespace
{: #ibm-hlfsupport-v2-troubleshooting-deployment-policies}
{: troubleshoot}

When I try to deploy {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 and apply the Security Context Constraint, ClusterRole, or ClusterRoleBinding to my namespace, I encounter one of the following errors.

When I apply the file, I receive a user forbidden error:
{: tsSymptoms}

```
securitycontextconstraints.security.openshift.io "e-block2" is forbidden: User cannot get securitycontextconstraints.security.openshift.io at the cluster scope: no RBAC policy matched
```

This problem occurs when you are not a cluster administrator. You must have a cluster administrator role and apply the security and access policies to your namespace.
{: tsCauses}

When I try to apply resource file, I receive a parsing error:
{: tsSymptoms}

```
error: error parsing ibm-hlfsupport-scc.yaml: error converting YAML to JSON: yaml: line 10: mapping values are not allowed in this context
```

This error happens when a problem exists with the indents in your YAML file. Refer to the documentation for the correct format for the security and access files.
{: tsCauses}


## My deployment fails when I try to apply the custom resource definition of the console or operator
{: #ibm-hlfsupport-v2-troubleshooting-deployment-cr}
{: troubleshoot}

When I try to deploy {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 and apply the custom resource definition of the operator or the console, I encounter one of the following errors:

When I apply the custom resource file, I receive an image pull or image pull back-off error:
{: tsSymptoms}

```
NAME                            READY     STATUS             RESTARTS   AGE
ibm-hlfsupport-operator-769d94ffbc-w52n6   0/1       ImagePullBackOff   0          32s
```

This problem occurs when your deployment cannot pull the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images from the {{site.data.keyword.IBM_notm}} Entitlement registry. This can happen because you provided an incorrect name of your secret to the `imagePullSecrets:` field, or if there was a problem with the URL or the key that you provided to the entitlement key secret. This error can also occur if you supplied the wrong image tag to the file.
{: tsCauses}

When I try to apply the custom resource file, I receive a parsing error:
{: tsSymptoms}

```
error: error parsing console.yaml: error converting YAML to JSON: yaml: line 11: mapping values are not allowed in this context
```

This error happens when a problem exists with the indents in your file. Refer to the documentation for the correct format for the custom resource files of the console and operator.
{: tsCauses}

## Extracting the TLS certificate from the Kubernetes webhook fails
{: #ibm-hlfsupport-v2-troubleshooting-wh-extract}
{: troubleshoot}

When deploying or upgrading to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, after I deploy the webhook, I am unable to successfully extract the TLS certificate that was generated by the webhook deployment. The following command fails:
```
kubectl get secret webhook-tls-cert -n ibm-hlfsupport-infra -o json | jq -r .data.\"cert.pem\"
```
{: tsSymptoms}

This problem can be occur when the entitlement `key`, that you specified in the `cp-pull-secret` for the `ibm-hlfsupport-infra` namespace or project, is invalid. Because only one entitlement key can be used per deployment, you need to refresh the key if it was already used for a different deployment.
{: tsCauses}

To resolve this problem, repeat the steps you followed to [Get your entitlement key](/docs/hlf-support?topic=hlf-support-deploy-ocp#deploy-ocp-entitlement-key) and then delete the kubernetes `cp-pull-secret` in the `ibm-hlfsupport-infra` namespace or project and recreate it using the refreshed key value. Then rerun the command to extract the TLS certificate that was generated by the webhook deployment.
{: tsResolve}


## Why is my cluster deployed from the Red Hat Marketplace `fail to pull image cp.icr.io/ibm-hlfsupport-init@sha256` during setup?
{: #ibm-hlfsupport-v2-troubleshooting-pull-image-fails}
{: troubleshoot}

I am deploying my trial cluster from the Red Hat Marketplace but I am receiving a `fail to pull image cp.icr.io/ibm-hlfsupport-init@sha256` error message.
{: tsSymptoms}

To resolve this problem, you need to get the pull secret from the Red Hat Marketplace to update your global cluster pull secret. See [Get pull secret](https://marketplace.redhat.com/en-us/documentation/clusters#get-pull-secret) for the setup procedures.
{: tsResolve}


## Why am I not able to log in to the console from my Chrome browser on Mac OS Catalina?
{: #ibm-hlfsupport-v2-troubleshooting-console-catalina}
{: troubleshoot}

The console has been working successfully, but after I upgraded my Mac OS to Catalina, I can no longer login to the console.
{: tsSymptoms}

There are three ways to resolve this problem:
{: tsResolve}
1. Use a different [supported browser](/docs/hlf-support?topic=hlf-support-deploy-ocp-getting-started#deploy-ocp-browsers) with Catalina.
2. Use your own [TLS certificates when deploying on OpenShift Contain Platform](/docs/hlf-support?topic=hlf-support-deploy-ocp#console-deploy-ocp-use-your-own-tls-certificates-optional) or [TLS certificates when deploying on Kubernetes](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-tls).
3. Run the following commands to generate a new key and certificate pair for the console that will fix the problem.
    - Run the following command to get the pod that corresponds to the console:

        ```
        kubectl get po
        ```
        {: codeblock}

    - Exec into the pod by running the command:

        ```
        kubectl get po <pod-name> -c optools bash
        ```
        {: codeblock}

    - Delete the console key and certificate by running the command:

        ```
        rm -f /certs/tls.key rm -f /certs/tls.crt
        ```
        {: codeblock}

    - Delete the console pod which causes it to restart by running the command:

        ```
        kubectl delete po <pod-name>
        ```
        {: codeblock}

    When the pod restart completes, you should now be able to log in to your console URL from a Chrome Browser.

## Why is my channel creation failing or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?
{: #ibm-hlfsupport-v2-troubleshooting-accept-tls}
{: troubleshoot}

If you are not using your own TLS certificates to secure communications in your blockchain network, you need to accept the self-signed certificate that was generated for you. Otherwise, when you try to create a channel or add an organization to an ordering service the action fails. Channel creation fails with the error `An error occurred when creating channel. submit config update failed: grpc code ???= response contains no code or message`. Or, when you click on your ordering service, you see `Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node, you will not be able to view or manage ordering service details`.
{: tsSymptoms}

This problem occurs when the blockchain console is deployed without the advanced deployment option to [use your own TLS certificates](/docs/hlf-support?topic=hlf-support-deploy-ocp#console-deploy-ocp-use-your-own-tls-certificates-optional).
{: tsCauses}

To resolve this problem, you need to accept the self-signed certificate in your browser.
{: tsResolve}

1. Copy your console URL, for example `https://<PROJECT-NAME>-ibpconsole-console.abc.xyz.com:443`, where the value of `abc.xyz.com` depends on your cloud provider.
2. Replace `-console` with `-proxy`. The new URL looks similar to: `https://<PROJECT-NAME>-ibpconsole-proxy.abc.xyz.com:443`.
3. Open a new tab in your browser and paste in the new URL.
4. Accept the certificate.

You need to accept the certificate from this URL to communicate with your nodes from your console and then log in as usual. When you switch to a new machine or a new browser, you need to repeat these steps.


## When I hover over my node, the status is `Status unavailable` or `Status unknown`, what does this mean?
{: #ibm-hlfsupport-v2-troubleshooting-status-unavailable}
{: troubleshoot}

A CA, peer, or ordering node has a gray status box, meaning the status of the node is not available. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This problem can occur if the node is newly created and the deployment process has not completed. If the node is a CA and the status has been gray for more than a few minutes, then it is likely that the deployment process has failed. Peers and ordering nodes take longer to deploy, but this condition can also occur when the health checker that runs against the peer or ordering nodes cannot contact the node. The request for status can fail with a timeout error because the node did not respond within a specific time period, the node could be down, or network connectivity is down.
{: tsCauses}

If this is a new node, wait a few more minutes for the deployment to complete. You can try reloading the page in your browser to refresh the status. If the node is not new, [examine the associated node logs](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-console-logs) for errors to determine the cause.
{: tsResolve}

## When I hover over my node, the status is `Status undetectable`, what does this mean?
{: #ibm-hlfsupport-v2-troubleshooting-status-undetectable}
{: troubleshoot}
{: support}

The node status in the tile for the peer or ordering node is yellow, meaning the status of the node cannot be detected. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This condition can occur on peer and ordering nodes that were *imported* to the console and the health checker cannot run against the node. This status happens because an `operations_url` was not specified when the node was imported. An operations URL is required for the node health checker to run. The node itself is likely `Running`, but because the operations URL was not specified, its status cannot be determined.
{: tsCauses}

**If you imported the node:**
{: tsResolve}

You can resolve this problem by performing the following steps:
1. Click the node tile to open it.
2. Click the **Settings** icon.
3. Click **Associate identity**, view and make note of the identity that is associated with this node. Click **Cancel** to close this panel.
4. Click **Export** to generate a `JSON` file for the node.
5. Edit the generated `JSON` file and enter the `operations_url` for the node. The value of the `operations_url` depends on how it was configured as well as various network settings. This value needs to be provided by the network administrator who deployed the node.
6. Click **Delete**. This step removes the imported node from the console, but does not delete the actual node.
7. From the **Nodes** tab, click **Add Peer** or **Add ordering service** followed by **Import an existing peer** or **Import an existing Ordering service**.
8. Click **Upload JSON** and browse to the JSON file you just edited. Click **Next**.
9. Associate the same identity you noted in step three.
10. Click **Add peer** or **Add ordering service**.
The health checker can now run against the node and report the status of the node.



## Why did my smart contract installation, instantiation or upgrade fail?
{: #ibm-hlfsupport-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}
{: support}

It is possible you may experience an error when installing, instantiating or upgrading a smart contract. For example, when you try to install a smart contract on a peer, it fails with the error `An error occurred when installing smart contract on peer.`
{: tsSymptoms}

You may receive this error if this version of the smart contract already exists on the peer, or if your peer is out of resources.
{: tsCauses}

- Open your Kubernetes dashboard and ensure the peer status is `Running`.
- Open the peer node and ensure the smart contract version does not already exist on the peer and try again with the proper version.
- If you are still experiencing problems after the node is up,  [check your node logs](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-node-logs) for errors.
{: tsResolve}

## Why is my smart contract fail to install with an error on my peer?
{: #ibm-hlfsupport-v2-troubleshooting-sc-install}
{: troubleshoot}
{: support}

Installing a smart contract on a peer fails with an error similar to the following:
{: tsSymptoms}

```
An error occurred when installing smart contract on peer.
error in simulation: failed to execute transaction
421fac...2fda: error sending: timeout expired while executing transaction.
```

When running the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric on s390x architecture, or in an environment with constrained resources, it is possible that smart contract installation can fail on a peer if the default timeout is too short on the peer that is running the Fabric v2x image.
{: tsCauses}

In some cases, if you simply wait several minutes and then refresh the **Smart contracts** tab in the console, you can see that the smart contract was successfully installed. You can customize how long the console waits for the installation to complete by changing the console settings with the [APIs](/apidocs/blockchain#edit-settings). See the `fabric_lc_install_cc_timeout_ms` setting. In some cases, it is the peer itself that is timing out and you need to override the peer configuration to extend its time out.
{: tsResolve}

1. From the console, open the peer tile and click the **Settings** icon.
2. Click **Edit configuration JSON (Advanced)**.
3. In the **Configuration updates** box paste the following text and click **Update peer**.
    ```
    {
    "chaincode":{
        "startuptimeout":"300s",
        "installTimeout":"600s",
        "executetimeout":"30s"
    }
    }
    ```
    {: codeblock}

The peer restarts and then you can retry the smart contract installation. Because the original installation failed you need to specify a new smart contract name and version. 

## Why is my Node.js smart contract fail to instantiate?
{: #ibm-hlfsupport-v2-troubleshooting-nodejs-instantiate}
{: troubleshoot}

Instantiating a Node.js smart contract fails with the timeout error:
{: tsSymptoms}
```
[endorser] SimulateProposal -> ERRO 0ba [channel2][37876c5f] failed to invoke chaincode name:"lscc" , error: timeout expired while starting chaincode myassetc:0.0.1 for transaction
github.com/hyperledger/fabric/core/chaincode.(*RuntimeLauncher).Launch
    /go/src/github.com/hyperledger/fabric/core/chaincode/runtime_launcher.go:75
```

When running the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric on s390x architecture, it is possible that Node.js smart contract instantiation can fail if the default timeout is too short.
{: tsCauses}

Customers should wait for five minutes after the failure occurs and then retry the instantiation again. It will then work successfully on the subsequent attempt.
{: tsResolve}

## Why is my Node.js smart contract fail to endorse?
{: #ibm-hlfsupport-v2-troubleshooting-nodejs-endorsement}
{: troubleshoot}
{: support}

My Node.js smart contract endorsement fails with the error:
{: tsSymptoms}

```
Error: endorsement failure during query. response: status:500 message:"error in simulation: failed to execute transaction 6cbcf9f94bdef3fc68abba5604e46293ae: could not launch chaincode nodecc:1.1: error building chaincode: error building image: external builder failed: external builder failed to build: external builder 'ibm-hlfsupport-builder' failed: exit status 3"
```

By default, a Fabric v2.x peer creates a Node v12 runtime. In order for the smart contract to work with Node 12 runtime, the `fabric-contract-api` and `fabric-shim` node modules must be at v1.4.5 or greater.
{: tsCauses}

If you are using a smart contract that was originally written to work with Fabric 1.4, update the Node modules by running the following command before deploying the smart contract on a Fabric v2.x peer.  See [Support and compatibility for fabric-chaincode-node](https://github.com/hyperledger/fabric-chaincode-node/blob/main/COMPATIBILITY.md) for more information.
{: tsResolve}

```
npm install --save fabric-contract-api@latest-1.4 fabric-shim@latest-1.4
```
{: codeblock}


## Why is the smart contract that I installed on the peer not listed in the UI?
{: #ibm-hlfsupport-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

A smart contract was installed on a peer but when you click on the **Smart contracts** tab, it is not listed.
{: tsSymptoms}

This issue can occur when another user or application installs the smart contract onto the peer and you do not have the peer admin identity in your browser wallet.
{: tsCauses}

In order to view the smart contracts installed on a peer, you need to be a peer admin. Ensure that the peer admin identity exists in your browser wallet. If it does not, you need to import it into your console wallet.
{: tsResolve}

## My nodes, channels, smart contracts, and identities have disappeared from the console. How can I get them back?
{: #ibm-hlfsupport-v2-troubleshooting-browser-storage}
{: troubleshoot}
{: support}

Nodes, identities, channels, or smart contracts (or some combination of these) that I successfully deployed into the console are no longer available.
{: tsSymptoms}

One of the features of {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric is that you are now responsible for securing and managing your certificates. Therefore, they are only persisted in the browser local storage to allow you to manage the component. If you are using a private browser window and then switch to another browser or non-private browser window, the identities that you created will be gone from your wallet in the new browser session. Therefore, it is required that you export the identities from the wallet in your private browser session to your file system. You can then import them into your non-private browser session if they are needed. Otherwise, there is no way to recover them.
{: tsCauses}

- Whenever you create a new organization MSP definition, you generate keys for an identity that is allowed to administer the organization. Therefore, during that process you must click the **Generate** and then **Export** buttons to store the generated identity in your wallet and then save it to your file system as a JSON file.
- To resolve this problem in your browser, you need to import those identities and associate them with the corresponding node:
    - In the browser where you are experiencing the problem, click the **Wallet** tab followed by **Add identity** to import the JSON file into your wallet.
    - Click **Upload JSON** and browse to the JSON file you exported using the **Add files** button.
    - Click **Submit**.
    - Now open the peer or ordering service node that this identity was originally associated to, and click on the **Settings** icon.
    - Click the **Associate identity** button.
    - Select the identity you just imported to your console wallet from the drop-down list.
    - Click **Associate**.
- Repeat this process for each identity that was in the wallet of the original browser.
{: tsResolve}

This problem can also occur when the console has lost contact with your cluster. This can happen if your console has not recently communicated with your cluster, or if you have made changes to your cluster that have overridden the settings used by the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric.
{: tsCauses}

You can renew the communication between your console and the {{site.data.keyword.IBM_notm}} Kubernetes service cluster by clicking on the service instance of your console in your **Resource list** and clicking the **Refresh cluster** button. When the link between your cluster and your console has been refreshed, click the **Launch the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric** button.
{: tsResolve}

## Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?
{: #ibm-hlfsupport-v2-troubleshooting-create-msp}
{: troubleshoot}
{: support}

When you attempt to create a new organization MSP definition from the Organizations tab, you experience the error `Unable to authenticate with the enroll ID and secret you provided`.
{: tsSymptoms}

This error occurs when the value that you specified for the enroll secret is not valid for the enroll ID that you selected in the `Generate organization admin certificate` section of the panel.
{: tsCauses}

Verify that you have selected the correct organization admin enroll ID from enroll ID drop down list and enter the correct value for the enroll secret.
{: tsResolve}

## Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?
{: #ibm-hlfsupport-v2-troubleshooting-update-channel}
{: troubleshoot}

When you attempt to add another organization to a channel, the update fails with the message `An error occurred when updating channel`.
{: tsSymptoms}

This error occurs when the selected **Channel Updater MSP ID** on the **Update channel** panel is not an admin of the channel.
{: tsCauses}

On the **Update channel** panel, scroll down to the **Channel Updater MSP ID** and select the MSP ID that was specified when the channel was created or specify the MSP ID that is the admin of the channel.
{: tsResolve}

## When I log in to my console, why am I getting a 401 Unauthorized error?
{: #ibm-hlfsupport-v2-troubleshooting-console-401}
{: troubleshoot}
{: support}

When I try to log in to my console, I am unable to access the console from my browser. If I check the browser logs, I can find the error 401 unauthorized.
{: tsSymptoms}

Your browser console session times out after **8 hours** of inactivity. If a session becomes inactive, the console will prevent the inactive user from performing any actions.
{: tsCauses}

If your session has become inactive, you can try simply refreshing your browser. If that does not work, close the browser including **all** tabs and windows. Open the URL again. You will be required to log in.

As a best practice, you should have already stored your certificates and identities on your file system. If you happen to be using an incognito window, all the certificates are deleted from the browser local storage when you close the browser. After you log in again you will need to re-import your identities and certificates.
{: note}


## Why am I getting an error “all SubConns are in TransientFailure” on the console?
{: #ibm-hlfsupport-console-transientfailure}
{: troubleshoot}

The following error is shown on the console: "All SubConns are in TransientFailutre."
{: tsSymptoms}

An Out of Memory (OOM) situation can cause this error.
{: tsCauses}

To resolve this problem, you need to resize the peers and CouchDB containers to add more memory, such as 2000 MB memory each. After resizing the memory, [delete the peer pods](#ibm-hlfsupport-troubleshooting-delete-peer) so they will be re-created. Then try the scenario again. See [Considerations when reallocating resources](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern-components#ibm-hlfsupport-console-govern-components-reallocate-resources) for more information.
{: tsResolve}

## Why is my first invoke of a smart contract returns the following error: no suitable peers available to initialize from?
{: #ibm-hlfsupport-v2-troubleshooting-smart-contract-anchor-peers}
{: troubleshoot}
{: support}

When I try to invoke a smart contract from the Fabric SDK, the transaction fails and returns the following error:
{: tsSymptoms}

```
error: [Network]: _initializeInternalChannel: no suitable peers available to initialize from Failed to submit transaction: Error: no suitable peers available to initialize from
```

This error occurs if you have not configured an anchor peer on your channel. Unless you have manually updated your connection profile, your application needs to use the [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-2.2/discovery-overview.html){: external} feature to learn about the peers it needs to submit the transaction to.
{: tsCauses}

Use the following steps to [configure anchor peers on your channel](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern#ibm-hlfsupport-console-govern-channels-anchor-peers).
Also, you should verify that you are submitting the transactions against the correct channel and organization MSP id.
{: tsResolve}

## Why are my node operations fail to operate after I create my peer or ordering service?
{: #ibm-hlfsupport-console-build-network-troubleshoot-entry1}
{: troubleshoot}

It is possible you may experience an error when managing an existing node. When that occurs, it is often useful to consult the node logs.

For example, when you try to operate the node, the action might fail.
{: tsSymptoms}

After creating a new peer or ordering service, depending on your cluster storage configuration, it may take a few minutes for the nodes to be ready for operation.
{: tsCauses}




Check your Kubernetes dashboard and ensure the peer or node status is `Running`. Then try your action again. If you are still experiencing problems after the node is up, [check your node logs](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-node-logs) for errors.
{: tsResolve}


## Why does my peer or ordering node fail to start?
{: #ibm-hlfsupport-console-build-network-troubleshoot-entry2}
{: troubleshoot}

It is possible to experience this error under a variety of conditions.

The peer log includes:
{: tsSymptoms}
```
[main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
```
or the ordering node log contains:

```
Failed to initialize local MSP: admin 0 is invalid [The identity does not contain OU [CLIENT], MSP: [orderermsp],The identity does not contain OU [ADMIN], MSP: [orderermsp]]
```

- This error can occur under the following conditions:
    - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that corresponds to an identity of type `peer` and not `client` or `admin`. It must be of type `client` or `admin`.
    - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that does not match the enroll ID or secret of the corresponding organization admin identity.
    - When you created the peer or ordering service, you specified the enroll ID and secret of an identity that is not type 'peer' or 'orderer'.
    - When you created the peer or ordering service, you associated an identity that does not have the `admin` role.

{: tsResolve}
- Open your peer or ordering service CA node and view the registered identities listed in the **Registered Users** table.
- Delete the peer or ordering service and recreate it, being careful to specify the correct enroll ID and secret of a user that has the `peer` or `orderer` role and associate an identity that has a role of `admin` with the node.
- Note that before you create the peer or ordering service, you need to register an organization admin user, with a type `admin`. Be sure to specify that same id as the enroll ID when you create the organization MSP definition. See these instructions for [registering peer identities](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network-use-CA-org1) and these instructions for [registering orderer identities](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network-use-CA-orderer).

## What is the proper way to clean up a failed node deployment?
{: #ibm-hlfsupport-v2-troubleshooting-cleanup}
{: troubleshoot}

Sometimes a node can fail to deploy, for example, due to lack of resources in your Kubernetes cluster. After you understand the cause of the node deployment failure, you need to clean up the failed node in your cluster.
{: tsSymptoms}

Do not attempt to use Kubernetes commands to remove the node. Instead, it is extremely important that you use the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console or the APIs to remove the failed node to ensure that the associated metadata and storage are also cleaned up.
{: tsResolve}

## How can I view my smart contract container logs?
{: #ibm-hlfsupport-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

You may need to view your smart contract, or chaincode, container logs to debug a smart contract issue.
{: tsSymptoms}


Follow these [instructions](/docs/hlf-support?topic=hlf-support-console-icp-manage#console-icp-manage-container-logs) to view your smart contract container logs.
{: tsResolve}


## Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?
{: #ibm-hlfsupport-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

When I invoke a smart contract to submit a transaction, the transaction returns the following endorsement policy failure:
{: tsSymptoms}

```
returned error: VSCC error: endorsement policy failure, err: signature set did not satisfy policy
```

If you have recently joined a channel and installed the smart contract, this error occurs if you have not added your organization to the endorsement policy. Because your organization is not on the list of organizations who can endorse a transaction from the smart contract, the endorsement from your peers is rejected by the channel. If you encounter this problem, you can change the endorsement policy by upgrading the smart contract. For more information, see [Specifying an endorsement policy](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v2#ibm-hlfsupport-console-smart-contracts-v2-install-propose) and [Versioning a smart contract](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v2#ibm-hlfsupport-console-smart-contracts-v2-versioning).
{: tsCauses}

## Why are the transactions I submit from VS Code failing with a No endorsement plan available error?
{: #ibm-hlfsupport-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Transactions submitted from VS Code fail with an error similar to:
{: tsSymptoms}

```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```
{: codeblock}

This error occurs if you are using the Fabric Service Discovery feature but did not configure any anchor peers on your channel.
{: tsCauses}

Follow step three of the [private data topic](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-smart-contracts-v2#ibm-hlfsupport-console-smart-contracts-v2-private-data) in the Deploy a smart contract tutorial to configure your anchor peers.
{: tsResolve}

## Why are the transactions I submit from VS Code failing with an endorsement failure?
{: #ibm-hlfsupport-v2-troubleshooting-endorsement}
{: troubleshoot}

My smart contract endorsement proposals from my peer are failing from my client application with the endorsement error `error: [Channel.js]: Channel:<channel_name> received discovery error:failed constructing descriptor for chaincodes:<name:"chaincode-name">` or `[ERROR] Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"MyAssetContract"}]}`
{: tsSymptoms}

Also in the endorsing peer logs I can see the error:
```
UTC [discovery] chaincodeQuery -> ERRO 23c Failed constructing descriptor for chaincode chaincodes:<name:"chaincode-name">,: cannot satisfy any principal combination
```
{: codeblock}

This error occurs when the peer's enroll id type does not match the smart contract endorsement policy that was configured when the smart contract was instantiated on the channel.
{: tsCauses}

The only way to resolve this error is to delete the peer and create a new one with an enroll id that has the correct type `peer`. You can use the enroll id and secret from an existing user of type `peer` from the peer's CA or register a new user with type `peer`. Follow the instructions in the [Build a network tutorial](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-build-network#ibm-hlfsupport-console-build-network-create-peer-org1) to create a new peer identity with the correct type and peer.
{: tsResolve}

## How do I delete a peer pod?
{: #ibm-hlfsupport-troubleshooting-delete-peer}
{: troubleshoot}

You are looking for the commands to delete a peer.
{: tsSymptoms}

Use the following CLI command to identify and then delete and restart the failing peer:
{: tsResolve}

* List the pods: `kubectl get pods --all-namespaces`
* Delete the pod: `kubectl delete pod -n <NAMESPACE> <PODNAME>`  

Replace `<NAMESPACE>` with the name of your Kubernetes namespace or your OpenShift project.

## How can I recover a contract after a failed upgrade of the smart contract container?
{: #ibm-hlfsupport-troubleshooting-contract-fail}
{: troubleshoot}

All contracts were lost after the procedure to upgrade the smart contract container crashed.
{: tsSymptoms}

[Delete all the peer pods](#ibm-hlfsupport-troubleshooting-delete-peer). This deletion triggers the peer to be created again and restarts the proxy.
{: tsResolve}

## Why is my peer unable to communicate with my ordering node?
{: #ibm-hlfsupport-troubleshooting-peer-bad-connection}
{: troubleshoot}

A peer node has a timeout or connection refused error when trying to communicate with the orderer.
{: tsSymptoms}

This problem can occur due to the network policies applied by the operator, which occurs when the environment variable `IBPOPERATOR_CONSOLE_APPLYNETWORKPOLICY` is set to `"true"`.
{: tsCauses}

[Disable the network policies in your namespace](#ibm-hlfsupport-troubleshooting-disable-network-policies). This should resolve connectivity issues with your nodes.
{: tsResolve}


## When I hover over my node, the status is red, what does this mean?
{: #ibm-hlfsupport-troubleshooting-status-error}
{: troubleshoot}

A CA, peer, or ordering node has a red status box, meaning there may be connectivity issues with the node. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This problem can occur due to the network policies applied by the operator, which occurs when the environment variable `IBPOPERATOR_CONSOLE_APPLYNETWORKPOLICY` is set to `"true"`.
{: tsCauses}

[Disable the network policies in your namespace](#ibm-hlfsupport-troubleshooting-disable-network-policies). This should resolve connectivity issues with your nodes.
{: tsResolve}

## How do I disable the network policies in my namespace?
{: #ibm-hlfsupport-troubleshooting-disable-network-policies}
{: troubleshoot}

You may need to disable the network policies in your namespace to address connectivity issues in your network.
{: tsSymptoms}

Check if network policies have been applied in your namespace by running the following CLI command:
{: tsResolve}

```
kubectl get netpol -n <NAMESPACE>
```
{: codeblock}

If the command returns a list of one or more network policies that you did not apply and/or you want to disable, first disable the operator creation of network policies. Get the operator deployment spec in your namespace and check for environment variable `IBPOPERATOR_CONSOLE_APPLYNETWORKPOLICY`:
```
kubectl get deploy ibm-hlfsupport-operator -n <NAMESPACE>
```
{: codeblock}

If it is present, remove the environment variable `IBPOPERATOR_CONSOLE_APPLYNETWORKPOLICY`:
```
kubectl edit deploy ibm-hlfsupport-operator -n <NAMESPACE>
```
{: codeblock}

Once the operator creation of the network policy has been disabled, delete the network policies from the namespace:
```
kubectl delete netpol -n <NAMESPACE> <NETWORKPOLICYNAME>
```
{: codeblock}


