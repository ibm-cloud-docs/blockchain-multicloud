---

copyright:
  years: 2018, 2020
lastupdated: "2019-07-25"

keywords: IBM Cloud Private, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# Deploying the {{site.data.keyword.blockchainfull_notm}} console
{: #console-deploy-icp}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud has been withdrawn from sale. To deploy the {{site.data.keyword.blockchainfull_notm}} Platform using the OpenShift Container Platform, {{site.data.keyword.cloud_notm}} Private, or open source Kubernetes, visit [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp).
{:important}

Use the following instructions to deploy the {{site.data.keyword.blockchainfull}} Platform console on your local {{site.data.keyword.cloud_notm}} Private cluster after you have installed the {{site.data.keyword.blockchainfull_notm}} Platform Helm chart.
{:shortdesc}

## Resources required
{: #console-deploy-icp-resources-required}

Ensure that your {{site.data.keyword.cloud_notm}} Private system meets the minimum hardware resource requirements for the console and the components you create. The number of vCPU/CPUs that are required can vary depending on your infrastructure, network design and performance requirements. An approximation of your vCPU/CPU requirements can be made by examining the [default resource allocations table](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-govern-components#ibp-console-govern-components-allocate-resources) for {{site.data.keyword.cloud_notm}}, although the allocations are slightly different in {{site.data.keyword.cloud_notm}} Private. Your actual resource allocations are visible in your blockchain console when you deploy a node.

**Notes:**  

- A vCPU is a virtual core that is assigned to a virtual machine or a physical processor core if the server is not partitioned for virtual machines. You need to consider vCPU requirements when you decide the virtual processor core (VPC) for your deployment in {{site.data.keyword.cloud_notm}} Private. VPC is a unit of measurement to determine the licensing cost of {{site.data.keyword.IBM_notm}} products. For more information about scenarios to decide VPC, see [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
- A vCPU is equivalent to one CPU, which is equivalent to one VPC.

## Storage
{: #console-deploy-icp-storage}

The {{site.data.keyword.blockchainfull_notm}} Helm chart uses dynamic provisioning to provision the storage that will be used by the console and the blockchain components you will create. Before you deploy the console, you need to create a storageClass with a sufficient amount of backing storage for your console and your components. You need to provide the name of the storageClass you created during configuration.

- If you use the default settings, the Helm chart will create a new Persistent Volume claim with the name of Helm release for your console data.


## Prerequisites for deploying the console
{: #console-deploy-icp-prerequisites}

1. Before you can deploy the {{site.data.keyword.blockchainfull_notm}} Platform console, you must [install {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-icp-console-setup#icp-console-setup) and [install the {{site.data.keyword.blockchainfull_notm}} Platform Helm chart](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-helm-install).

2. You should create a new, custom namespace for your {{site.data.keyword.blockchainfull_notm}} Platform deployment. Your namespace needs to use the [required PodSecurityPolicy](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-helm-install#console-helm-install-prereqs-pod-security-requirements). If you plan to create multiple blockchain networks, for example to create different environments for development, staging, and production, you should create a unique namespace for each environment. Note that you can only deploy one Helm chart per namespace, so if you want multiple instances of the console to run on the same cluster, be sure to use separate namespaces.

3. Retrieve the value of the cluster Proxy IP address of your CA from the {{site.data.keyword.cloud_notm}} Private console. **Note:** You will need to be a [Cluster administrator](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external} to access your proxy IP. Log in to the {{site.data.keyword.cloud_notm}} Private cluster. In the left navigation panel, click **Platform** and then **Nodes** to view the nodes that are defined in the cluster. Click the node with the role `proxy` and then copy the value of the `Host IP` from the table. **Important:** Save this value and you will use it when you configure the `Proxy IP` field of the Helm chart.

4. Create an [image security policy](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-deploy-icp#console-deploy-icp-image-policy) that allows your deployment to download the required images from your cluster docker registry.

5. Create a password that you will use to login to the console for the first time and store it inside a secret object in {{site.data.keyword.cloud_notm}} Private. You can find the steps to create the secret in a [following section](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-deploy-icp#console-deploy-icp-password-secret).

6. Create an [imagePullSecret](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-deploy-icp#console-deploy-icp-imagepullsecret) to store the credentials that your deployment will use to access the docker registry. This step allows you to deploy the console to a non default namespace.

## Cluster Image Policy requirements
{: #console-deploy-icp-image-policy}

[Container image security](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_images/image_security.html) is enabled by default in ICP 3.2+. Therefore, you need to add the Docker Hub container registry that specified you when you installed the Helm chart to the list of trusted registries. Otherwise, your console deployment will not be able to access the images in that registry. Follow the steps below to create a new image policy:

1. Log in to the {{site.data.keyword.cloud_notm}} Private console. In the left navigation panel, click **Manage** and then **Resource Security**. Under the **Resource Security** menu, navigate to **Image Policies** and then click **Create Image Policy**.

2. In the **Policy Details** section, complete the following fields:
  - Enter a **Name** for the new image policy. For example: `ibp-imagepolicy`.
  - In **Scope**, select `namespace`.
  - In **Namespace**, select the namespace where you installed your Helm chart.   

3. In the **Policy Details** section, click **Add Registry** and provide the image registry that you specified when you [Installed the Helm chart](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-helm-install#console-helm-install-importing), followed by `/*`.
  - As an example, you can provide the registry of `<cluster_CA_domain>:8500/*`, or `<cluster_CA_domain>:8500/ibp/*` and `<cluster_CA_domain>:8500/op-tools/*`.

You can also add a new cluster image policy by using a YAML file and the kubectl command line tool. You can find an example YAML file below:

```
apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
kind: ClusterImagePolicy
metadata:
  name: ibp-imagepolicy
spec:
  repositories:
  - name: <cluster_CA_domain>:8500/ibp/*
  - name: <cluster_CA_domain>:8500/op-tools/*
```

## Creating a console password secret
{: #console-deploy-icp-password-secret}

Before you can access your console, you need to create a default password that you will use to login to the console for the first time. Create a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/){: external} to store the password prior to deploying the console. A Kubernetes secret allows you to protect and share information without having to pass it directly to the deployment.

1. Create a password and encode it in base64 format. Run the following command in a terminal window and replace the value of `password` with the value that you want to use. **Save output of this command**.
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. Log in to the {{site.data.keyword.cloud_notm}} Private console. In the left navigation panel, click **Configuration** and then **Secrets**. Click the **Create Secret** button to open a pop panel that allows you to generate a new secret object.

3. On the **General** tab, complete the following fields:
  - **Name:** Give your secret a unique name within your cluster. You will use this name when deploying your console. The name must be all lowercase.
  - **Namespace:** The name space to add your secret. Select the `namespace` that you want to deploy your console to.
  - **Type:** Enter the value `generic`.

4. Leave the **Annotations** tab blank.

5. In the **Data** tab, add the user name and password as key value pairs.
  1. In the first **Name** field, enter `password`.
  2. In the first **Value** field, enter the result of `echo -n 'password' | base64` from step 1.
  3. Click **Create** to form a new Secret object.

Provide the name of the secret to the `Console administrator password secret name` field of the configuration page when you deploy your console. You will need to use the value that you encoded in step one to login to the console for the first time. This value will become the default password of the console unless changed by a console administrator. For more information, see [Managing users from the console](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-manage#console-icp-manage-users).

## Creating an imagePullSecret
{: #console-deploy-icp-imagepullsecret}

You need to create a imagePullSecret to store the credentials that your deployment will use to access the docker registry. Completing this step allows you to deploy the {{site.data.keyword.blockchainfull_notm}} Platform console to a non default namespace. Use the instructions in the {{site.data.keyword.cloud_notm}} Private documentation to [create an imagePullSecret](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_images/imagepullsecret.html) and load the secret to the namespace you created to deploy the console.

Provide the name of the imagePullSecret to the `imagePullSecret name` field of the configuration page when you deploy the console.

If you do not want to create an imagePullSecret, you can also change the scope of the Hyperledger Fabric images used by your console. Log in to the {{site.data.keyword.cloud_notm}} Private console, open the menu and navigate to **Container Images**. Search for `hlfabric` to retrieve the list of Hyperledger Fabric images. Click on `namespace` in the table and change the scope from `namespace` to `global` for each of the nine images. This allows you to deploy the console to a non default namespace without creating an imagePullSecret.
{:note}

## Creating TLS secret (Optional)
{: #console-deploy-icp-tls-secret}

The console uses TLS to secure communication between your console and your blockchain nodes. By default, the console generates its own TLS certificates. However, you have the option of providing your own TLS certificate and key. Use the steps below to store the certificates in a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/). You can then provide those certificates to the console by providing the name of the secret to the TLS secret field during configuration.

1. Convert the TLS certificate and private key into base64 format. You can convert the contents of the certificate or key file into a base64 string by running the following command on your local machine:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  Run this command for both your TLS certificate and the accompanying TLS key. **Save** the output.

2. Log in to the {{site.data.keyword.cloud_notm}} Private console. In the left navigation panel, click **Configuration** and then **Secrets**. Click the **Create Secret** button to open a pop panel that allows you to generate a new secret object.

3. On the **General** tab, complete the following fields:
  - **Name:** Give your secret a unique name within your cluster. You will use this name for configuring your CA. The name must be all lowercase.
  - **Namespace:** The name space to add your secret. Select the `namespace` that you want to deploy your CA to.
  - **Type:** Enter the value `kubernetes.io/tls`.

4. Leave the **Annotations** tab blank.

5. In the **Data** tab, add the user name and password as key value pairs.
  1. In the first **Name** field, enter `tls.crt`.
  2. In the first **Value** field, enter the string of your base64 encoded TLS certificate from step 1.
  3. Click the **Add data** button to add a second key value pair.
  4. In the first **Name** field, enter `tls.key`.
  5. In the first **Value** field, enter the string of your base64 encoded TLS key from step 1.
  6. Click **Create** to form a new Secret object.

Provide the name of the secret you create to the `TLS secret` field in the all parameters section of the configuration page when you deploy your console.

Secrets are not removed from your {{site.data.keyword.cloud_notm}} Private cluster when you delete your Helm release. You are responsible for managing the password and TLS secrets in your {{site.data.keyword.cloud_notm}} Private cluster. If you plan to deploy another console in the future, you may reuse the secrets. Otherwise, you are responsible for deleting them from your {{site.data.keyword.cloud_notm}} Private cluster.
{:note}

## Configuration
{: #console-deploy-icp-configuration}

After you create the TLS secret object, you can deploy the console by using the following steps.

1. Log in to the {{site.data.keyword.cloud_notm}} Private console and click **Catalog** in the upper-right corner.
2. Click `Blockchain` in the left navigation panel and locate the tile that is labeled `ibm-blockchain-platform-prod`. Click the tile to open it. You should see a Readme file that includes information about installing and configuring the Helm chart.
3. Click the **Configuration** tab on the top of the panel or click the **Configure** button in the lower-right corner.
4. Specify the values for the [Configuration and pod security parameters](#icp-peer-deploy-configuration-parms) and accept the license agreement.
5. Navigate to the **Parameters** section:
- You can deploy the console by using only the [Quickstart parameters](#icp-peer-deploy-quickstart-parms). Use this option if you are experimenting or getting started. If you are deploying the helm chart to a non default namespace (recommended), you also need to complete the `imagePullSecret name` field in the [All parameters](#icp-peer-deploy-quickstart-parms) section.
- You can use the [All parameters](#icp-peer-deploy-quickstart-parms) section to customize network access, resources, and storage used by your console. The **All parameters** section is recommended for only more experienced Kubernetes users.
6. Click **Install**.

When you provide the namespace to the `namespace` field on the configuration field, you will see the following error: `Pod Security Conflict this chart requires a namespace with a ibm-privileged-psp pod security policy.` This error does not stop you from deploying the console and can be safely ignored.
{:note}

The following tables describe the configuration parameter fields and their default values.

### Configuration and Pod Security parameters
{: #icp-peer-deploy-configuration-parms}

|  Parameter     | Description    | Default  | Required |
| --------------|-----------------|-------|------- |
| `Helm release name`| Name to identify your helm release deployment. Begin with a lowercase letter, end with any alphanumeric character, and must contain only hyphens and lowercase alphanumeric characters. | None | Yes |
| `Target namespace`| Specify the namespace that you created for your console and components. The namespace must include an `ibm-privileged-psp` policy. Otherwise, [bind a PodSecurityPolicy](/docs/blockchain-multicloud?topic=blockchain-multicloud-icp-console-setup#icp-console-setup-psp) to your namespace. | None | Yes |
| `Target Cluster`| Specify the clusters where you would like to deploy the resource. Available clusters are filtered by the selected namespace and Kubernetes version requirements. | None | Yes |
| `Target namespace policies`| Pre-configured to use your target namespace. The values must include the `ibm-privileged-psp` or `ibm-blockchain-platform-psp` policy. | None | Yes |

### Quickstart and All Parameters
{: #icp-peer-deploy-quickstart-parms}

Use the quickstart section if you are experimenting or getting started. The All parameters section allows you customize network access, resources, and storage and is recommended for only more experienced Kubernetes users.

**Click the All Parameters tab to view all of the configuration parameters.**

|  Parameter     | Description    | Default  | Required |
| --------------|-----------------|-------|------- |
| **Console settings** | **Administration and deployment of your console** | | |
| `Proxy IP` | IP address of the proxy node of your cluster. | None| Yes |
| `Console administrator email` | Email used to log in to the console. | None | Yes |
| `Console administrator password secret name` | Name of the secret that you [created to store the password](#console-deploy-icp-password-secret) that you will use to login to the console. | None | Yes|
| **Network settings** | **Network access to your console** | | |
| `Console hostname` | Enter the same value as the Proxy IP. | None | Yes |
| `Console port` | Enter the port you would like to use in the range of 31210 - 31220. This port cannot be used by another application. | None | Yes |
| `Proxy hostname` | Proxy server hostname. Enter the same value as the Proxy IP. | None | Yes |
| `Proxy port` | Enter any port you would like to use in the range of 31210 - 31220. This port cannot be used by another application or the console. | None | Yes |
| **Storage settings** | **Storage to be used by your console** | | |
| `Storage class name` | Enter the name of the storage class to be used by the console and the components that you create. | None | Yes |
{: caption="Table 1. Quickstart Parameters" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Parameter     | Description    | Default  | Required |
| --------------|-----------------|-------|------- |
| `License` | Set to accept to indicate that you accept the terms of the license | Not accepted | Yes |
| `Architecture` | Select your cloud platform architecture. (AMD64 or S390x) | AMD64 | No |
| **Console settings** | **Administration and deployment of your console** | | |
| `Service account name` | Service account to be used by operator. | None | No |
| `Proxy IP` | IP address of the proxy node in your cluster. | None | Yes|
| `Console administrator email` | Email used to log in to the console. | None | Yes |
| `Console administrator password secret name` | Name of the secret that you [created to store the password](#console-deploy-icp-password-secret) that you will use to login to the console. | None | Yes|
| **Docker image settings** | **Use these settings to customize the Fabric images to be pulled by the console** | | |
| `imagePullSecret name` | Name of the [imagePullSecret](#console-deploy-icp-imagepullsecret) that you created| `ibp-ibmregistry` | No |
| **Network settings** | **Network access to your console** | | |
| `Console hostname` | Enter the same value as the proxy IP. | None | Yes |
| `Console port` | Enter any port you would like to use in the range of 31210 - 31220. | None | Yes |
| `Proxy hostname` | Proxy server hostname. Enter the same value as the Proxy IP. | None | Yes |
| `Proxy port` | Enter any port you would like to use in the range of 31210 - 31220. This port cannot be used by another application or the console. | None | Yes |
| `TLS secret` | Name of the secret that you [created to store the TLS certificates](#console-deploy-icp-tls-secret) that will be used by the console. | None | No |
| **Storage settings**| **Provision storage for your console and tools** | | |
| `Volume claim size`| Size of the Persistent Volume Claim to be provisioned. | 10Gi  | No |
| `Storage class name`| Name of the storage class to be used by the console and the components you create. | None | Yes |
| `Storage access mode`| Specify the storage [access mode](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) for the PVC.  | ReadWriteMany | No |
| **Allocate resources**| **Allocate resources to the console** | | |
| `Opstools CPU limit` | Maximum number of CPUs to allocate to the opstools component. | 500m | No |
| `Opstools memory limit` | Maximum amount of memory to allocate to the opstools component. | 1000Mi | No |
| `Opstools CPU request` | Minimum number of CPUs to allocate to the opstools component. | 500m | No |
| `Opstools memory request` | Minimum amount of memory to allocate to the opstools component.| 1000Mi | No |
| `Configtxlator CPU limit` | Maximum number of CPUs to allocate to the configtxlator tool. | 25m | No |
| `Configtxlator memory limit` | Maximum amount of memory to allocate to the configtxlator tool. | 50Mi | No |
| `Configtxlator CPU request` | Minimum number of CPUs to allocate to the configtxlator tool.| 25m | No |
| `Configtxlator memory request` | Minimum amount of memory to allocate to the configtxlator tool.| 50Mi | No |
| `CouchDB CPU limit` | Maximum number of CPUs to allocate to CouchDB. | 500m | No |
| `CouchDB memory limit` | Maximum amount of memory to allocate to CouchDB. | 1000Mi | No |
| `CouchDB CPU request` | Minimum number of CPUs to allocate to CouchDB.| 500m | No |
| `CouchDB memory request` | Minimum amount of memory to allocate to CouchDB.| 1000Mi | No |
| `Operator CPU limit` | Maximum number of CPUs to allocate to operator component. | 100m | No |
| `Operator memory limit` | Maximum amount of memory to allocate to operator component. | 200Mi | No |
| `Operator CPU request` | Minimum number of CPUs to allocate to operator component. | 100m | No |
| `Operator memory request` | Minimum amount of memory to allocate to operator component. | 200Mi | No |
| `Deployer CPU limit` | Maximum number of CPUs to allocate to deployer component. | 100m | No |
| `Deployer memory limit` | Maximum amount of memory to allocate to deployer component. | 200Mi | No |
| `Deployer CPU request` | Minimum number of CPUs to allocate to deployer component. | 100m | No |
| `Deployer memory request` | Minimum amount of memory to allocate to deployer component. | 200Mi | No |
{: caption="Table 2. All Parameters" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Using the Helm command line to install the Helm release
{: #console-deploy-icp-helm-cli}

Alternatively, you can use the `helm` CLI to install the Helm release. Before you run the `helm install` command, ensure that you [add your cluster's Helm repository to the Helm CLI environment](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}.

You can set the parameters required for installation by creating a `yaml` file and passing it to the following `helm install` command.
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

Where:
- `<helm_release name>` represents the name you want to give your helm release.
- `<helm_chart>` represents the name of the Helm chart that is imported into the catalog.
- `<helm_chart_version>` represents the version of the Helm chart that is imported into the catalog.
- `<customvalues.yaml>` is the name of the yaml file containing the configuration parameters.

For example:
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

You can create a new `yaml` file by editing `values.yaml` included in the downloaded archive file, which includes all the necessary parameters with their default settings.

## Verifying the installation

After you complete the configuration parameters and click the **Install** button, click the **View Helm Release** button to view your deployment. If it was successful, you should see the value 1 in the `DESIRED`, `CURRENT`, `UP TO DATE`, and `AVAILABLE` fields in the Deployment table. You may need to click refresh and wait for the table to be updated.

You view the details of your deployment by navigating to the **Deployment** overview screen, and clicking on the pod that was created. The Helm chart deployment creates five containers on your cluster:
- **optools**: The console UI.
- **deployer**: A tool that allows your console to communicate with your deployments.
- **configtxlator**: A tool used by the console to read and create channel updates.
- **couchdb**: An instance of CouchDB that stores the data from your console, including your authorization information.
- **operator**: A kubernetes operator that manages your component deployments.

## Logging into the console

You can use your browser to access the console after installation. You can find the URL in the **Notes:** section of the Helm release overview screen that opens after deployment. Check to make sure you are not using the ESR version of Firefox. If you are, switch to another browser such as Chrome and retry.

In your browser, you should be able to see the console login screen:
- For the **User ID**, use the value you provided for the `Console administrator email` field during configuration.
- For the **Password**, use the value you encoded and stored inside the [password secret](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-deploy-icp#console-deploy-icp-password-secret) and then passed to the console during configuration. This password will become the default password for the console that all new users use to login to the console. After you login for the first time, you will be asked to provide a new password that you can use to login to the console.

The administrator that provisioned the helm chart can grant other users access to the console and specify which operations they can preform. For more information, see [Managing users from the console](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-manage#console-icp-manage-users).

## Next steps
{: #console-deploy-icp-next-steps}

After accessing your console, you can view the **nodes** tab of your console UI. You can use this screen to deploy components in your local cluster. Visit the [Building a network tutorial](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-build-network#ibp-console-build-network) to get started using the console. You can also use this tab to operate nodes that have been created on other clouds. For more information, see [Importing nodes](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-import-nodes#ibp-console-import-nodes).

To learn how to manage the users that can access the console, use the {{site.data.keyword.blockchainfull_notm}} Platform APIs, and view the logs of your console and blockchain components, visit [Administering your console on {{site.data.keyword.cloud_notm}} Private](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-manage#console-icp-manage).
