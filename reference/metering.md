---

copyright:
  years: 2022
lastupdated: "2022-07-08"

keywords: metering, licensing service, license consumption, license tracking

subcollection: hlf-support

---

{{site.data.keyword.attribute-definition-list}}


{:table:.aria-labeledby="caption"}





Documentation for this on-prem product has been moved from IBM Cloud to IBM Documentation at [https://www.ibm.com/docs/en/SSGWM34_1.0.0/reference/metering.html](https://www.ibm.com/docs/en/SSGWM34_1.0.0/reference/metering.html){: external}. Update your bookmarks.
{: important}

# Tracking license consumption
{: #metering}

This page describes the licensing service that can be used with the {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric to track license consumption. The License Service collects and measures the license usage of Virtual Processor Core (VPC) metric at the cluster level.
{: shortdesc}

## Overview
{: #metering-overview}

The integrated licensing solution collects and stores licensing information that can be used for audit purposes and for tracking license consumption in cloud environments and on premises. This solution works in the background and does not require any configuration. Only one instance of the license service is deployed per cluster regardless of the number of {{site.data.keyword.IBM_notm}} Support for Hyperledger Fabric services and containerized programs that are installed on the cluster.

## Manual integration of License Service
{: #metering-manual-integration}

For instructions on how to manually install the License Service see the
[IBM licensing operator readme file](https://github.com/IBM/ibm-licensing-operator/blob/master/README.md){: external}

## Validating if License Service is deployed on the cluster

To ensure license reporting continuity for license compliance purposes, you need to confirm that the license service is successfully deployed and periodically verify that it is active by logging in to the cluster and running the following command:

```
kubectl get pods --all-namespaces | grep ibm-licensing | grep -v operator
```
{: codeblock}

If it is successfully deployed and running, you see output similar to:
```
1/1 Running
```

## Archiving license information data before you decommission the cluster
{: #metering-archive}

Before you decommission your cluster where the licensing service is running, you need to archive the license metric utilization evidence. Retrieve the audit snapshot for the period when it was running on the cluster and store it for audit purposes.

For more information about the licensing solution, see the [License Service documentation](https://www.ibm.com/docs/en/cpfs?topic=service-license-1xx-operator){: external}.
