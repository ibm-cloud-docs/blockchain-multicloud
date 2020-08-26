---

copyright:
years: 2018, 2020
lastupdated: "2020-03-27"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes, Helm chart, behind a firewall

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

# About {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #console-icp-about}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud has been withdrawn from sale. To deploy the {{site.data.keyword.blockchainfull_notm}} Platform using the OpenShift Container Platform, {{site.data.keyword.cloud_notm}} Private, or open source Kubernetes, visit [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp).
{:important}

The {{site.data.keyword.blockchainfull}} Platform can be deployed across public and private clouds, such as the {{site.data.keyword.cloud_notm}}, your own data center, and third-party public clouds. This multicloud deployment is possible through {{site.data.keyword.cloud_notm}} Private, IBM's Kubernetes based container orchestration platform. This release provides a blockchain console user interface that you can use to deploy and manage blockchain components on an {{site.data.keyword.cloud_notm}} Private cluster. The {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private uses the Hyperledger Fabric v1.4.1 code base and supports deployment on {{site.data.keyword.cloud_notm}} Private v3.2.0.
{:shortdesc}

If you are looking for {{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.3) which can be deployed on Red Hat OpenShift Container Platform, Red Hat Open Kubernetes Distribution, {{site.data.keyword.cloud_notm}} Private V3.2.1 or any x86_64 Kubernetes v1.11 or high container platform,  see [Getting started with IBM Blockchain Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp).

## What {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private offers
{: #console-icp-about-offers}

You can use this offering to install the {{site.data.keyword.blockchainfull_notm}} Platform console on a deployment of {{site.data.keyword.cloud_notm}} Private. You can then use the console to create all of the fundamental components of a Hyperledger Fabric blockchain, a Certificate Authority, an ordering service, and peers, on your local cluster. For more information about the building blocks of Hyperledger Fabric networks, see the [blockchain component overview](/docs/blockchain-multicloud?topic=blockchain-multicloud-blockchain-component-overview#blockchain-component-overview). You can also use your console to operate a distributed multicloud network by importing nodes deployed on other {{site.data.keyword.cloud_notm}} Private clusters or on {{site.data.keyword.cloud_notm}}.

The {{site.data.keyword.blockchainfull_notm}} Platform includes the following key features:

**BUILD ---- Integrated developer experience**
- **Easily code** your smart contracts in Node.js, Golang, Java, or JavaScript, write client applications using the new {{site.data.keyword.blockchainfull_notm}} VS Code extension, leverage **SDK integration** with the console, and learn from our rich tutorials and samples.
- **Simplified DevOps** allows you to move from development to test to production in a single environment by scaling up your Kubernetes resources to add more components.
- **Up-to-date Fabric key features**. Leverage the latest features of Hyperledger Fabric v1.4.6:
  - [Raft ordering service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Private data collections](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) that provide increased data privacy by ensuring that ledger data is shared to only authorized peers via the gossip protocol.
  - [Fabric Node OUs](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html#node-ou-roles-and-msps){: external}
  - [Service discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, allowing you to dynamically discover and update how your application interacts with your network.
  - [Channel access control lists](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} that allow you additional control the governance of your channels and smart contracts.

**OPERATE --- Total control of your deployments**
- **Host or join a network**. Deploy peers that are hosted in your cluster to multiple channels on multiple clouds, or invite other organizations to join your consortium or channels while the organizations manage their nodes independently across infrastructures.
- **Maintain complete control of your identities**. Store and manage the keys that are used to administer your nodes. Optionally, use an [Hardware Security Module (HSM)](#x6704988){: term} to generate and store the private key of your nodes.
- **Run Anywhere**. Thanks to the **unified codebase** of the {{site.data.keyword.blockchainfull_notm}} Platform console, it is possible to run your components on any environment supported by {{site.data.keyword.cloud_notm}} and third-party public clouds.
- **Unified operation**. The {{site.data.keyword.blockchainfull_notm}} Platform console allows you to deploy and manage all of your organizations and nodes in **one console**. You can also add or remove members from a blockchain consortium, create and join channels, and install and instantiate smart contracts from your console.
- **Dynamic signature collection** that allows better control over collaborative governance over channel configurations.
- **Manage access** of the users who can administer or monitor your nodes.
- **Direct access to the logs** of your nodes from your {{site.data.keyword.IBM_notm}} Kubernetes service. Use the {{site.data.keyword.la_full_notm}} or any supported third-party service to extract and analyze your logs.
- **Kubernetes service integration.** Leverage services such as LogDNA for logging and Prometheus and Sysdig for monitoring. Leverage the built-in {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.cloud_notm}} Kubernetes Service Dashboard, {{site.data.keyword.la_full_notm}}, and {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).

**GROW --- Scalability and flexibility**
- **Choose your compute**. You have the flexibility to decide the amount of CPU, memory, and storage you want to provision in your Kubernetes cluster. For more information, see [Allocating resources](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-govern-components#ibp-console-govern-components-allocate-resources).
- **Scale** up and down the resources in your Kubernetes cluster, paying for only what you need.
- **Disaster recovery and multi-region high availability (HA).** This option duplicates your Kubernetes deployment across zones and  regions, enabling high availability (HA) of your components and disaster recovery (DR).
- **Connect to other Fabric networks**: Join {{site.data.keyword.blockchainfull_notm}} Platform peers to any network running Hyperledger Fabric components. Similarly, you can invite Fabric peers to join channels hosted on an ordering service deployed on the {{site.data.keyword.blockchainfull_notm}} Platform. Note that you will need to use Hyperledger Fabric APIs or the CLI.


{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private is a bundled product for {{site.data.keyword.cloud_notm}} Private and is delivered as a Kubernetes Helm Chart. For more information about {{site.data.keyword.cloud_notm}} Private, see the documentation for [{{site.data.keyword.cloud_notm}} Private version 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Is {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private suitable for you
{: #console-icp-about-suitable}

Running the {{site.data.keyword.blockchainfull_notm}} Platform outside of {{site.data.keyword.cloud_notm}} provides you with more flexibility to grow or join a blockchain network. It helps network initiators grow their networks by allowing new members to join while using the platform of their choice. It will allow organizations that are interested in joining blockchain networks to collocate their peers with their existing applications or to integrate with their systems of record.

Users of this offering will manage their own security and infrastructure. {{site.data.keyword.cloud_notm}} doesn't provide those services. Review the [Considerations and limitations](#console-icp-about-considerations) in the next section before you begin.

## Considerations and limitations
{: #console-icp-about-considerations}

Before you begin, ensure that you understand the following limitations:

- You are responsible for the management of health monitoring, security, logging, and resource usage of your blockchain components.
- The console can only be used to create and govern components based on Hyperledger Fabric v1.4.1 or higher.
- You can only deploy one console peer Kubernetes namespace. If you plan to create multiple blockchain networks, for example to create different environments for development, staging, and production, you should create a unique namespace for each environment.
- You can only import nodes that have been exported from other {{site.data.keyword.blockchainfull_notm}} Platform consoles. In order to be able to operate an imported peer or ordering node from the console, you also need to import the associated node's organization MSP definition and administrator identity into your console.
- {{site.data.keyword.blockchainfull_notm}} Platform is only supported on {{site.data.keyword.cloud_notm}} Private v3.2 Enterprise Edition.  {{site.data.keyword.cloud_notm}} Private v3.2 Community Edition is not supported.
- You cannot use {{site.data.keyword.IBM_notm}} Multicloud Manager to install the {{site.data.keyword.blockchainfull_notm}} Platform Helm Chart.

For a look at the environments supported by {{site.data.keyword.cloud_notm}} Private, see [Supported environments](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## System prerequisites
{: #console-icp-about-prerequisites}

See [System requirements](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external} for the list of hardware and software prerequisites for {{site.data.keyword.cloud_notm}} Private. Note however that the {{site.data.keyword.blockchainfull_notm}} Platform is not supported on Linux on Power (ppc64le) POWER8 systems.

The {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm chart has been validated to run on {{site.data.keyword.cloud_notm}} Private v3.2 clusters on Ubuntu Linux by using the following worker nodes and backing storage:

- **LinuxONE and {{site.data.keyword.IBM_notm}} Z**: z/VM and KVM, which are using NFS.
- **x86**: Linux 64-bit that uses GlusterFS.

## License
{: #console-icp-about-license}

See [Pricing for {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-software-pricing) for more information on licensing and pricing.

## Installing {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private is delivered as a Helm chart file that can be installed in a local {{site.data.keyword.cloud_notm}} Private cluster. After you install the Helm chart, you can find {{site.data.keyword.blockchainfull_notm}} Platform as an application in the {{site.data.keyword.cloud_notm}} Private Catalog. For instructions on how to install the Helm chart and the necessary prerequisites, see [Installing {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-helm-install).

If you are a new user of {{site.data.keyword.cloud_notm}} Private and would like information and tips on installing and deploying {{site.data.keyword.cloud_notm}} Private, see [Setting up {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-icp-console-setup#icp-console-setup).

You can deploy the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall, without having access to the public Internet. The downloaded Helm chart package includes all of the Fabric component Docker images that {{site.data.keyword.blockchainfull_notm}} Platform uses, without pulling them from DockerHub during deployment.

## Security Considerations
{: #console-icp-about-security}

Because these components are deployed on your own infrastructure, you are responsible for managing their security. This includes important areas of security, such as key management and data encryption. Review the following topics when you consider security for your components.

### Data security
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} uses whole disk encryption that is based on [symmetric key encryption](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} to protect all the data created by each service instance. You must take similar steps in your own environment to protect your peer data.

The data in your state database, whether you use LevelDB or CouchDB, is not encrypted. You can use application level encryption to protect the data at rest in your state database.

### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-about-data-residency#console-icp-about-data-residency).

### Key management
{: #console-icp-about-security-key-management}

Key management is a critical aspect of security. If a private key is compromised or lost, hostile actors might be able to access your data and functionality. {{site.data.keyword.IBM_notm}} uses physical appliances that are called [Hardware Security Modules](/docs/blockchain-multicloud?topic=blockchain-multicloud-glossary#glossary-hsm) (HSM) to store the private keys of {{site.data.keyword.blockchainfull_notm}} Platform Enterprise Plan networks.

You are responsible for managing your private keys. Although you can use the {{site.data.keyword.blockchainfull_notm}} Platform console UI to generate private keys, those keys are not stored by the console or within {{site.data.keyword.cloud_notm}} Private. It is essential to store your keys securely so that they are not compromised.

You can use Key Escrow to recover lost private keys. This needs to be performed prior to the loss of any keys. If a private key cannot be recovered, you need to get new private keys by registering a new identity with your Certificate Authority. You should also remove and replace your signCert from any channels that you joined.

### TLS
{: #console-icp-about-security-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) is embedded in the trust model of Hyperledger Fabric. All {{site.data.keyword.blockchainfull_notm}} Platform components use TLS to authenticate and communicate with each other. Therefore, nodes on {{site.data.keyword.cloud_notm}} Private need to be able to complete a TLS handshake with other components and your applications. One implication of this is that you need to enable passthru, by using an allowlist for example, in your firewall from client apps to your nodes.

### Application security
{: #console-icp-about-security-appl}

Because all chaincode invocations are signed, Fabric manages the application security. In addition, Fabric also includes ACL-based application level checks.

## Getting support
{: #console-icp-about-support}

For more information about how to get support on {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud, as well as free blockchain developer resources and support forums that you can use to troubleshoot problems, see [Getting support](/docs/blockchain-multicloud?topic=blockchain-multicloud-blockchain-support#blockchain-support).

If you have purchased an {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private license and you want to contact Customer support, see the information about [accessing the {{site.data.keyword.IBM_notm}} Support Community and opening a support ticket](https://www.ibm.com/support/pages/support-ibm-blockchain-platform-ibm-cloud-private-ibp-icp){: external}.
