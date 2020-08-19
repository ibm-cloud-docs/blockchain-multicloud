---

copyright:
  years: 2018, 2020
lastupdated: "2019-07-25"

keywords: IBM Cloud Private, data storage CA, cluster ICP, configuration

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

# Setting up {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud has been withdrawn from sale. To deploy the {{site.data.keyword.blockchainfull_notm}} Platform using the OpenShift Container Platform, {{site.data.keyword.cloud_notm}} Private, or open source Kubernetes, visit [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp).
{:important}

Before you deploy {{site.data.keyword.blockchainfull}} Platform components and build blockchain network on {{site.data.keyword.cloud_notm}} Private, you need to set up {{site.data.keyword.cloud_notm}} Private in your own environment.
{:shortdesc}

## Prerequisites
{: #icp-console-setup-prerequisites}

Complete the following prerequisites and prepare your environment to install {{site.data.keyword.cloud_notm}} Private.

### Docker
{{site.data.keyword.cloud_notm}} Private requires Docker to be installed. Follow the related instructions in [Installing {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external} to install Docker.

### {{site.data.keyword.cloud_notm}} Private settings
Before you install {{site.data.keyword.cloud_notm}} Private, the following tips are useful to prepare your nodes for {{site.data.keyword.cloud_notm}} Private installation. Additional {{site.data.keyword.cloud_notm}} Private prerequisites can be found in the [{{site.data.keyword.cloud_notm}} Private documentation](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}.

#### Update `vm.max_map_count` setting
{{site.data.keyword.cloud_notm}} Private uses Elastic Search for logging and metering. To avoid out-of-memory exceptions, Elastic Search requires the `vm.max_map_count` system property to be configured. Before you install {{site.data.keyword.cloud_notm}} Private, see the [Elastic Search configuration instructions](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external} to configure this property on each node. You can use the following commands to set this property permanently:

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### Configure the `/etc/hosts` file on each node in your cluster

- {{site.data.keyword.cloud_notm}} Private uses [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} to manage containerized applications. The Kubernetes Domain Name Server (DNS) fails if host names are not configured in the `/etc/hosts` file on each node. [Insert the IP address, hostname, and shortname of each node in your cluster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external} into the `/etc/hosts` file on each node.

- [IPv6 is not supported by {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}. To avoid problems with the DNS service in an {{site.data.keyword.cloud_notm}} Private cluster, disable the IPv6 settings in the `/etc/hosts` file on each node by commenting out the following line with a `#` sign at the beginning of the line:
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## Resources required
{: #icp-console-setup-resources}

Ensure that your {{site.data.keyword.cloud_notm}} Private system meets the minimum hardware resource requirements for the console and the components you create. The number of vCPU/CPUs that are required can vary depending on your infrastructure, network design and performance requirements. An approximation of your vCPU/CPU requirements can be made by examining the [default resource allocations table](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-govern-components#ibp-console-govern-components-allocate-resources) for {{site.data.keyword.cloud_notm}}, although the allocations are slightly different in {{site.data.keyword.cloud_notm}} Private. Your actual resource allocations are visible in your blockchain console when you deploy a node.

**Notes:**  

- A vCPU is a virtual core that is assigned to a virtual machine or a physical processor core if the server is not partitioned for virtual machines. You need to consider vCPU requirements when you decide the virtual processor core (VPC) for your deployment in {{site.data.keyword.cloud_notm}} Private. VPC is a unit of measurement to determine the licensing cost of {{site.data.keyword.IBM_notm}} products. For more information about scenarios to decide VPC, see [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
- A vCPU is equivalent to one CPU, which is equivalent to one VPC.

### Storage considerations
{: #icp-console-setup-storage-considerations}

The {{site.data.keyword.blockchainfull_notm}} Helm chart uses dynamic provisioning to provision the storage that will be used by the console and the blockchain components you will create. Before you deploy the console, you need to create a storageClass with a sufficient amount of backing storage for your console and your components. You need to provide the name of the storageClass you created during configuration.

- If you use the default settings, the Helm chart will create a new Persistent Volume claim with the name of Helm release for your console data.
- If you use NFS v2/v3 Persistent Volumes, you must enable the **NFS status monitor for NFSv2/v3 Filesystem Locks** module, which is also known as **rpc-statd**, on the host system where the NFS file system exists. This module allows your NFS file system to check for exclusive locks on files that other processes hold. Run the following commands to enable this module:

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## Installing {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup-install}

Complete the following steps to install and set up {{site.data.keyword.cloud_notm}} Private in your environment.

1. Install an [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} cluster at v3.2.0.

2. Install the {{site.data.keyword.cloud_notm}} Private CLI [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external} to install the Helm chart.

3. Create a new, custom namespace for your {{site.data.keyword.blockchainfull_notm}} Platform deployment. Note that you can only deploy one Helm chart per namespace, so if you want multiple instances of the console to run on the same cluster, be sure to use separate namespaces.

4. Setup the security and access policies for the target namespace. Instructions are provided in the [next section](#icp-console-setup-psp).

After you install {{site.data.keyword.cloud_notm}} Private and bind a pod security policy to a target namespace, you can continue to [import the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm chart](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-helm-install#console-helm-install) into your {{site.data.keyword.cloud_notm}} Private cluster.

## PodSecurityPolicy Requirements
{: #icp-console-setup-psp}

The {{site.data.keyword.blockchainfull_notm}} Platform Helm chart requires specific security and access policies be bound to the target namespace prior to installation. We provide YAML files defining the policies in the steps below. You can save these files to your local system and then bind them your namespace using the {{site.data.keyword.cloud_notm}} Private CLI. Follow the steps below prior to deploying the {{site.data.keyword.blockchainfull_notm}} Platform Helm chart.

1. Save the file below that defines the {{site.data.keyword.blockchainfull_notm}} Platform PodSecurityPolicy as `ibm-blockchain-platform-psp.yaml` on your local system:

    ```
    apiVersion: extensions/v1beta1
    kind: PodSecurityPolicy
    metadata:
      name: ibm-blockchain-platform-psp
    spec:
      hostIPC: false
      hostNetwork: false
      hostPID: false
      privileged: true
      allowPrivilegeEscalation: true
      readOnlyRootFilesystem: false
      seLinux:
        rule: RunAsAny
      supplementalGroups:
        rule: RunAsAny
      runAsUser:
        rule: RunAsAny
      fsGroup:
        rule: RunAsAny
      allowedCapabilities:
      - NET_BIND_SERVICE
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - SETUID
      - FOWNER
      volumes:
      - '*'
    ```
    {:codeblock}

2. Save the file below that defines the required ClusterRole for the PodSecurityPolicy as `ibm-blockchain-platform-clusterrole.yaml`:

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      annotations:
      name: ibm-blockchain-platform-clusterrole
    rules:
    - apiGroups:
      - extensions
      resourceNames:
      - ibm-blockchain-platform-psp
      resources:
      - podsecuritypolicies
      verbs:
      - use
    - apiGroups:
      - "*"
      resources:
      - pods
      - services
      - endpoints
      - persistentvolumeclaims
      - persistentvolumes
      - events
      - configmaps
      - secrets
      - ingresses
      - roles
      - rolebindings
      - serviceaccounts
      verbs:
      - '*'
    - apiGroups:
      - apiextensions.k8s.io
      resources:
      - persistentvolumeclaims
      - persistentvolumes
      - customresourcedefinitions
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      - ibpservices
      - ibpcas
      - ibppeers
      - ibpfabproxies
      - ibporderers
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      verbs:
      - '*'
    - apiGroups:
      - apps
      resources:
      - deployments
      - daemonsets
      - replicasets
      - statefulsets
      verbs:
      - '*'
    ```
    {:codeblock}

3. Save the file below that defines the ClusterRoleBinding as `ibm-blockchain-platform-clusterrolebinding.yaml`. Replace `<namespace>` with the name of the target namespace. If you decide to change the ServiceAccount name in the file below, you need to provide the name to the `Service account name` field in the **All Parameters** section of the configuration page when deploying the Helm chart.

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      name: ibm-blockchain-platform-clusterrolebinding
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: ibm-blockchain-platform-clusterrole
    subjects:
    - kind: ServiceAccount
      name: default
      namespace: <namespace>
    ```
    {:codeblock}

Once you have saved the PodSecurityPolicy, ClusterRole, and ClusterRoleBinding YAML files to your local system, a cluster administrator will need use the {{site.data.keyword.cloud_notm}} Private CLI to bind the policies to your namespace.

1. Log in to your {{site.data.keyword.cloud_notm}} Private cluster and select the target namespace of your deployment.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. Log in to the docker image registry for your cluster:

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. Use the following commands to apply the policies to your target namespace:

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. After applying the policies, you must grant your service account the required level of permissions to deploy your console. Run the following command with the name of your target namespace:

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```
