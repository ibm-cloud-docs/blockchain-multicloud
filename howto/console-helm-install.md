---

copyright:
  years: 2017, 2020
lastupdated: "2019-07-25"

keywords: IBM Cloud Private, IBM Blockchain Platform, install, Helm chart, PodSecurityPolicy

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Installing the {{site.data.keyword.blockchainfull_notm}} Platform Helm chart
{: #console-helm-install}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud has been withdrawn from sale. To deploy the {{site.data.keyword.blockchainfull_notm}} Platform using the OpenShift Container Platform, {{site.data.keyword.cloud_notm}} Private, or open source Kubernetes, visit [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp).
{:important}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private is delivered as a Helm chart that can be installed on a local {{site.data.keyword.cloud_notm}} Private cluster. After you install the Helm chart, you can find {{site.data.keyword.blockchainfull_notm}} Platform as an application in the {{site.data.keyword.cloud_notm}} Private Catalog.

The Helm chart must be purchased through [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}. Upon your purchase, technical support for {{site.data.keyword.blockchainfull_notm}} Platform is included.

Before you install {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private, review the [Considerations and limitations](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-about#console-icp-about-considerations). For more information about pricing, support, and security and data residency considerations, see [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-about#console-icp-about).

## Prerequisites for installing the Helm chart
{: #console-helm-install-prereqs}

Before you install the Helm chart, you must have configured an {{site.data.keyword.cloud_notm}} Private cluster and created a new target namespace that is bound to a pod security policy. Review the instructions for [setting up and configuring an {{site.data.keyword.cloud_notm}} Private cluster](/docs/blockchain-multicloud?topic=blockchain-multicloud-icp-console-setup#icp-console-setup). You can only deploy one console per namespace. If you plan to create multiple blockchain networks, for example to create different environments for development, staging, and production, you should create a unique namespace for each environment.

### PodSecurityPolicy Requirements
{: #console-helm-install-prereqs-pod-security-requirements}

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

## Importing the Helm chart to {{site.data.keyword.cloud_notm}} Private
{: #console-helm-install-importing}

1. Download the Helm chart of {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private from [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}.

2. If you haven't already, log in to your {{site.data.keyword.cloud_notm}} Private cluster and select the target namespace of your deployment.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. Ensure that the [Docker CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html) is configured. After you configure the Docker CLI, access the image registry on your cluster by using the following command:
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. Import Helm chart by using the command line. From the directory where you stored the downloaded Helm chart package from PPA, run the following command in the {{site.data.keyword.cloud_notm}} Private CLI to import the Helm chart into your {{site.data.keyword.cloud_notm}} Private cluster.

  ```
  cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  Replace the following values:
  - `<archive-name>` with the name of the downloaded `.tgz` file.
  - `<cluster_CA_domain>:8500` with domain that you use to log in to your {{site.data.keyword.cloud_notm}} Private cluster.
  - `<repo-name>` with the Helm repository where you want to upload the chart.

    You create a new repository for your {{site.data.keyword.blockchainfull_notm}} Platform Helm carts, ie "blockchain-repo", by providing a new repo name to the install command when you are issuing the command for the first time. You will then be able to find the new repository on your cluster by running `cloudctl catalog repos`. After creating the repository, select the same repository when you are installing your Helm chart on additional namespaces.

  When this command completes successfully, you can see something that is similar to the following information:

  ```  
  Uploading Helm chart(s)
   Processing chart: charts/ibm-blockchain-platform-prod-1.1.0.tgz
   Updating chart values.yaml
   Uploading chart
  Loaded Helm chart
  OK

  Synch charts
  OK

  Archive finished processing
  ```  
  </details>

Click the **Catalog** button in the {{site.data.keyword.cloud_notm}} Private console, and then click **Blockchain** in the left navigation panel. If the import was successful, the **ibm-blockchain-platform-prod** tile will be visible on the {{site.data.keyword.cloud_notm}} Private Catalog page.

## Next steps
{: #console-helm-install-next-steps}

After you install the Helm chart, you can use the **ibm-blockchain-platform-prod** tile in your {{site.data.keyword.cloud_notm}} Private Catalog to deploy the {{site.data.keyword.blockchainfull_notm}} Platform console. You need to create a new target namespace for the deployment and ensure that your cluster has sufficient resources for your {{site.data.keyword.blockchainfull_notm}} Platform components before you complete the configuration page. For more information, see [Deploying the {{site.data.keyword.blockchainfull_notm}} console on {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-deploy-icp#console-deploy-icp).
