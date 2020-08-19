---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-23"

keywords: IBM Cloud Private, IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

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
{:pre: .pre}
{:curl: #curl .ph data-hd-programlang='curl'}

# Administering your console
{: #console-icp-manage}




After you deploy your console on {{site.data.keyword.cloud}} Private, you can use the console to add or remove console users and access APIs that allow you to operate your network and govern your console. You can also access and customize the logs of your console.
{:shortdesc}




## Managing users from the console
{: #console-icp-manage-users}

The user who provisions the {{site.data.keyword.blockchainfull_notm}} Platform console is considered the console administrator. The administrator can then add and grant other users access to the console from the **Users** tab. Every user that accesses the console must be assigned an access policy with a user role defined. The policy determines what actions the user can perform within the console. By default, the console administrator is given the **Manager** role for the console. Other users can be assigned with **Manager**, **Writer**, or **Reader** roles when a console manager adds them to the console. Note that the users can also be managed with [APIs](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-icp-manage#console-icp-manage-users-apis).

### Role to permission mapping
{: #console-icp-manage-role-mapping}

| Role | Permissions |
|--------|----------|
| Manager | As a Manager, you have permissions beyond the Writer role. You can do everything a Reader and Writer can do as well as: <ul><li>Provision new components such as CAs, peers, and ordering services, by using the console or APIs.</li><li>Delete provisioned components by using the console or APIs.</li><li>Add/remove users and change user access policies.</li><li>Change console logging levels by using the console or APIs.</li><li>Restart the console by using an API.</li></ul> |
| Writer | As a Writer, you have permissions beyond the Reader role, including: <ul><li>Import components by using the console or APIs.</li><li>Remove imported components by using the console or APIs.</li><li>Register users on a CA.</li><li> Add or remove notifications by using the console or APIs.</li></ul>  |
| Reader | As a reader, you can perform read-only actions including: <ul><li>View console UI.</li><li>View console log.</li><li>Export components.</li><li>Issue any GET API.</li></ul> | |
{: caption="Table 1. Role to permissions mapping table" caption-side="bottom"}

Permissions are cumulative. If you select a **Manager** role, the user will also be able to perform all **Writer** and **Reader** actions, and you are not required to additionally check those roles. Likewise, a user with the **Writer** role will be able to perform all of the actions in the **Reader** role.

### Managing a user's password
{: #console-icp-manage-user-pw}


The password that was stored inside the [password secret](/docs/blockchain-multicloud?topic=blockchain-multicloud-console-deploy-icp#console-deploy-icp-password-secret) and then passed to the console during configuration becomes the default password for the console. All users need to use this password when they log in to the console for the first time. The console manager can also specify a new default password. In the **Users** tab of the console, click the **Update configuration** link under your authentication service tile name. In the right panel, you can view or update the default password for new users of the console.




You need to share the default password, or the default password that you reset, with the users so that they can log in to the console. They will be required to change the password upon their first login.
{:note}

Users with the manager role can reset passwords for other users. In the **Users** tab of the console, click the three vertical dots at the end of the row for a specific user, and then click **Reset password**. The console resets that user's password to the default password, which you can check and confirm in the right panel. Users with any role can change their own password at any time. Click the avatar icon in the upper right corner, and then click **Change password**.

After you add new users to the console, the users might not be able to view all the nodes, channels, or chaincode, which other users deploy. To work with these components, each user needs to import the associated identities into their own console wallet. For more information, see [Storing identities in your console wallet](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Modifying a user's role
{: #console-icp-manage-reset-user-pw}

A user with a manager role can update the roles of other console users and provide them access to more or fewer console permissions. In the **Users** tab of the console, click the three vertical dots at the end of the specific user row, and then click **Update authenticated user**. Select the new role for the user in the right panel. The user's role will be updated when they log in to the console the next time.

### Deleting a user from the console
{: #console-icp-manage-icp-remove-user}

If you are a user with a manager role, you can remove a user's access to the console. In the **Users** tab of the console, click the checkbox at the beginning of the specific user row. Then, click the **Trash** can icon at the top of the table, next to the **Add new users** button. After you remove the user from the table, the user cannot log in to the console again.


## Using the {{site.data.keyword.blockchainfull_notm}} APIs
{: #console-icp-manage-apis}

The {{site.data.keyword.blockchainfull_notm}} Platform exposes RESTful APIs that allow you to import, edit, and remove blockchain nodes from your console. You can also use APIs to manage the settings, logging, and notifications of your console. Use the instructions below to use the APIs provided by a console deployed on {{site.data.keyword.cloud_notm}} Private.

### Prerequisites
{: #console-icp-manage-prereqs}

To use the APIs, you need to gather the following information:

- The console **API Endpoint**. This is the URL that you use to access the console by using your browser. You can find this URL in the **Notes:** section of the Helm release overview screen.
- A username and password that you can use to access the console. To create an API key, you must have an account with a [manager role](#console-icp-manage-users).

### Using an API key
{: #console-icp-manage-api-key}

Each console provides its own identity and access management. You can use your console username and password to generate an API key and secret that can authorize your API calls.

Each API key is associated with a role that governs the user's permissions. The API keys use the same access policy roles as exist for users who log in to the console by using a username and password. Refer to the table in the [Managing users](#console-icp-manage-users) section for the list of actions each role is allowed to perform. Because only manager roles can create API keys, a console administrator needs to create API keys for reader and writer users. The API keys never expire. As a result, the console administrator should delete API keys that are not being used.

There are three APIs available to manage your API keys:
- [Create an API key](#console-icp-manage-create-api-key)
- [View API keys](#console-icp-manage-view-api-keys)
- [Delete API keys](#console-icp-manage-delete-api-keys)

Use the request below to create an API key and secret. You can then use this key and secret to use the other APIs. The API secret is not stored by the console and needs to be saved by the user.

For all of APIs provided by the console, you need to add a `-k` or ``--insecure`` flag to bypass the requirement for a TLS certificate. You can also download the TLS certificate from your console by using your browser.

#### Create an API key
{: #console-icp-manage-create-api-key}

| **Request** |  |
|-------------|-----------|
| PATH | POST `<API_endpoint>`/ak/api/v2/permissions/keys |
| **Request body fields** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` At least one value is required </li><li>`string` optional</li></ul>|
| **Response body fields** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` Save: the key is not stored</li><li>`["<role>"]`</li></ul>|
| Authorization required | manager |

#### Example curl request: Create API key
{: #console-icp-manage-create-api-key-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v2/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -k \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### Manage your console API keys
{: #console-icp-manage-api-keys}

Once you have created an API key and secret, you can use the APIs to view or remove the keys that you have created. The keys and secrets do not expire until they are deleted.

#### View API keys
{: #console-icp-manage-view-api-keys}

| **Request** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v2/permissions/keys |
| **Response body fields** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` UNIX time stamp in milliseconds</li><li>`string`</li></ul>|
| Authorization required | reader |

#### Example curl request: view API keys
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v2/permissions/keys \
  -u <api_key>:<api_secret> \
  -k
```

#### Delete API keys
{: #console-icp-manage-delete-api-keys}

| **Request** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v2/permissions/keys/`<api_key>` |
| Authorization required| manager |

#### Example curl request: delete API key
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v2/permissions/keys/<api_key> \
  -u <api_key>:<api_secret> \
  -k
```

### Managing users using the APIs
{: #console-icp-manage-users-apis}


You can also use the APIs to list, add, or remove users who can log in to the console or access the console APIs. You can also edit the users roles to upgrade a users level of access. The following APIs are available:

- [List users](#console-icp-manage-list-users-api)
- [Edit users](#console-icp-manage-edit-users-api)
- [Add users](#console-icp-manage-add-users-api)
- [Remove users](#console-icp-manage-remove-users-api)

#### List users
{: #console-icp-manage-list-users-api}


| **Request** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v2/permissions/users |
| **Response body fields** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` user ID</li><li>`string` email address</li><li>`["<role>"]`</li><li>`number` UNIX time stamp in milliseconds</li></ul>|
| Authorization required | reader |

#### Example curl request: list users
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
  -u <api_key>:<api_secret> \
  -k
```

#### Edit users
{: #console-icp-manage-edit-users-api}

| **Request** |  |
|-------------|-----------|
| Path | PUT `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li>`uuids`</li><li>`roles`</li></ul> | <ul><li>`array of strings` user IDs </li><li>`["reader", "writer", "manager"]` At least one value is required</li></ul> |
| **Response body fields** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`array of strings` user IDs </li></ul>|
| Authorization| manager |

#### Example curl request: edit a user
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
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

#### Add users
{: #console-icp-manage-add-users-api}

| **Request** |  |
|-------------|-----------|
| Path | POST `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` At least one value is required </li><li>`string` optional</li></ul>|
| Authorization | manager |

#### Example curl request: add a user
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
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

#### Remove users
{: #console-icp-manage-remove-users-api}

| **Request** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` user ID</li></ul>|
| **Response body fields** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` user ID</li></ul>|
| Authorization | manager |

#### Example curl request: remove a user
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
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

### Use the APIs to manage your components

You can view the complete set of APIs that are available in the [{{site.data.keyword.blockchainfull_notm}} Platform API reference](https://cloud.ibm.com/apidocs/blockchain).

Because you are using the APIs to communicate with your console on {{site.data.keyword.cloud_notm}} Private, you need to use the authorization that is provided by {{site.data.keyword.cloud_notm}} with the authentication that is provided by your console. Replace the `Bearer Auth` in the API reference with  `-u <api_key>:<api_secret>`. You also need to add a `-k` or ``--insecure`` flag to the command, or download the console TLS certificate by using your browser. You cannot use the **Try it out** tab to test the APIs for networks on {{site.data.keyword.cloud_notm}} Private.

As an example, the API call below returns information about all of your components running on a service instance of the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.
```
curl -X GET \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v2/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
The same API call would resemble the request below for a console deployed on {{site.data.keyword.cloud_notm}} Private.
```
curl -X GET \
https://9.30.252.107:31212/ak/api/v2/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -k
```

You can use the APIs to create nodes on the cluster where your console is deployed, and to import nodes from other clusters or {{site.data.keyword.cloud_notm}}. For more information, see [Build a network by using APIs](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-v2-apis#ibp-v2-apis-build-with-apis) and [Import a network by using APIs](/docs/blockchain-multicloud?topic=blockchain-multicloud-ibp-v2-apis#ibp-v2-apis-import-with-apis).


## Viewing your logs
{: #icp-console-manage-logs}

When you use the {{site.data.keyword.blockchainfull_notm}} Platform console, you might need to view logs to debug an issue.

### Viewing your console logs
{: #console-icp-manage-console-logs}

You can easily access the console logs if you need to debug problems that you encounter when you use the console or operate your nodes. You can also set the logging level to increase or decrease the amount of logs that the console collects. The console logs are collected separately from the [node logs](#console-icp-manage-node-logs), which are collected by {{site.data.keyword.cloud_notm}} Private.  

Navigate to the **Settings** tab in the console browser to change the logging settings. The console logs are collected from two separate sources:

  * **Client logging:** These logs are collected when commands are sent from your browser to the console.
  * **Server logging:** These logs are collected when the console sends commands to your nodes and from the console deployment. These logs include the Hyperledger Fabric logging output.

Set the number of collected logs by using the drop-down list under each log type. For example, **Error** and **Warning** collect the least amount of logs, while **Debug** and **All** collect the most.

You can view only the console logs if you are logged in as a console administrator. To view the logs from the **Settings** tab, replace the word `settings` in the browser URL with `api/v2/logs`. For example, if your console url is `localhost:3001/settings`, you can view your logs by navigating to `localhost:3001/api/v2/logs`. Client and server logs are collected in separate files. The most recent logs are at the top of the page.

### Viewing your node logs
{: #console-icp-manage-node-logs}


Component logs can be viewed from the command line by using the [kubectl CLI commands](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} or through [Kibana](https://www.elastic.co/products/kibana){: external}, which is included in your {{site.data.keyword.cloud_notm}} Private cluster.




- Use the `kubectl logs` command to view the container logs inside the pod. Follow the instructions to [Install the kubectl cli](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} if you have not already done so. If you are unsure of your pod name, run the following command to view your list of pods.

  ```
  kubectl get pods
  ```
  {:codeblock}

  Then, run the following command to retrieve the logs for the node container that resides inside the pod:

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Replace `<pod_name>` with the name of your pod from the command output above.  
  Replace `<node>` with `ca`, `peer`, or `orderer` to view the logs for your node.  
  Replace `<node>` with `fluentd`  to view the logs for your smart contracts.



  For more information about the `kubectl logs` command, see [Kubernetes documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.


- Alternatively, you can [access the logs](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external} from your {{site.data.keyword.cloud_notm}} Private console, which opens the logs in Kibana.

  **Note:** When you view your logs in Kibana, you might receive the response `No results found`. This condition can occur if {{site.data.keyword.cloud_notm}} Private uses your worker node IP address as its hostname. To resolve this problem, remove the filter that begins with `node.hostname.keyword` at the top of the panel and the logs will become visible.




### Viewing your smart contract container logs
{: #console-icp-manage-container-logs}

If you encounter issues with your smart contract, you can view the smart contract, or chaincode, container logs to debug an issue. You can run the following command to view the smart contract container logs:


```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}




Replace `<peer_pod>` with the name of the peer pod where the chaincode is running. Use the command `kubectl get po` to get the list of running pods.




## Installing patches for your nodes
{: #console-icp-manage-patch}

The underlying {{site.data.keyword.IBM_notm}} Hyperledger Fabric docker images for the peer, CA, and orderer nodes might need to be updated over time, for example, with security updates or to a new Fabric point release. You can update your Fabric images when you upgrade the {{site.data.keyword.blockchainfull_notm}} Platform helm chart.

After you upgrade your Helm chart, you will see the **Patch available** text on a node tile if a component update is available. This patch can be installed on a node whenever you are ready. These patches are optional, but recommended.

Patches are applied to nodes one at a time. While the patch is being applied, the node is unavailable to process requests or transactions. Therefore, to avoid any disruption of service, whenever possible you should ensure that another node of the same type is available to process requests. Installing patches on a node takes about a minute to complete and when the update is complete, the node is ready to process requests.
{:note}

To apply a patch to a node, open the node tile and click the **Install patch** button.
You cannot patch nodes that you imported to the console.
{: important}
