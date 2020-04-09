---

copyright:
  years: 2018, 2020
lastupdated: "2020-04-09"

subcollection: blockchain-multicloud

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud has been withdrawn from sale. To deploy the {{site.data.keyword.blockchainfull_notm}} Platform using the OpenShift Container Platform, {{site.data.keyword.cloud_notm}} Private, or open source Kubernetes, visit [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp).
{:important}

The {{site.data.keyword.blockchainfull}} Platform can be deployed across public and private clouds, such as the {{site.data.keyword.cloud_notm}}, your own data center, and third-party public clouds. This multicloud deployment is possible through {{site.data.keyword.cloud_notm}} Private, {{site.data.keyword.IBM_notm}}'s Kubernetes based container orchestration platform. You can use this offering to install the {{site.data.keyword.blockchainfull_notm}} Platform console on a deployment of {{site.data.keyword.cloud_notm}} Private and then use the console to create {{site.data.keyword.blockchainfull_notm}} components on your local cluster. You can also use your console to operate a distributed multicloud network by importing nodes deployed on other {{site.data.keyword.cloud_notm}} Private clusters or on {{site.data.keyword.cloud_notm}}.
{:shortdesc}

**Target audience**: This topic is designed for system administrators who are responsible for deploying {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}} Private V3.2.0.

If you are interested in learning more about how to use {{site.data.keyword.blockchainfull_notm}} Platform on Red Hat OpenShift Container Platform, Red Hat Open Kubernetes Distribution, {{site.data.keyword.cloud_notm}} Private V3.2.1 or any Kubernetes v1.14 - v1.16 container platform on x86_64 or LinuxONE s390x, see [Getting started with IBM Blockchain Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp).

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private is a bundled product for {{site.data.keyword.cloud_notm}} Private. For more information about {{site.data.keyword.cloud_notm}} Private, see the documentation for [{{site.data.keyword.cloud_notm}} Private version 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Step one: Set up a Kubernetes cluster on {{site.data.keyword.cloud_notm}} Private
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

The first step is to setup a Kubernetes cluster on {{site.data.keyword.cloud_notm}} Private on the cloud platform of your choice.
You can visit [Setting up {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-icp-console-setup#icp-console-setup) for recommendations and guidance.

If you are building a network that will be used in production, you need setup your cluster deployment and your blockchain network for High Availability:

- For recommendations on configuring your cluster, visit [Implement high availability on {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}.
- The following table includes the default resource allocations for each node type for consideration when you estimate the size of cluster that is required. When you deploy your nodes, you can adjust the resource allocations according to your needs.



| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       | 1.1           | 2.8                   | 200 (includes 100GB for peer and 100GB for state database)|
| **CA**                         | 0.1           | 0.2                   | 20                     |
| **Ordering node**              | 0.35          | 0.7                   | 100                    |



{: caption="Table 1.Default resource allocations" caption-side="bottom"}

## Step two: Install the {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private is delivered as a Helm chart that can be downloaded from Passport Advantage (PPA). To learn how to install the Helm chart on your local cluster, visit [Installing {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-helm-install#console-helm-install).

## Step three: Deploy the {{site.data.keyword.blockchainfull_notm}} Platform console
{: #get-started-console-icp-step-three-deploy-console}

After you have installed the Helm chart, you can click the {{site.data.keyword.blockchainfull_notm}} Platform application tile on the Catalog page to install an {{site.data.keyword.blockchainfull_notm}} Platform console on your local cluster. To learn about how to configure the console, as well as the resources that are required by your blockchain components, see [Deploying the {{site.data.keyword.blockchainfull_notm}} Platform console](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-deploy-icp#console-deploy-icp).

## Step four: Add users to the console as the administrator
{: #get-started-console-icp-step-four-add-console-admin}

The console administrator can login to the console using the email address and password that was provided during deployment. The password that was provided becomes the default password of the console, and is used by all new users to login to the console for the first time. The administrator can then add new users to the console, allowing others to login and start working with {{site.data.keyword.blockchainfull_notm}} nodes. The administrator can also set a new default password. To learn more, see [Managing users from the console](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-manage#console-icp-manage-users).

## Step five: Use the console to create your components
{: #get-started-console-icp-build-network}

Once you have deployed the console, you can use it to create, operate and govern {{site.data.keyword.blockchainfull_notm}} components on your local cluster. To get started using the console UI, visit the [Building a network tutorial](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-build-network#ibp-console-build-network).


## Step six: Connect networks across clouds
{: #get-started-console-icp-import-nodes}

You can use the console to operate components that are running on other {{site.data.keyword.cloud_notm}} Private clusters or on {{site.data.keyword.cloud_notm}}. First, you will need to export the component information to a JSON file from the console where the component was originally deployed. Then, you can import the node JSON file into the console that is deployed on your local cluster and manage the components across clouds. For more information, see [Importing nodes](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-import-nodes#ibp-console-import-nodes).
