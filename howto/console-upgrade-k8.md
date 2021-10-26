---

copyright:
  years: 2021
lastupdated: "2021-10-26"

keywords: Kubernetes, {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric console, deploy, resource requirements, storage, parameters

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}




# Upgrading your console and components
{: #upgrade-k8}

You can upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric without disrupting a running network. Because the platform is deployed by using a Kubernetes operator, you can pull the latest {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images from the {{site.data.keyword.IBM_notm}} Entitlement registry without having to reinstall the platform. You can use these instructions to upgrade to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0.
{: shortdesc}

## {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric overview
{: #upgrade-k8-platform-overview}

The table provides an overview of the current and past releases.

| Version | Release date | Image tags | New features |
|----|----|----|----|
| [{{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0](/docs/hlf-support?topic=hlf-support-whats-new#whats-new-03-29-2021) | 15 Sep 2021| **Console and tools** <ul><li>1.0.0-20211005</li><li>1.0.0-20210915-amd64</li></ul> **Fabric nodes** <ul><li>2.2.3-20211005</li><li>2.2.3-20210915</li></ul> **CouchDB** <ul><li>2.2.3-20211005</li> <li>2.2.3-20210915</li></ul> | **Fabric Version Upgrade** <ul><li>Fabric version 2.2.3</li></ul> **Improvements to the Console UI** <ul><li>Support for Fabric v2.2.3 Lifecycle.</li></ul> |

**Additional platforms** <ul><li>Platform can be deployed on the OpenShift Container Platform 3.11</ul> |
{: caption="Table 1. {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric versions" caption-side="bottom"}

## Before you begin
{: #upgrade-k8-before}

The upgrade process that you follow depends on the version of the platform that you are upgrading from v1.0.0.
- [Upgrade to {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0](#upgrade-k8-steps-100)


Or, if you are upgrading from behind a firewall
- [Upgrade to {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0](#upgrade-k8-firewall)

After you upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric Operator, the Operator will automatically upgrade the console that is deployed on your Kubernetes namespace. You can then use the upgraded console to upgrade your blockchain nodes.

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, deploy smart contracts, or create new channels during the upgrade process.

Updating the Operator triggers a restart of all components managed by this installation of the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric including Fabric nodes. To avoid disruption of service, a multiregion setup is recommended.
{: note}

It is a best practice to upgrade your SDK to the latest version as part of a general upgrade of your network. While the SDK will always be compatible with equivalent releases of Fabric and lower, it might be necessary to upgrade to the latest SDK to leverage the latest Fabric features. Also, after upgrading, it's possible your client application may experience errors. Consult the your Fabric SDK documentation for information about how to upgrade.
{: tip}

## Upgrade to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 
{: #upgrade-k8-steps-100}

When you upgrade to {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 you need to update the webhook, the custom resource definitions (CRDs), the ClusterRole, and the operator using the following steps. **These same steps can be followed even if your deployment is behind a firewall.**

1. [Update webhook image.](#upgrade-k8-steps-100-webhook)
2. [Update the CRDs.](#upgrade-k8-steps-100-crds) 
3. [Update the ClusterRole.](#upgrade-k8-steps-100-clusterrole) 
4. [Update the operator.](#upgrade-k8-steps-100-operator)
5. [Use your console to upgrade your running blockchain nodes.](#upgrade-k8-nodes)
6. [Update MSPs in consortium to add organization-level endorsement policy.](#upgrade-k8s-update-consortium)

You need to repeat steps 3-6 for each network that that runs in a separate namespace. If you experience any problems, see the instructions for [rolling back an upgrade](#upgrade-k8-rollback). If you deployed your network behind a firewall, without access to the external internet, see the separate set of instructions for [Upgrading the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric behind a firewall](#upgrade-k8-firewall).

### Step one: Update the webhook image
{: #upgrade-k8-steps-100-webhook}

Log in to your cluster and run the following command to update the webhook image in the `ibm-hlfsupport-infra` namespace or project:

```
kubectl set image deploy/ibm-hlfsupport-webhook -n ibm-hlfsupport-infra ibm-hlfsupport-webhook="cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211005-amd64"
```
{: codeblock}

### Step two: Update the CRDs
{: #upgrade-k8-steps-100-crds}

* Extract the webhook TLS certificate from the `ibm-hlfsupport-infra` namespace by running the following command: 
  ``` 
  export TLS_CERT=$(kubectl get secret/webhook-tls-cert -n ibm-hlfsupport-infra -o jsonpath={'.data.cert\.pem'})
  ```
  {: codeblock}

* When you deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 you need to apply the following four CRDs for the CA, peer, orderer, and console. Run the following four commands to apply or update each CRD.

Run this command to update the CA CRD:   
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibpcas.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v210
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: false
    storage: false
    subresources:
      status: {}
  - name: v212
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: false
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

Depending on whether you are creating or updating the CRD, when successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com created
```

or
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com configured
```

Run this command to update the peer CRD:
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibppeers.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com created
```
or
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com configured
```

Run this command to update the console CRD:
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibpconsoles.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com created
```
or
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com configured
```

Run this command to update the orderer CRD:  
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibporderers.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com created
```
{: codeblock}

or
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com configured
```
{: codeblock}


### Step three: Update the ClusterRole
{: #upgrade-k8-steps-100-clusterrole}

You need to update the ClusterRole that is applied to your namespace. Copy the following text to a file on your local system and save the file as `ibm-hlfsupport-clusterrole.yaml`. Edit the file and replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace.  

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: <NAMESPACE>
  labels:
    release: "operator"
    helm.sh/chart: "ibm-hlfsupport"
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport-operator"
rules:
- apiGroups:
  - extensions
  resourceNames:
  - ibm-hlfsupport-psp
  resources:
  - podsecuritypolicies
  verbs:
  - use
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - persistentvolumeclaims
  - persistentvolumes
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - customresourcedefinitions
  verbs:
  - get
- apiGroups:
  - route.openshift.io
  resources:
  - routes
  - routes/custom-host
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - ""
  resources:
  - pods
  - pods/log
  - persistentvolumeclaims
  - persistentvolumes
  - services
  - endpoints
  - events
  - configmaps
  - secrets
  - nodes
  - serviceaccounts
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - "batch"
  resources:
  - jobs
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - "authorization.openshift.io"
  - "rbac.authorization.k8s.io"
  resources:
  - roles
  - rolebindings
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
  - bind
  - escalate
- apiGroups:
  - ""
  resources:
  - namespaces
  verbs:
  - get
- apiGroups:
  - apps
  resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - monitoring.coreos.com
  resources:
  - servicemonitors
  verbs:
  - get
  - create
- apiGroups:
  - apps
  resourceNames:
  - ibm-hlfsupport-operator
  resources:
  - deployments/finalizers
  verbs:
  - update
- apiGroups:
  - ibp.com
  resources:
  - ibpcas.ibp.com
  - ibppeers.ibp.com
  - ibporderers.ibp.com
  - ibpconsoles.ibp.com
  - ibpcas
  - ibppeers
  - ibporderers
  - ibpconsoles
  - ibpcas/finalizers
  - ibppeers/finalizers
  - ibporderers/finalizers
  - ibpconsoles/finalizers
  - ibpcas/status
  - ibppeers/status
  - ibporderers/status
  - ibpconsoles/status
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - extensions
  - networking.k8s.io
  - config.openshift.io
  resources:
  - ingresses
  - networkpolicies
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
```
{: codeblock}

After you edit and save the file, run the following commands.
```
oc apply -f ibm-hlfsupport-clusterrole.yaml
oc adm policy add-scc-to-group <NAMESPACE> system:serviceaccounts:<NAMESPACE>
```
{: codeblock}

### Step four: Upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator
{: #upgrade-k8-steps-100-operator}

Before updating the operator image, you need to stop the ibm-hlfsupport-console by running the following command. Replace <NAMESPACE> with the name of your Kubernetes namespace:
```
kubectl patch ibpconsole ibm-hlfsupport-console -n <NAMESPACE> -p='[{"op": "replace", "path":"/spec/replicas", "value":0}]' --type=json
```
{: codeblock}

Wait a few minutes for the console to stop. While the console is stopped you are unable to deploy or manage your blockchain components.
{: note}

Run the following command to upgrade the operator. Replace `<NAMESPACE>` with the Kubernetes namespace for the platform.
```
kubectl set image deploy/ibm-hlfsupport-operator -n <NAMESPACE> ibm-hlfsupport-operator="cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211005-amd64"
```
{: codeblock}

After applying the new image to the operator deployment, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to deploy smart contracts, or use the console or the APIs to create or remove a node.

Run the `kubectl get deployment ibm-hlfsupport-operator -o yaml` command to confirm that the command updated the operator spec.

You can check that the upgrade is complete by running `kubectl get deployment`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibm-hlfsupport-operator   1/1       1            1           1m
ibm-hlfsupport-console     1/1       1            1           4m
```
{: codeblock}

After the operator restarts, you can now restart the console by running the following command:

```
kubectl patch ibpconsole ibm-hlfsupport-console -n <NAMESPACE> -p='[{"op": "replace", "path":"/spec/replicas", "value":1}]' --type=json
```
{: codeblock}

### Step five: Upgrade your nodes

You can now follow the [steps](#upgrade-k8-nodes) to upgrade your blockchain nodes. Be aware that after the nodes are upgraded, there is no way to roll back the upgrade from 1.0.0. After you upgrade to 1.0.0, you can take advantage of the new Fabric v2.x lifecycle deployment process for your smart contracts. But to avoid your peers crashing, you need to ensure that you upgrade your peers before you upgrade your channels. Learn more about considerations when  [Upgrading to a new version of Fabric](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern-components#ibm-hlfsupport-console-govern-components-upgrade).

After upgrading your nodes and updating your channels, make sure you update your organizations to [add an organization-level endorsement policy](#upgrade-k8s-update-consortium).

### Step six: Update MSPs in consortium to add organization-level endorsement policy
{: #upgrade-k8s-update-consortium}

To use the 2.x smart contract lifecycle, an organization must have an endorsement policy defined. If any organization in the consortium (the list of organizations maintained by the ordering service that are allowed to create channels) do not have an endorsement policy defined, a warning message will appear on the **Details** page of the ordering service with a list of organization MSPs that must be updated.

The best practice to add this endorsement policy to the MSP is to delete the MSP from the system channel and then re-add the MSP. The console detects the fact that the MSP does not contain the endorsement policy and automatically adds it. Note that this action can only be completed by an ordering service administrator. You do not need to delete and re-add the MSPs in the configuration of any application channels that have already been created. For these MSPs, the endorsement policy is added as part of the process of deploying the smart contract.

## Upgrade to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 
{: #upgrade-k8-steps-21x}

You can upgrade an {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network by using the following steps:

1. [Create the `ibm-hlfsupport-infra` project for the webhook](#upgrade-k8s-ibm-hlfsupport-infra)
2. [Create a secret for your entitlement key](#upgrade-k8s-secret-ibm-hlfsupport-infra)
3. [Deploy the webhook and custom resource definitions to your Kubernetes cluster](#upgrade-k8s-webhook-crd)
4. [Update the ClusterRole](#upgrade-k8-clusterrole)
5. [Upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator](#upgrade-k8-operator)
6. [Use your console to upgrade your running blockchain nodes](#upgrade-k8-nodes)
7. [Update MSPs in consortium to add organization-level endorsement policy](#upgrade-k8-before-steps-21x-update-consortium)

You need to complete steps 4-6 for each network that that runs on a separate namespace. If you experience any problems, see the instructions for [rolling back an upgrade](#upgrade-k8-rollback). If you deployed your network behind a firewall, without access to the external internet, see the separate set of instructions for [Upgrading the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric behind a firewall](#upgrade-k8-firewall).

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, deploy smart contracts, or create new channels during the upgrade process.

It is a best practice to upgrade your SDK to the latest version as part of a general upgrade of your network. While the SDK will always be compatible with equivalent releases of Fabric and lower, it might be necessary to upgrade to the latest SDK to leverage the latest Fabric features. Also, after upgrading, it's possible your client application may experience errors. Consult the your Fabric SDK documentation for information about how to upgrade.
{: tip}

### Before you begin
{: #upgrade-k8-before-steps-21x}

To upgrade your network, you need to [retrieve your entitlement key](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-entitlement-key) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the Entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.


### Step one: Create the `ibm-hlfsupport-infra` namespace for the webhook
{: #upgrade-k8s-ibm-hlfsupport-infra}

Because the platform has updated the internal apiversion from `v1alpha1` in previous versions to `v1beta1` in 1.0.0, a Kubernetes conversion webhook is required to update the CA, peer, operator, and console to the new API version. This webhook will continue to be used in the future, so new deployments of the platform are required to deploy it as well.  The webhook is deployed to its own namespace, referred to as  `ibm-hlfsupport-infra` throughout these instructions.

After you log in to your cluster, you can create the new `ibm-hlfsupport-infra` namespace for the Kubernetes conversion webhook using the kubectl CLI. The new namespace needs to be created by a cluster administrator.

Run the following command to create the namespace:
```
kubectl create namespace ibm-hlfsupport-infra
```
{: codeblock}

### Step two: Create a secret for your entitlement key
{: #upgrade-k8s-secret-ibm-hlfsupport-infra}

After you purchase the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. You need to store the entitlement key on your cluster by creating a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/){: external}. Kubernetes secrets are used to securely store the key on your cluster and pass it to the operator and the console deployments.

Run the following command to create the secret and add it to your `ibm-hlfsupport-infra` namespace or project:
```
kubectl create secret docker-registry cp-pull-secret --docker-server=cp.icr.io --docker-username=cp --docker-password=<KEY> --docker-email=<EMAIL> -n ibm-hlfsupport-infra
```
{: codeblock}

- Replace `<KEY>` with your entitlement key.
- Replace `<EMAIL>` with your email address.

The name of the secret that you are creating is `cp-pull-secret`. It is required by the webhook that you will deploy later. You can only use the key once per deployment. You can refresh the key before you attempt another deployment and use that value here.
{: note}


### Step three: Deploy the webhook and custom resource definitions to your Kubernetes cluster
{: #upgrade-k8s-webhook-crd}

Before you can deploy a new instance of the platform to your Kubernetes cluster, you need to create the conversion webhook by completing the steps in this section. The webhook is deployed to its own namespace or project, referred to `ibm-hlfsupport-infra` throughout these instructions.

The first three steps are for deployment of the webhook. The last step is for creation of the custom resource definitions for the CA, peer, orderer and console components that the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric requires. You only have to deploy the webhook and custom resource definitions **once per cluster**. If you have already deployed this webhook and custom resource definitions to your cluster, you can skip these four steps below.
{: important}

#### 1. Configure role-based access control (RBAC) for the webhook
{: #upgrade-webhook-rbac}

Copy the following text to a file on your local system and save the file as `rbac.yaml`. This step allows the webhook to read and create a TLS secret in its own project.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: webhook
  namespace: ibm-hlfsupport-infra
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: webhook
rules:
- apiGroups:
  - "*"
  resources:
  - secrets
  verbs:
  - "*"
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: ibm-hlfsupport-infra
subjects:
- kind: ServiceAccount
  name: webhook
  namespace: ibm-hlfsupport-infra
roleRef:
  kind: Role
  name: webhook
  apiGroup: rbac.authorization.k8s.io

```
{: codeblock}

Run the following command to add the file to your cluster definition:
```
kubectl apply -f rbac.yaml -n ibm-hlfsupport-infra
```
{: codeblock}

When the command completes successfully, you should see something similar to:
```
serviceaccount/webhook created
role.rbac.authorization.k8s.io/webhook created
rolebinding.rbac.authorization.k8s.io/ibm-hlfsupport-infra created
```
{: codeblock}

#### 2.(OpenShift cluster only) Apply the Security Context Constraint
{: #upgrade-webhook-scc}

Skip this step if you are not using OpenShift. The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric requires specific security and access policies to be added to the `ibm-hlfsupport-infra` project. Copy the security context constraint object below and save it to your local system as `ibm-hlfsupport-infra-scc.yaml`. Replace `<PROJECT_NAME>` with `ibm-hlfsupport-infra`.
```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: false
allowPrivilegedContainer: false
allowedCapabilities:
- NET_BIND_SERVICE
- CHOWN
- DAC_OVERRIDE
- SETGID
- SETUID
- FOWNER
apiVersion: security.openshift.io/v1
defaultAddCapabilities: []
fsGroup:
  type: RunAsAny
groups:
- system:serviceaccounts:<PROJECT_NAME>
kind: SecurityContextConstraints
metadata:
  name: <PROJECT_NAME>
readOnlyRootFilesystem: false
requiredDropCapabilities: []
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: RunAsAny
supplementalGroups:
  type: RunAsAny
users:
- system:serviceaccounts:<PROJECT_NAME>
volumes:
- "*"

```
{: codeblock}

After you save the file, run the following commands to add the file to your cluster and add the policy to your project.
```
oc apply -f ibm-hlfsupport-infra-scc.yaml -n ibm-hlfsupport-infra
oc adm policy add-scc-to-user ibm-hlfsupport-infra system:serviceaccounts:ibm-hlfsupport-infra
```
{: codeblock}

If the commands are successful, you can see a response that is similar to the following example:
```
securitycontextconstraints.security.openshift.io/ibm-hlfsupport-infra created
scc "ibm-hlfsupport-infra" added to: ["system:serviceaccounts:ibm-hlfsupport-infra"]
```
{: codeblock}

#### 3. Deploy the webhook
{: #upgrade-webhook-deploy}

In order to deploy the webhook, you need to create two `.yaml` files and apply them to your Kubernetes cluster.

##### deployment.yaml
{: #upgrade-webhook-deployment-yaml}

Copy the following text to a file on your local system and save the file as `deployment.yaml`. 

If you are deploying on OpenShift Container Platform on LinuxONE, you need to replace `amd64` with `s390x`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: "ibm-hlfsupport-webhook"
  labels:
    helm.sh/chart: "ibm-hlfsupport"
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        helm.sh/chart: "ibm-hlfsupport"
        app.kubernetes.io/name: "ibm-hlfsupport"
        app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
      annotations:
        productName: "IBM Support for Hyperledger Fabric"
        productID: "5d5997a033594f149a534a09802d60f1"
        productVersion: "1.0.0"
        productChargedContainers: ""
        productMetric: "VIRTUAL_PROCESSOR_CORE"
    spec:
      serviceAccountName: webhook
      imagePullSecrets:
        - name: cp-pull-secret
      hostIPC: false
      hostNetwork: false
      hostPID: false
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 2000
      containers:
        - name: "ibm-hlfsupport-webhook"
          image: "cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211026-amd64"
          imagePullPolicy: Always
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            runAsUser: 1000
            capabilities:
              drop:
              - ALL
              add:
              - NET_BIND_SERVICE
          env:
            - name: "LICENSE"
              value: "accept"
            - name: NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: SERVICE_NAME
              value: ibm-hflsupport-webhook
          ports:
            - name: server
              containerPort: 3000
          livenessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 30
            timeoutSeconds: 5
            failureThreshold: 6
          readinessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 26
            timeoutSeconds: 5
            periodSeconds: 5
          resources:
            requests:
              cpu: 0.1
              memory: "100Mi"

```
{: codeblock}

Run the following command to add the configuration to your cluster definition:
```
kubectl apply -n ibm-hlfsupport-infra -f deployment.yaml
```
{: codeblock}

When the command completes successfully you should see something similar to:
```
deployment.apps/ibm-hlfsupport-webhook created
```
{: codeblock}

##### service.yaml
{: #upgrade-webhook-service-yaml}

Secondly, copy the following text to a file on your local system and save the file as `service.yaml`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: "ibm-hlfsupport-webhook"
  labels:
    type: "webhook"
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
    helm.sh/chart: "ibm-hlfsupport"
spec:
  type: ClusterIP
  ports:
    - name: server
      port: 443
      targetPort: server
      protocol: TCP
  selector:
    app.kubernetes.io/instance: "ibm-hlfsupport-webhook"

```
{: codeblock}

Run the following command to add the configuration to your cluster definition:
```
kubectl apply -n ibm-hlfsupport-infra -f service.yaml
```
{: codeblock}

When the command completes successfully you should see something similar to:
```
service/ibm-hlfsupport-webhook created
```
{: codeblock}

#### 4. Extract the certificate and create the custom resource definitions
{: #upgrade-webhook-extract-cert}

* Extract the webhook TLS certificate from the `ibm-hlfsupport-infra` namespace by running the following command: 
  ``` 
  export TLS_CERT=$(kubectl get secret/webhook-tls-cert -n ibm-hlfsupport-infra -o jsonpath={'.data.cert\.pem'})
  ```
  {: codeblock}

* When you deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 you need to apply the following four CRDs for the CA, peer, orderer, and console. Run the following four commands to apply or update each CRD.

Run this command to update the CA CRD:   
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibpcas.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v210
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: false
    storage: false
    subresources:
      status: {}
  - name: v212
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: false
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

Depending on whether you are creating or updating the CRD, when successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com created
```

or
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com configured
```

Run this command to update the peer CRD:
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibppeers.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com created
```
or
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com configured
```

Run this command to update the console CRD:
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibpconsoles.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com created
```
or
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com configured
```

Run this command to update the orderer CRD:  
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibporderers.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com created
```
{: codeblock}

or
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com configured
```
{: codeblock}


### Step four: Update the ClusterRole
{: #upgrade-k8-clusterrole}

You need to update the ClusterRole that is applied to your project. Copy the following text to a file on your local system and save the file as `ibm-hlfsupport-clusterrole.yaml`. Edit the file and replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: <NAMESPACE>
  labels:
    release: "operator"
    helm.sh/chart: "ibm-hlfsupport"
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport-operator"
rules:
- apiGroups:
  - extensions
  resourceNames:
  - ibm-hlfsupport-psp
  resources:
  - podsecuritypolicies
  verbs:
  - use
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - persistentvolumeclaims
  - persistentvolumes
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - customresourcedefinitions
  verbs:
  - get
- apiGroups:
  - route.openshift.io
  resources:
  - routes
  - routes/custom-host
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - ""
  resources:
  - pods
  - pods/log
  - persistentvolumeclaims
  - persistentvolumes
  - services
  - endpoints
  - events
  - configmaps
  - secrets
  - nodes
  - serviceaccounts
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - "batch"
  resources:
  - jobs
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - "authorization.openshift.io"
  - "rbac.authorization.k8s.io"
  resources:
  - roles
  - rolebindings
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
  - bind
  - escalate
- apiGroups:
  - ""
  resources:
  - namespaces
  verbs:
  - get
- apiGroups:
  - apps
  resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - monitoring.coreos.com
  resources:
  - servicemonitors
  verbs:
  - get
  - create
- apiGroups:
  - apps
  resourceNames:
  - ibm-hlfsupport-operator
  resources:
  - deployments/finalizers
  verbs:
  - update
- apiGroups:
  - ibp.com
  resources:
  - ibpcas.ibp.com
  - ibppeers.ibp.com
  - ibporderers.ibp.com
  - ibpconsoles.ibp.com
  - ibpcas
  - ibppeers
  - ibporderers
  - ibpconsoles
  - ibpcas/finalizers
  - ibppeers/finalizers
  - ibporderers/finalizers
  - ibpconsoles/finalizers
  - ibpcas/status
  - ibppeers/status
  - ibporderers/status
  - ibpconsoles/status
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - extensions
  - networking.k8s.io
  - config.openshift.io
  resources:
  - ingresses
  - networkpolicies
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
```
{: codeblock}

After you save and edit the file, run the following commands. Replace `<NAMESPACE>` with your Kubernetes namespace.
```
kubectl apply -f ibm-hlfsupport-clusterrole.yaml -n <NAMESPACE>
```
{: codeblock}

Replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace.

### Step five: Upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator
{: #upgrade-k8-operator}

You can upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator by fetching the operator deployment spec from your cluster. When the upgraded operator is running, the new operator will upgrade your console and download the latest images for your blockchain nodes.

Log in to your cluster by using the kubectl CLI. Because each {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network runs in a different namespace, you must switch to each namespace and upgrade each network separately. use the following command to set the context to the namespace of the network that you want to upgrade. Replace `<NAMESPACE>` with your namespace.
```
kubectl config set-context --current --namespace=<NAMESPACE>
```
{: codeblock}

When you are operating from your namespace, run the following command to download the operator deployment spec to your local file system:
```
kubectl get deployment ibm-hlfsupport-operator -o yaml > operator.yaml
```
{: codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-upgrade.yaml`. You need to update the `image:` field with the updated version of the operator image. You can find the name and tag of the latest operator image below:
```
cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211005-amd64
```
{: codeblock}

You also need to edit the `env:` section of the file. Find the following lines in `operator-upgrade.yaml`:
```
- name: ISOPENSHIFT
  value: "false"
```
{: codeblock}

Replace the values above with the following lines at the same indentation:
```
- name: CLUSTERTYPE
  value: <CLUSTER_TYPE>
```
{: codeblock}

- Replace `<CLUSTER_TYPE>` with `K8S` if you are deploying the platform on open source Kubernetes or Rancher.

When you are finished editing the file, the `env:` section would look similar to the following:
```
env:
- name: WATCH_NAMESPACE
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.namespace
- name: POD_NAME
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.name
- name: OPERATOR_NAME
  value: ibm-hlfsupport-operator
- name: CLUSTERTYPE
  value: K8S
```
{: codeblock}

Save the file on your local system. You can then issue the following command upgrade your operator:
```
kubectl apply -f operator-upgrade.yaml -n <NAMESPACE>
```
{: codeblock}

Replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace.

You can use the `kubectl get deployment ibm-hlfsupport-operator -o yaml` command to confirm that the command updated the operator spec.

After you apply the `operator-upgrade.yaml` operator spec to your namespace, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to deploy smart contracts, or use the console or the APIs to create or remove a node.

You can check that the upgrade is complete by running `kubectl get deployment ibm-hlfsupport-operator`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
ibm-hlfsupport-operator   1         1         1            1           1m
ibm-hlfsupport-console     1         1         1            1           8m
```
{: codeblock}

If you experience a problem while you are upgrading the operator, go to this [troubleshooting topic](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-troubleshooting#ibm-hlfsupport-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems. You can run the command to apply the original operator file, `kubectl apply -f operator.yaml` to restore your original operator deployment.

### Step six: Upgrade your nodes

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. For more information, see [Upgrade your blockchain nodes](#upgrade-k8-nodes).

After upgrade your nodes, make sure you update the MSPs in the consortium to [add an organization-level endorsement policy](#upgrade-k8-before-steps-21x-update-consortium).

### Step seven: Update MSPs in consortium to add organization-level endorsement policy
{: #upgrade-k8-before-steps-21x-update-consortium}

To use the 2.x smart contract lifecycle, an organization must have an endorsement policy defined. If any organization in the consortium (the list of organizations maintained by the ordering service that are allowed to create channels) do not have an endorsement policy defined, a warning message will appear on the **Details** page of the ordering service with a list of organization MSPs that must be updated.

The best practice to add this endorsement policy to the MSP is to delete the MSP from the system channel and then re-add the MSP. The console detects the fact that the MSP does not contain the endorsement policy and automatically adds it. Note that this action can only be completed by an ordering service administrator. You do not need to delete and re-add the MSPs in the configuration of any application channels that have already been created. For these MSPs, the endorsement policy is added as part of the process of deploying the smart contract.

## Upgrade to the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 from behind a firewall
{: #upgrade-k8-firewall}

If you deployed the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric behind a firewall, without access to the external internet, you can upgrade your network by using the following steps:

1. [Pull the latest {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images](#upgrade-k8-images-firewall)
2. [Create the `ibm-hlfsupport-infra` project for the webhook](#upgrade-k8s-ibm-hlfsupport-infra-fw)
3. [Create a secret for your entitlement key](#upgrade-k8s-secret-ibm-hlfsupport-infra-fw)
4. [Deploy the webhook and custom resource definitions to your Kubernetes cluster](#upgrade-webhook-fw)
5. [Update the ClusterRole](#upgrade-k8-clusterrole-firewall)
6. [Upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator](#upgrade-k8-operator-firewall)
7. [Use your console to upgrade your running blockchain nodes](#upgrade-k8-nodes-firewall)
8. [Update MSPs in consortium to add organization-level endorsement policy](#upgrade-k8s-nodes-firewall-update-consortium)

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, deploy smart contracts, or create new channels during the upgrade process.

### Before you begin
{: #upgrade-k8-begin-firewall}

To upgrade your network, you need to [retrieve your entitlement key](/docs/hlf-support?topic=hlf-support-deploy-k8-firewall#deploy-k8-entitlement-key-firewall) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/hlf-support?topic=hlf-support-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the Entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

### Step one: Pull the latest {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images
{: #upgrade-k8-images-firewall}

To upgrade your network, download the latest set of {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric images and push them to a container registry that you can access from behind your firewall.

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
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-operator:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-init:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-init:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-console:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-console:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-grpcweb:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-grpcweb:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-deployer:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-deployer:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-fluentd:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-fluentd:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:2.3.1-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-couchdb:2.3.1-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-couchdb:3.1.1-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-couchdb:3.1.1-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-peer:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-peer:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-orderer:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-orderer:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ca:1.5.0-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-ca:1.5.0-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-dind:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-dind:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-utilities:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-utilities:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-peer:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-peer:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-orderer:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-orderer:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-chaincode-launcher:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-chaincode-launcher:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-utilities:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-utilities:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-ccenv:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-goenv:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-goenv:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-nodeenv:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:2.2.3-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-javaenv:2.2.3-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-crdwebhook:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-ccenv:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-ccenv:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-goenv:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-goenv:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-nodeenv:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-nodeenv:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-javaenv:1.4.12-20210713 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-javaenv:1.4.12-20210713 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
skopeo copy docker://cp.icr.io/cp/ibm-hlfsupport-enroller:1.0.0-20211005 docker://<LOCAL_REGISTRY>/ibm-hlfsupport-enroller:1.0.0-20211005 -q --src-creds cp:<ENTITLEMENT_KEY> --dest-creds <LOCAL_REGISTRY_USER>:<LOCAL_REGISTRY_PASSWORD> --all
```
{: codeblock}

After you complete these steps, you can use the following instructions to deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric with the images in your registry.

### Step two: Create the `ibm-hlfsupport-infra` namespace for the webhook
{: #upgrade-k8s-ibm-hlfsupport-infra-fw}

Because the platform has updated the internal apiversion from `v1alpha1` in previous versions to `v1beta1` in 1.0.0, a Kubernetes conversion webhook is required to update the CA, peer, operator, and console to the new API version. This webhook will continue to be used in the future, so new deployments of the platform are required to deploy it as well.  The webhook is deployed to its own namespace, referred to as  `ibm-hlfsupport-infra` throughout these instructions.

After you log in to your cluster, you can create the new `ibm-hlfsupport-infra` namespace for the Kubernetes conversion webhook using the kubectl CLI. The new namespace needs to be created by a cluster administrator.

Run the following command to create the namespace:
```
kubectl create namespace ibm-hlfsupport-infra
```
{: codeblock}

### Step three: Create a secret for your entitlement key
{: #upgrade-k8s-secret-ibm-hlfsupport-infra-fw}

After you purchase the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. You need to store the entitlement key on your cluster by creating a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/){: external}. Kubernetes secrets are used to securely store the key on your cluster and pass it to the operator and the console deployments.

Run the following command to create the secret and add it to your `ibm-hlfsupport-infra` namespace or project:
```
kubectl create secret docker-registry cp-pull-secret --docker-server=cp.icr.io --docker-username=cp --docker-password=<KEY> --docker-email=<EMAIL> -n ibm-hlfsupport-infra
```
{: codeblock}

- Replace `<KEY>` with your entitlement key.
- Replace `<EMAIL>` with your email address.

The name of the secret that you are creating is `cp-pull-secret`. It is required by the webhook that you will deploy later. You can only use the key once per deployment. You can refresh the key before you attempt another deployment and use that value here.
{: note}


### Step four: Deploy the webhook and custom resource definitions to your Kubernetes cluster
{: #upgrade-webhook-fw}

Before you can deploy a new instance of the platform to your Kubernetes cluster, you need to create the conversion webhook by completing the steps in this section. The webhook is deployed to its own namespace or project, referred to `ibm-hlfsupport-infra` throughout these instructions.

The first three steps are for deployment of the webhook. The last step is for creation of the custom resource definitions for the CA, peer, orderer and console components that the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric requires. You only have to deploy the webhook and custom resource definitions **once per cluster**. If you have already deployed this webhook and custom resource definitions to your cluster, you can skip these four steps below.
{: important}

#### 1. Configure role-based access control (RBAC) for the webhook
{: #upgrade-webhook-rbac}

Copy the following text to a file on your local system and save the file as `rbac.yaml`. This step allows the webhook to read and create a TLS secret in its own project.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: webhook
  namespace: ibm-hlfsupport-infra
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: webhook
rules:
- apiGroups:
  - "*"
  resources:
  - secrets
  verbs:
  - "*"
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: ibm-hlfsupport-infra
subjects:
- kind: ServiceAccount
  name: webhook
  namespace: ibm-hlfsupport-infra
roleRef:
  kind: Role
  name: webhook
  apiGroup: rbac.authorization.k8s.io

```
{: codeblock}

Run the following command to add the file to your cluster definition:
```
kubectl apply -f rbac.yaml -n ibm-hlfsupport-infra
```
{: codeblock}

When the command completes successfully, you should see something similar to:
```
serviceaccount/webhook created
role.rbac.authorization.k8s.io/webhook created
rolebinding.rbac.authorization.k8s.io/ibm-hlfsupport-infra created
```
{: codeblock}

#### 2.(OpenShift cluster only) Apply the Security Context Constraint
{: #upgrade-webhook-scc}

Skip this step if you are not using OpenShift. The {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric requires specific security and access policies to be added to the `ibm-hlfsupport-infra` project. Copy the security context constraint object below and save it to your local system as `ibm-hlfsupport-infra-scc.yaml`. Replace `<PROJECT_NAME>` with `ibm-hlfsupport-infra`.
```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: false
allowPrivilegedContainer: false
allowedCapabilities:
- NET_BIND_SERVICE
- CHOWN
- DAC_OVERRIDE
- SETGID
- SETUID
- FOWNER
apiVersion: security.openshift.io/v1
defaultAddCapabilities: []
fsGroup:
  type: RunAsAny
groups:
- system:serviceaccounts:<PROJECT_NAME>
kind: SecurityContextConstraints
metadata:
  name: <PROJECT_NAME>
readOnlyRootFilesystem: false
requiredDropCapabilities: []
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: RunAsAny
supplementalGroups:
  type: RunAsAny
users:
- system:serviceaccounts:<PROJECT_NAME>
volumes:
- "*"

```
{: codeblock}

After you save the file, run the following commands to add the file to your cluster and add the policy to your project.
```
oc apply -f ibm-hlfsupport-infra-scc.yaml -n ibm-hlfsupport-infra
oc adm policy add-scc-to-user ibm-hlfsupport-infra system:serviceaccounts:ibm-hlfsupport-infra
```
{: codeblock}

If the commands are successful, you can see a response that is similar to the following example:
```
securitycontextconstraints.security.openshift.io/ibm-hlfsupport-infra created
scc "ibm-hlfsupport-infra" added to: ["system:serviceaccounts:ibm-hlfsupport-infra"]
```
{: codeblock}

#### 3. Deploy the webhook
{: #upgrade-webhook-deploy}

In order to deploy the webhook, you need to create two `.yaml` files and apply them to your Kubernetes cluster.

##### deployment.yaml
{: #upgrade-webhook-deployment-yaml}

Copy the following text to a file on your local system and save the file as `deployment.yaml`. 

If you are deploying on OpenShift Container Platform on LinuxONE, you need to replace `amd64` with `s390x`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: "ibm-hlfsupport-webhook"
  labels:
    helm.sh/chart: "ibm-hlfsupport"
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        helm.sh/chart: "ibm-hlfsupport"
        app.kubernetes.io/name: "ibm-hlfsupport"
        app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
      annotations:
        productName: "IBM Support for Hyperledger Fabric"
        productID: "5d5997a033594f149a534a09802d60f1"
        productVersion: "1.0.0"
        productChargedContainers: ""
        productMetric: "VIRTUAL_PROCESSOR_CORE"
    spec:
      serviceAccountName: webhook
      imagePullSecrets:
        - name: cp-pull-secret
      hostIPC: false
      hostNetwork: false
      hostPID: false
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 2000
      containers:
        - name: "ibm-hlfsupport-webhook"
          image: "cp.icr.io/cp/ibm-hlfsupport-crdwebhook:1.0.0-20211026-amd64"
          imagePullPolicy: Always
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            runAsUser: 1000
            capabilities:
              drop:
              - ALL
              add:
              - NET_BIND_SERVICE
          env:
            - name: "LICENSE"
              value: "accept"
            - name: NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: SERVICE_NAME
              value: ibm-hflsupport-webhook
          ports:
            - name: server
              containerPort: 3000
          livenessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 30
            timeoutSeconds: 5
            failureThreshold: 6
          readinessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 26
            timeoutSeconds: 5
            periodSeconds: 5
          resources:
            requests:
              cpu: 0.1
              memory: "100Mi"

```
{: codeblock}

Run the following command to add the configuration to your cluster definition:
```
kubectl apply -n ibm-hlfsupport-infra -f deployment.yaml
```
{: codeblock}

When the command completes successfully you should see something similar to:
```
deployment.apps/ibm-hlfsupport-webhook created
```
{: codeblock}

##### service.yaml
{: #upgrade-webhook-service-yaml}

Secondly, copy the following text to a file on your local system and save the file as `service.yaml`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: "ibm-hlfsupport-webhook"
  labels:
    type: "webhook"
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport-webhook"
    helm.sh/chart: "ibm-hlfsupport"
spec:
  type: ClusterIP
  ports:
    - name: server
      port: 443
      targetPort: server
      protocol: TCP
  selector:
    app.kubernetes.io/instance: "ibm-hlfsupport-webhook"

```
{: codeblock}

Run the following command to add the configuration to your cluster definition:
```
kubectl apply -n ibm-hlfsupport-infra -f service.yaml
```
{: codeblock}

When the command completes successfully you should see something similar to:
```
service/ibm-hlfsupport-webhook created
```
{: codeblock}

#### 4. Extract the certificate and create the custom resource definitions
{: #upgrade-webhook-extract-cert}

* Extract the webhook TLS certificate from the `ibm-hlfsupport-infra` namespace by running the following command: 
  ``` 
  export TLS_CERT=$(kubectl get secret/webhook-tls-cert -n ibm-hlfsupport-infra -o jsonpath={'.data.cert\.pem'})
  ```
  {: codeblock}

* When you deploy the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric 1.0.0 you need to apply the following four CRDs for the CA, peer, orderer, and console. Run the following four commands to apply or update each CRD.

Run this command to update the CA CRD:   
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibpcas.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v210
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: false
    storage: false
    subresources:
      status: {}
  - name: v212
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: false
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

Depending on whether you are creating or updating the CRD, when successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com created
```

or
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com configured
```

Run this command to update the peer CRD:
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibppeers.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com created
```
or
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com configured
```

Run this command to update the console CRD:
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibpconsoles.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com created
```
or
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com configured
```

Run this command to update the orderer CRD:  
```yaml
cat <<EOF | kubectl apply  -f -
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ibporderers.ibp.com
  labels:
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport"
spec:
  conversion:
    strategy: Webhook
    webhook:
      clientConfig:
        caBundle: "${TLS_CERT}"
        service:
          name: ibm-hlfsupport-webhook
          namespace: ibm-hlfsupport-infra
          path: /crdconvert
      conversionReviewVersions:
      - v1beta1
      - v1alpha2
      - v1alpha1
  group: ibp.com
  names:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  scope: Namespaced
  versions:
  - name: v1beta1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: true
    subresources:
      status: {}
  - name: v1alpha2
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
  - name: v1alpha1
    schema:
      openAPIV3Schema:
        x-kubernetes-preserve-unknown-fields: true
    served: true
    storage: false
    subresources:
      status: {}
status:
  acceptedNames:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  conditions: []
  storedVersions:
  - v1beta1
EOF
```
{: codeblock}

When successful, you should see:
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com created
```
{: codeblock}

or
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com configured
```
{: codeblock}


### Step five: Update the ClusterRole
{: #upgrade-k8-clusterrole-firewall}

You need to update the ClusterRole that is applied to your project. Copy the following text to a file on your local system and save the file as `ibm-hlfsupport-clusterrole.yaml`. Edit the file and replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace.  

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: <NAMESPACE>
  labels:
    release: "operator"
    helm.sh/chart: "ibm-hlfsupport"
    app.kubernetes.io/name: "ibm-hlfsupport"
    app.kubernetes.io/instance: "ibm-hlfsupport"
    app.kubernetes.io/managed-by: "ibm-hlfsupport-operator"
rules:
- apiGroups:
  - extensions
  resourceNames:
  - ibm-hlfsupport-psp
  resources:
  - podsecuritypolicies
  verbs:
  - use
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - persistentvolumeclaims
  - persistentvolumes
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - customresourcedefinitions
  verbs:
  - get
- apiGroups:
  - route.openshift.io
  resources:
  - routes
  - routes/custom-host
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - ""
  resources:
  - pods
  - pods/log
  - persistentvolumeclaims
  - persistentvolumes
  - services
  - endpoints
  - events
  - configmaps
  - secrets
  - nodes
  - serviceaccounts
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - "batch"
  resources:
  - jobs
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - "authorization.openshift.io"
  - "rbac.authorization.k8s.io"
  resources:
  - roles
  - rolebindings
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
  - bind
  - escalate
- apiGroups:
  - ""
  resources:
  - namespaces
  verbs:
  - get
- apiGroups:
  - apps
  resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - monitoring.coreos.com
  resources:
  - servicemonitors
  verbs:
  - get
  - create
- apiGroups:
  - apps
  resourceNames:
  - ibm-hlfsupport-operator
  resources:
  - deployments/finalizers
  verbs:
  - update
- apiGroups:
  - ibp.com
  resources:
  - ibpcas.ibp.com
  - ibppeers.ibp.com
  - ibporderers.ibp.com
  - ibpconsoles.ibp.com
  - ibpcas
  - ibppeers
  - ibporderers
  - ibpconsoles
  - ibpcas/finalizers
  - ibppeers/finalizers
  - ibporderers/finalizers
  - ibpconsoles/finalizers
  - ibpcas/status
  - ibppeers/status
  - ibporderers/status
  - ibpconsoles/status
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
- apiGroups:
  - extensions
  - networking.k8s.io
  - config.openshift.io
  resources:
  - ingresses
  - networkpolicies
  verbs:
  - get
  - list
  - create
  - update
  - patch
  - watch
  - delete
  - deletecollection
```
{: codeblock}

After you save and edit the file, run the following commands. Replace `<NAMESPACE>` with your Kubernetes namespace.
```
kubectl apply -f ibm-hlfsupport-clusterrole.yaml -n <NAMESPACE>
```
{: codeblock}

Replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace.

### Step six: Upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator
{: #upgrade-k8-operator-firewall}

You can upgrade the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric operator by fetching the operator deployment spec from your cluster. You can then update the spec with the latest operator image that you pushed to your local registry. When the upgraded operator is running, the new operator will download the images that you pushed to your local registry and upgrade your console.

Log in to your cluster by using the kubectl CLI. Because each {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric network runs in a different namespace, you must switch to each namespace and upgrade each network separately. use the following command to set the context to the namespace of the network that you want to upgrade. Replace `<NAMESPACE>` with your namespace.
```
kubectl config set-context --current --namespace=<NAMESPACE>
```
{: codeblock}

Replace `<NAMESPACE>` with the name of your {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric deployment namespace.

When you are operating from your namespace, run the following command to download the operator deployment spec to your local file system:
```
kubectl get deployment ibm-hlfsupport-operator -o yaml > operator.yaml
```
{: codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-upgrade.yaml`. You need to update the `image:` field with the updated version of the operator image:
```yaml
cp.icr.io/cp/ibm-hlfsupport-operator:1.0.0-20211005-amd64
```
{: codeblock}

You also need to edit the `env:` section of the file. Find the following lines in `operator-upgrade.yaml`:
```yaml
- name: ISOPENSHIFT
  value: "false"
```
{: codeblock}

Replace the values above with the following lines at the same indentation:
```yaml
- name: CLUSTERTYPE
  value: <CLUSTER_TYPE>
```
{: codeblock}

- Replace `<CLUSTER_TYPE>` with `K8S` if you are deploying the platform on open source Kubernetes or Rancher.

When you are finished editing the file, the `env:` section would look similar to the following:
```yaml
env:
- name: WATCH_NAMESPACE
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.namespace
- name: POD_NAME
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.name
- name: OPERATOR_NAME
  value: ibm-hlfsupport-operator
- name: CLUSTERTYPE
  value: K8S
```
{: codeblock}

Save the file on your local system. You can then issue the following command upgrade your operator:
```
kubectl apply -f operator-upgrade.yaml
```
{: codeblock}

You can use the `kubectl get deployment ibm-hlfsupport-operator -o yaml` command to confirm that the command updated the operator spec.

After you apply the `operator-upgrade.yaml` deployment spec to your namespace, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to deploy smart contracts, or use the console or the APIs to create or remove a node.

You can check that the upgrade is complete by running `kubectl get deployment ibm-hlfsupport-operator`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
ibm-hlfsupport-operator   1         1         1            1           1m
ibm-hlfsupport-console     1         1         1            1           8m
```
{: codeblock}

If you experience a problem while you are upgrading the operator, go to this [troubleshooting topic](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-v2-troubleshooting#ibm-hlfsupport-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems.

If your console experiences an image pull error, you may need to update the console CR spec with local registry that you used to download the images. Run the following command to download the CR spec of the console:
```
kubectl get ibpconsole ibm-hlfsupport-console -o yaml > console.yaml
```
{: codeblock}

Then add the URL of your local registry to the `spec:` section of `console.yaml`. Replace `<LOCAL_REGISTRY>` with the URL of your local registry:
```yaml
spec:
  registryURL: <LOCAL_REGISTRY>
```
{: codeblock}

Save the updated file as `console-upgrade.yaml` on your local system. You can then issue the following command upgrade your console:
```
kubectl apply -f console-upgrade.yaml
```
{: codeblock}

### Step seven: Upgrade your blockchain nodes
{: #upgrade-k8-nodes-firewall}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. For more information, see [Upgrade your blockchain nodes](#upgrade-k8-nodes).

### Step eight: Update MSPs in consortium to add organization-level endorsement policy
{: #upgrade-k8s-nodes-firewall-update-consortium}

To use the 2.x smart contract lifecycle, an organization must have an endorsement policy defined. If any organization in the consortium (the list of organizations maintained by the ordering service that are allowed to create channels) do not have an endorsement policy defined, a warning message will appear on the **Details** page of the ordering service with a list of organization MSPs that must be updated.

The best practice to add this endorsement policy to the MSP is to delete the MSP from the system channel and then re-add the MSP. The console detects the fact that the MSP does not contain the endorsement policy and automatically adds it. Note that this action can only be completed by an ordering service administrator. You do not need to delete and re-add the MSPs in the configuration of any application channels that have already been created. For these MSPs, the endorsement policy is added as part of the process of deploying the smart contract.

## Upgrade your blockchain nodes
{: #upgrade-k8-nodes}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. Browse to the console UI and open the nodes overview tab. You can find the **Upgrade available** text on a node tile if there is an upgrade available for the component. You can install this upgrade whenever you are ready. These upgrades are optional, but they are recommended. You cannot upgrade nodes that were imported into the console.

Apply upgrades to nodes one at a time. Your nodes are unavailable to process requests or transactions while the patch is being applied. Therefore, to avoid any disruption of service, you need to ensure that another node of the same type is available to process requests whenever possible. Installing upgrades on a node takes about a minute to complete and when it is complete, the node is ready to process requests.
{:important}

To update a node, open the node tile and click the **Upgrade available** button. You cannot update nodes that you imported to the console. Learn more about considerations when [Upgrading to a new version of Fabric](/docs/hlf-support?topic=hlf-support-ibm-hlfsupport-console-govern-components#ibm-hlfsupport-console-govern-components-upgrade).

## Roll back an upgrade
{: #upgrade-k8-rollback}

When you upgrade your operator, it saves the secrets, deployment spec, and network information of your console before it the operator attempts to upgrade the console. If your upgrade fails for any reason, {{site.data.keyword.IBM_notm}} Support can roll back your upgrade and restore your previous deployment by using the information on your cluster. If you need to roll back your upgrade, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/node/1072956){: external} page.

You can roll back an upgrade after you use the console to operate your network. However, after you use the console to upgrade your blockchain nodes, you can no longer roll back your console to a previous version of the platform.
