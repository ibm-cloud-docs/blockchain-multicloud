---

copyright:
  years: 2019, 2020
lastupdated: "2020-04-01"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain-multicloud

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:pre: .pre}


# FAQs
{: #ibp-v2-faq}


**General**   

- [What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [How can I find what version of the {{site.data.keyword.blockchainfull_notm}} Platform that I am running?](#ibp-v2-faq-version)
- [What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [What database do the peers use for their ledger?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [What languages are supported for smart contracts?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Do you support using certificates from non-IBM Certificate Authorities (CAs)?](#ibp-v2-faq-v2-external-certs)
- [Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?](#ibp-v2-faq-multicloud)
- [How can I find the examples and tutorials within the VSCode extension?](#ibp-v2-faq-vscode-tutorials)
- [Is there a best practice for monitoring my blockchain resources?](#ibp-v2-faq-mon-res)
- [If service discovery is on, will an endorsement request be routed to any peer on the network?](#ibp-v2-faq-service-discovery)


**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [What are the benefits of {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-benefits)
- [Which environments does {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud support?](#ibp-v2-faq-icp-environments)
- [What is the pricing model for {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-pricing)
- [What services do I need to install to use {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-services)
- [How can I estimate the {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud sizing requirements for my dev, test, and production environments?](#ibp-v2-faq-icp-sizing)
- [What happens to my blockchain components when I delete my Helm release?](#ibp-v2-faq-icp-delete)


## What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

Hyperledger Fabric is a powerful, versatile, pluggable, open source, distributed ledger technology capable of addressing a wide variety of use cases across many industries. {{site.data.keyword.blockchainfull_notm}} Platform is built on top of Fabric and includes integrated tools that provide end to end features for developers and network operators to develop, test, operate, monitor, and govern Hyperledger Fabric components by using an intuitive console UI. Quickly deploy an instance and use the streamlined console UI to [build a network](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-build-network), easily [install and instantiate smart contracts](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-smart-contracts), [govern your components](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-govern-components), and [govern your channel](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-govern). Interested in APIs? See the [{{site.data.keyword.blockchainfull_notm}} Platform API reference](https://cloud.ibm.com/apidocs/blockchain){: external}. With the {{site.data.keyword.blockchainfull_notm}} Platform, it is easy to extend a basic network, work with multicloud solutions, and receive {{site.data.keyword.IBM_notm}} worldwide support when needed. Finally, the {{site.data.keyword.blockchainfull_notm}} Platform provides additional security benefits that are essential for running an enterprise-grade production network.



## How can I find what version of the {{site.data.keyword.blockchainfull_notm}} Platform that I am running?
{: #ibp-v2-faq-version}
{: faq}
{: support}

View the Support page by clicking the question mark icon <img src="../images/support-link.png" alt="Support link icon" width="30" style="width:30px; border-style: none"/> in the upper right corner of the page. The {{site.data.keyword.blockchainfull_notm}} Platform version is visible under the page heading.

## What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}


{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} uses Hyperledger Fabric v1.4.6.

## What database do the peers use for their ledger?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}
{: support}

You have the choice of either CouchDB or LevelDB when you configure your peer database. Because data is modeled differently in a Couch database than in a Level database, the peers in a channel must all use the same database type. See [LevelDB versus CouchDB](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-govern-components#ibp-console-govern-components-level-couch) to decide what is best for your business needs.

## What languages are supported for smart contracts?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}
{: support}

The {{site.data.keyword.blockchainfull_notm}} Platform supports smart contracts that are written in  Node.js, Golang, or JavaScript, and Java. The new Hyperledger Fabric programming model currently supports JavaScript, TypeScript, Java, and Go. If you are interested in preserving your existing application code, or by using Fabric SDKs for *Go*, you can still connect to your {{site.data.keyword.blockchainfull_notm}} Platform network by using the lower-level Fabric SDK APIs.

## Do you support using certificates from non-IBM Certificate Authorities?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Yes, you can bring your own certificates if they are issued by a CA that is X.509 compliant. The CA should sign by using ECDSA and the defaults should be set to use P256 curve. See this topic about [Using certificates from an external CA with your peer or ordering node](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-govern-components#ibp-console-govern-third-party-ca).

## Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?
{: #ibp-v2-faq-multicloud}
{: faq}

You can not currently deploy blockchain nodes to multiple hosted cloud providers. However, you can use your console to operate a distributed multicloud network by importing nodes deployed by using consoles on other clouds.

## How can I find the examples and tutorials within the VSCode extension?
{: #ibp-v2-faq-vscode-tutorials}
{: faq}

The {{site.data.keyword.blockchainfull_notm}} Platform extension provides guided tutorials within VS Code to help you get started. You can find these tutorials on the extension home page when the extension is first installed. However, you can return to the tutorials and the home page by going to the extensions tab. For more information, see [Guided tutorials in VS Code](/docs/blockchain-multicloud?topic=blockchain-multicloud-develop-vscode#develop-vscode-guided-tutorials).

## Is there a best practice for monitoring my blockchain resources?
{: #ibp-v2-faq-mon-res}
{: faq}
{: support}

You are responsible for the health monitoring and resource allocation of the blockchain nodes in your Kubernetes cluster. While requests against the nodes are being actively processed, you should be monitoring for spikes in resource consumption to avoid problems.  {{site.data.keyword.IBM_notm}} recommends that you configure {{site.data.keyword.mon_full_notm}} and setup alerts to track when blockchain nodes are reaching their limits.

You should be aware that JavaScript and TypeScript smart contracts require more resources than contracts written in Go. Therefore, when you are allocating resources to your peer node, it is important to allocate sufficient resources and monitor the peer containers to ensure adequate resources are available to the peer when smart contracts are instantiated on a channel and during transaction processing.
{: tip}

## If service discovery is on, will an endorsement request be routed to any peer on the network?
{: #ibp-v2-faq-service-discovery}
{: faq}

Yes, if you have the endorsement policy set to “ANY”. However, you do have the opportunity to bind the policy directly to an organization's peers. The service discovery information provided by the peer supplies two pieces of information, `Layouts` and `EndorsersByGroup`. With these two pieces of data, the SDK has the ability to send requests to peers in different organizations that meet the endorsement policy requirements. The node.js SDK provides default code that uses the `Layouts` and `EndoresersByGroup` and sends the requests to the appropriate peers to meet the endorsement policy requirements. This existing logic can be customized to meet the business needs.

## What are the benefits of {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-benefits}
{: faq}

See [What the {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud offers](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-about#console-icp-about-offers).

## Which environments does {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud support?
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud supports all environments that {{site.data.keyword.cloud_notm}} Private v3.2 supports. See [Supported operating systems and platforms](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external} for more information.

## What is the pricing model for {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-pricing}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud [licensing](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-software-pricing) is based on the amount of CPU (VPC) made available to the product. For details, [contact IBM](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## What services do I need to install to use {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-services}
{: faq}

You need to install only {{site.data.keyword.cloud_notm}} Private v3.2.

## How can I estimate the {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud sizing requirements for my dev, test, and production environments?
{: #ibp-v2-faq-icp-sizing}
{: faq}

After you understand how many CAs, peers, and ordering nodes are required, you can examine the [default resource allocations table](/docs/blockchain-multicloud?topic=blockchain-multicloud-icp-console-setup#icp-console-setup-resources) to get an approximate estimate of the CPUs (VPCs) required for your network.

## What happens to my blockchain components when I delete my Helm release?
{: #ibp-v2-faq-icp-delete}
{: faq}

When you delete a helm release from your {{site.data.keyword.cloud_notm}} Private cluster, the associated blockchain components are not deleted. To properly remove a helm release from your cluster, delete all of your components by using the blockchain console or the blockchain APIs first. Then, you can delete the helm chart.-->
