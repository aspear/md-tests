# vke v0.9.1 (Build: 8cb3f2d)

Command line interface for VMware Kubernetes Engine
## List of supported commands

### 1 ```account```
Commands to log in and show account info


#### 1.1 ```account login```
Log in to your organization

##### Description

To see your organization ID, log in to VKE console, click on your name/org
	at the top of the page, and then click on the abbreviated organization ID to see
	the full organization ID. To get your refresh token, click on your name/org at the
 	top of the page, click on My Account, and then click on API Tokens.

##### Example

    vke account login -t fd2c1d78-9f00-4e30-8268-4ab8162080d \
                     -r 5fde5099-f329-4f1a-a580-fe359d919a7

##### Flags 
```
--organization value, -t value	VMware Cloud Services organization ID
--refresh-token value, -r value	OAuth refresh token obtained from VMware Cloud Services console
```
#### 1.2 ```account logout```
Logout from your organization


#### 1.3 ```account show```
Show the current user info

##### Description

Show information about the current logged in user.


### 2 ```cluster```
Commands to manage clusters


#### 2.1 ```cluster auth```
Sub-commands to manage kubectl authentication


#### 2.1.1 ```cluster auth delete```
Remove the configuration for a cluster from the default kubectl configuration file

##### Description

Remove the entries for a cluster from the default kubectl configuration file.

##### Example

    vke cluster auth delete TestCluster

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.1.2 ```cluster auth setup```
Configure kubectl to allow communication with a cluster

##### Description

Modify the default kubectl configuration file to allow access to a cluster.

##### Example

    vke cluster auth setup TestCluster

##### Flags 
```
--embed-ca, -e	Embed CA cert into kubectl config
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.2 ```cluster create```
Create a new cluster

##### Description

Create a new cluster in the current working project.

##### Example

    vke cluster create -n TestCluster -v 1.7.5 --region us-west-2

##### Flags 
```
--name value, -n value	Cluster name
--cluster-type value, --ct value	DEVELOPMENT: No HA for etcd or masters. PRODUCTION: etcd and masters 
		are deployed in HA fashion (multiple nodes & AZs)
--display-name value, -d value	Friendly name of the cluster, which is used for display
--cluster-network value	CIDR representation of the cluster network (optional). 
		If not specified, the default value is '10.1.0.0/16'
--pod-network value	CIDR representation of the pod network (optional). 
		If not specified, the default value is '10.2.0.0/16'
--service-network value	CIDR representation of the service network (optional). 
		If not specified, the default value is '10.0.0.0/24'
--region value, -r value	Region where the cluster should be created
--version value, -v value	Kubernetes version that should be created
--privilegedMode	Creates a cluster with privileged mode capabilities.
		What is privileged mode?
		A collection of root-equivalent functionalities available for all pods running 
		on the cluster. Privileged mode includes the following Kubernetes features: 
		hostPath, hostPID, hostNetwork, hostIPC, privileged pods and all linux capabilities.
		WARNING: By selecting "privileged mode", users enable a set of root-equivalent 
		functionalities for pods running on the cluster. Applications with privileged
		mode enabled have two risks. First, an application may modify the underlying OS 
		in ways that may result in an unstable node or cluster and ultimately result in 
		the inability for the VKE service to fully manage the smart cluster. Second, if  
		an application is compromised by a malicious attacker, it is easier to spread 
		the attack to other applications in the same cluster.
--force	This option will force creating a cluster with privileged mode capabilities.
--ip-whitelist value	Comma-delimited list of IPs that will be whitelisted for access to the UI and API. 
		If not specified, the default value is '0.0.0.0/0'		List is comma seperated IP ranges in CIDR format
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.3 ```cluster delete```
Delete a cluster

##### Description

Delete a cluster by name. WARNING: All running applications will be forcibly stopped.

##### Example

    vke cluster delete TestCluster

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.4 ```cluster get-kubectl-auth```
(Deprecated) Generate the kubectl command for authentication

##### Description

Generate the kubectl (Kubernetes command line client) configuration. There are two options. 
   1. Run vke cluster get-kubectl-auth TestCluster with --configfile option 
   to create a new Kubernetes config. You can move the created file to ~/.kube/config (Mac OS X and Linux) to replace 
   the default config. 
   2. Run vke cluster get-kubectl-auth TestCluster without the --configfile 
   option. This will print commands that can be copied and pasted to modify the Kubernetes configuration.

##### Example

    vke cluster get-kubectl-auth TestCluster -u user1@org-id -p password -cf config-file.txt

##### Flags 
```
--configfile value, --cf value	Filename to generate the kubectl configuration
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.5 ```cluster iam```
Sub-commands to manage access policies on a cluster


#### 2.5.1 ```cluster iam add```
Bind an identity to a role to grant permissions

##### Description

Bind an identity to a role on a cluster to grant permissions. Role can be 'smartcluster.admin', 
   'smartcluster.edit' or 'smartcluster.view'.

##### Example

    vke cluster iam add TestCluster -s user1@account.local \
             -r smartcluster.edit
      vke cluster iam add TestCluster -s group1 -r smartcluster.view

##### Flags 
```
--subject value, -s value	Name of the identity to bind (user or group)
--role value, -r value	Name of the role to bind
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.5.2 ```cluster iam export```
Export the cluster access policy to a file

##### Description

Export the cluster access policy to a file.

##### Example

    vke cluster iam export TestCluster -o file.txt

##### Flags 
```
--output value, -o value	Output file path
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.5.3 ```cluster iam import```
Import the cluster access policy from a file

##### Description

Import the cluster access policy from a file.

##### Example

    vke cluster iam import TestCluster -i file.txt

##### Flags 
```
--input value, -i value	Input file path
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.5.4 ```cluster iam remove```
Remove an identity from a role binding

##### Description

Remove an identity from a role binding on a cluster. Role can be 'smartcluster.admin',
   'smartcluster.edit', 'smartcluster.view' or use '*' to remove all existing roles.

##### Example

    vke cluster iam remove TestCluster -s user1@account.local \
              -r smartcluster.edit 
      vke cluster iam remove TestCluster -s group1 -r smartcluster.view

##### Flags 
```
--subject value, -s value	Name of the identity to unbind (user or group)
--role value, -r value	Name of the role to unbind
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.5.5 ```cluster iam show```
Show the direct and inherited access policies for the cluster

##### Description

Show the direct and inherited access policies for the cluster.

##### Example

    vke cluster iam show TestCluster

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.6 ```cluster ip-whitelist```
Commands to manage whitelisted IPs


#### 2.6.1 ```cluster ip-whitelist add```
Adds whitelisted IP ranges to the existing whitelisted IPs

##### Description

Add  whitelisted IP ranges to the existing whitelisted IPs 
          list-of-ip-ranges is a comma-separated list of IP ranges in CIDR format

##### Example

    vke cluster ip-whitelist add TestCluster "192.168.1.0/24,192.168.2.0/24"

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.6.2 ```cluster ip-whitelist create```
Create new whitelisted IP list

##### Description

Create new whitelisted IP list. 
              list-of-ip-ranges is a comma-separated list of IP ranges in CIDR format

##### Example

    vke cluster ip-whitelist create TestCluster "192.168.1.0/24,192.168.2.0/24"

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.6.3 ```cluster ip-whitelist delete```
Deletes whitelisted IP list. Access list will be restored to default, which is 0.0.0.0/0

##### Description

Delete whitelisted IP list.

##### Example

    vke cluster ip-whitelist delete TestCluster

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.6.4 ```cluster ip-whitelist remove```
Removes whitelisted IP ranges from the existing whitelisted IPs

##### Description

Removes  whitelisted IP ranges from the existing whitelisted IPs
          list-of-ip-ranges is a comma-separated list of IP ranges in CIDR format

##### Example

    vke cluster ip-whitelist remove TestCluster "192.168.1.0/24,192.168.2.0/24"

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.6.5 ```cluster ip-whitelist show```
Shows whitelisted IPs in CIDR format

##### Description

Shows whitelisted IPs in CIDR format.

##### Example

    vke cluster ip-whitelist show TestCluster

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.6.6 ```cluster ip-whitelist update```
Updates the whitelisted IP list. This overrides the existing whitelisted IP ranges

##### Description

Updates the whitelisted IP list. This overrides the existing whitelisted IP ranges
          list-of-ip-ranges is a comma-separated list of IP ranges in CIDR format

##### Example

    vke cluster ip-whitelist update TestCluster "192.168.1.0/24,192.168.2.0/24"

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.7 ```cluster list```
List clusters

##### Description

List the clusters in the current organization. The list can be filtered by folder and project.

##### Example

    vke cluster list

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.8 ```cluster maintain```
Start cluster maintenance

##### Description

Performs maintenance on the cluster, including re-creation of failed master nodes. 
   Please note that this should normally not be needed, except in unusual situations, 
   because maintenance is automatically performed.

##### Example

    vke cluster maintain TestCluster

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.9 ```cluster merge-kubectl-auth```
(Deprecated) Configure kubectl for authentication

##### Description

Modify the default kubectl configuration file to allow access to a cluster.

##### Example

    vke cluster merge-kubectl-auth TestCluster

##### Flags 
```
--embed-ca, -e	Embed CA cert into kubectl config
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10 ```cluster namespace```
Sub-commands to manage cluster namespaces


#### 2.10.1 ```cluster namespace create```
Create a new namespace

##### Description

Create a new namespace in a cluster.

##### Example

    vke namespace create cluster1 namespace1

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.2 ```cluster namespace delete```
Delete a namespace

##### Description

Delete a namespace by name.

##### Example

    vke namespace delete cluster1 namespace1

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.3 ```cluster namespace iam```
Sub-commands to manage access policies on a namespace


#### 2.10.3.1 ```cluster namespace iam add```
Bind an identity to a role to grant permissions

##### Description

Bind an identity to a role on a namespace to grant permissions. Role can be 'namespace.admin', 
   'namespace.edit' or 'namespace.view'.

##### Example

    vke namespace iam add <cluster-name> <namespace> \
          -s user1@account.local -r namespace.edit
      vke namespace iam add <cluster-name> <namespace> \
          -s group1 -r namespace.view

##### Flags 
```
--subject value, -s value	Name of the identity to bind (user or group)
--role value, -r value	Name of the role to bind
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.3.2 ```cluster namespace iam export```
Export the namespace access policy to a file

##### Description

Export the namespace access policy to a file.

##### Example

    vke namespace iam export cluster1 namespace1 -o file.txt

##### Flags 
```
--output value, -o value	Output file path
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.3.3 ```cluster namespace iam import```
Import the namespace access policy from a file

##### Description

Import the namespace access policy from a file.

##### Example

    vke namespace iam import cluster1 namespace1 -i file.txt

##### Flags 
```
--input value, -i value	Input file path
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.3.4 ```cluster namespace iam remove```
Remove an identity from a role binding

##### Description

Remove an identity from a role binding on a namespace. Role can be 'namespace.admin', 
   'namespace.edit', 'namespace.view' or use '*' to remove all existing roles.

##### Example

    vke namespace iam remove <cluster-name> <namespace> \
          -s user1@account.local -r namespace.edit 
      vke namespace iam remove <cluster-name> <namespace> \
          -s group1 -r namespace.view

##### Flags 
```
--subject value, -s value	Name of the identity to unbind (user or group)
--role value, -r value	Name of the role to unbind
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.3.5 ```cluster namespace iam show```
Show the access policies for the namespace

##### Description

Show the direct and inherited access policies for the namespace.

##### Example

    vke namespace iam show cluster1 namespace1

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.4 ```cluster namespace list```
List namespaces

##### Description

List the namespaces in a cluster.

##### Example

    vke namespace list cluster1

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.10.5 ```cluster namespace show```
Show information about a namespace

##### Description

Show details of a particular namespace.

##### Example

    vke namespace show cluster1 namespace1

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.11 ```cluster peering```
Sub-commands to manage VPC peerings


#### 2.11.1 ```cluster peering create```
Create a new VPC peering

##### Description

Create a new VPC peering request in a cluster.

##### Example

    vke cluster peering create -c <cluster-name> -n <peering-name> \
           --customer-account-id <customer-account-id> --customer-network-id <customer-network-id>  \
           --customer-network-cidr <customer-network-cidr> --customer-network-region <network-region>

##### Flags 
```
--cluster-name value, -c value	Cluster name
--name value, -n value	Peering name
--customer-account-id value	Customer account id
--customer-network-id value	Customer network id
--customer-network-cidr value	Customer network cidr
--customer-network-region value	Customer network region
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.11.2 ```cluster peering delete```
Delete a VPC peering

##### Description

Delete a VPC peering from a cluster.

##### Example

    vke cluster peering delete <cluster-name> <peering-id>

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.11.3 ```cluster peering list```
List VPC peerings

##### Description

List the VPC peerings in a cluster.

##### Example

    vke cluster peering list <cluster-name>

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.11.4 ```cluster peering rename```
Rename a VPC peering

##### Description

Update the name of a VPC peering.

##### Example

    vke cluster peering rename <cluster-name> <peering-id> <new-peering-name>

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.11.5 ```cluster peering show```
Show information about a VPC peering

##### Description

Show information of a VPC peering.

##### Example

    vke cluster peering show <cluster-name> <peering-id>

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.12 ```cluster rename```
Change the display name of a cluster

##### Description

Change the display name of a cluster.

##### Example

    vke cluster rename TestCluster 'New Display Name'

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.13 ```cluster show```
Show information about a cluster 

##### Description

Display the cluster's name, state, type, workerCount and the extended properties.

##### Example

    vke cluster show TestCluster

##### Flags 
```
--perf, -p	Cluster Creation performance report
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.14 ```cluster show-health```
Show health information about a cluster 

##### Description

Display the cluster's overall health, message and health details of individual components.

##### Example

    vke cluster show-health TestCluster

##### Flags 
```
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.15 ```cluster upgrade```
Upgrade the Kubernetes version a cluster is running

##### Description

Upgrade the Kubernetes version a cluster is running.

##### Example

    vke cluster upgrade TestCluster -v 1.7.5

##### Flags 
```
--version value, -v value	version
--folder value, -f value	Current working folder
--project value, --pr value	Current working project
```
#### 2.16 ```cluster versions```
Sub-commands to get information about Kubernetes versions


#### 2.16.1 ```cluster versions list```
List Kubernetes versions

##### Description

List the Kubernetes versions that are currently supported for a specific region.

##### Example

    vke cluster versions list --region us-west-2

##### Flags 
```
--region value, -r value	Region name (required)
```
### 3 ```folder```
Commands to manage folders


#### 3.1 ```folder create```
Create a new folder

##### Description

Create a new folder in the current organization.

##### Example

    vke folder create folder1 --display-name 'New Folder'

##### Flags 
```
--display-name value, -d value	Friendly name of the folder, which is used for display
```
#### 3.2 ```folder delete```
Delete a folder

##### Description

Delete a folder by name.

##### Example

    vke folder delete folder1

#### 3.3 ```folder get```
Show the current working folder

##### Description

Show the current working folder, used by vke commands that require a folder context.

##### Example

    vke folder get

#### 3.4 ```folder iam```
Sub-commands to manage access policies on a folder


#### 3.4.1 ```folder iam add```
Bind an identity to a role to grant permissions

##### Description

Bind an identity to a role on a folder to grant permissions. Role can be 
   'folder.edit' or 'folder.view'.

##### Example

    vke folder iam add folder1 -s user1@account.local \
              -r folder.edit
      vke folder iam add folder1 -s group1 -r folder.view

##### Flags 
```
--subject value, -s value	Name of the identity to bind (user or group)
--role value, -r value	Name of the role to bind
```
#### 3.4.2 ```folder iam export```
Export the folder access policy to a file

##### Description

Export the folder access policy to a file.

##### Example

    vke folder iam export folder1 -o output.txt

##### Flags 
```
--output value, -o value	Output file path
```
#### 3.4.3 ```folder iam import```
Import the folder access policy from a file

##### Description

Import the folder access policy from a file.

##### Example

    vke folder iam import folder1 -i input.txt

##### Flags 
```
--input value, -i value	Input file path
```
#### 3.4.4 ```folder iam remove```
Remove an identity from a role binding

##### Description

Remove an identity from a role binding on a folder. Role can be 
   'folder.edit', 'folder.view' or use '*' to remove all existing roles.

##### Example

    vke folder iam remove folder1 -s user1@account.local \
              -r folder.edit 
      vke folder iam remove folder1 -s group1 -r folder.view

##### Flags 
```
--subject value, -s value	Name of the identity to unbind (user or group)
--role value, -r value	Name of the role to unbind
```
#### 3.4.5 ```folder iam show```
Show the access policies for the folder

##### Description

Show the direct and inherited access policies for the folder.

##### Example

    vke folder iam show folder1

#### 3.5 ```folder list```
List folders

##### Description

List the folders in the current organization.

##### Example

    vke folder list

#### 3.6 ```folder set```
Set the current working folder

##### Description

Set the current working folder, used by vke commands that require a folder context.

##### Example

    vke folder set folder1

#### 3.7 ```folder show```
Show information about a folder

##### Description

Show details of a particular folder.

##### Example

    vke folder show folder1

#### 3.8 ```folder unset```
Unset the current working folder

##### Description

Remove the current working folder, set in the vke configuration file.

##### Example

    vke folder unset

### 4 ```help```
Shows a list of commands or help for one command


### 5 ```iam```
Commands to manage Identity and Access Management (IAM)


#### 5.1 ```iam group```
Sub-commands to manage groups


#### 5.1.1 ```iam group create```
Create a new group

##### Description

Create a new group. Only organization administrators can create a new group.

##### Example

    vke group create group1 --description 'Purpose of this group'

##### Flags 
```
--description value, -d value	Description for group
```
#### 5.1.2 ```iam group delete```
Delete a group

##### Description

Delete a group. Only organization administrators can delete a group.

##### Example

    vke group delete group1

#### 5.1.3 ```iam group list```
List groups

##### Description

List the groups in the current organization.


#### 5.1.4 ```iam group member```
Sub-commands to manage group membership


#### 5.1.4.1 ```iam group member add```
Add a member to a group

##### Description

Add a member to a group. Only organization administrators can add members to a group.

##### Example

    vke group member add group1 --member-name Bob

##### Flags 
```
--member-name value, -m value	Member name (user or group)
```
#### 5.1.4.2 ```iam group member list```
List members

##### Description

List the members in a group.

##### Example

    vke group member list group1

#### 5.1.4.3 ```iam group member remove```
Remove a member from a group

##### Description

Remove a member from a group. Only organization administrators can remove members from a group.

##### Example

    vke group member remove group1 --member-name Bob

##### Flags 
```
--member-name value, -m value	Member name (user or group)
```
#### 5.1.5 ```iam group show```
Show group information

##### Description

Show details about a specific group.

##### Example

    vke group show group1

#### 5.2 ```iam role```
Sub-commands to manage roles


#### 5.2.1 ```iam role list```
List role definitions

##### Description

List the available role definitions.


#### 5.3 ```iam user```
Sub-commands to manage users


#### 5.3.1 ```iam user list```
List users

##### Description

List the users in the current organization.


#### 5.3.2 ```iam user show```
Show detailed user info

##### Description

Show information about a specific user.

##### Example

    vke user show user-name@organization

### 6 ```info```
Commands to get general VKE information


#### 6.1 ```info region```
Sub-commands to get information about the operational regions


#### 6.1.1 ```info region list```
List enabled regions

##### Description

List the currently enabled regions for the current organization.


### 7 ```organization```
Commands to manage the current organization


#### 7.1 ```organization iam```
Sub-commands to manage access policies on the organization


#### 7.1.1 ```organization iam add```
Bind an identity to a role to grant permissions

##### Description

Bind an identity to a role on a organization to grant permissions. Role can be 
   'organization.edit' or 'organization.view'.

##### Example

    vke organization iam add fd2c1d78-9f00-4e30-8268-4ab8162080d \
          -s user1@account.local -r organization.edit
      vke organization iam add fd2c1d78-9f00-4e30-8268-4ab8162080d \
          -s group1 -r organization.view

##### Flags 
```
--subject value, -s value	Name of the identity to bind (user or group)
--role value, -r value	Name of the role to bind
```
#### 7.1.2 ```organization iam export```
Export the organization access policy to a file

##### Description

Export the organization access policy to a file.

##### Example

    vke org iam export fd2c1d78-9f00-4e30-8268-4ab8162080d -o output.txt

##### Flags 
```
--output value, -o value	Output file path
```
#### 7.1.3 ```organization iam import```
Import the organization access policy from a file

##### Description

Import the organization access policy from a file.

##### Example

    vke org iam import fd2c1d78-9f00-4e30-8268-4ab8162080d -i input.txt

##### Flags 
```
--input value, -i value	Input file path
```
#### 7.1.4 ```organization iam remove```
Remove an identity from a role binding

##### Description

Remove an identity from a role binding on a organization. Role can be 
   'organization.edit' or 'organization.view'.

##### Example

    vke organization iam remove fd2c1d78-9f00-4e30-8268-4ab8162080d \
          -s user1@account.local -r organization.edit 
      vke organization iam remove fd2c1d78-9f00-4e30-8268-4ab8162080d \
          -s group1 -r organization.view

##### Flags 
```
--subject value, -s value	Name of the identity to unbind (user or group)
--role value, -r value	Name of the role to unbind
```
#### 7.1.5 ```organization iam show```
Show the access policy for the organization

##### Description

Show the access policy for the organization.

##### Example

    vke org iam show fd2c1d78-9f00-4e30-8268-4ab8162080d

#### 7.2 ```organization show```
Show detailed organization information

##### Description

Show information related to the current organization.

##### Example

    vke org show fd2c1d78-9f00-4e30-8268-4ab8162080d

### 8 ```project```
Commands to manage projects


#### 8.1 ```project create```
Create a new project

##### Description

Create a new project in the current working folder.

##### Example

    vke project create project1

##### Flags 
```
--display-name value, -d value	Friendly name of the project, which is for display
--folder value, -f value	Current working folder
```
#### 8.2 ```project delete```
Delete a project

##### Description

Delete a project by name.

##### Example

    vke project delete project1

##### Flags 
```
--folder value, -f value	Current working folder
```
#### 8.3 ```project get```
Show the current working project

##### Description

Show the current working project, used by vke commands that require a project context.

##### Example

    vke project get

#### 8.4 ```project iam```
Sub-commands to manage access policies on a project


#### 8.4.1 ```project iam add```
Bind an identity to a role to grant permissions

##### Description

Bind an identity to a role on a project to grant permissions. Role can be 
   'project.edit' or 'project.view'.

##### Example

    vke project iam add project1 -s user1@account.local -r project.edit
      vke project iam add project1 -s group1 -r project.view

##### Flags 
```
--subject value, -s value	Name of the identity to bind (user or group)
--role value, -r value	Name of the role to bind
--folder value, -f value	Current working folder
```
#### 8.4.2 ```project iam export```
Export the project access policy to a file

##### Description

Export the project access policy to a file.

##### Example

    vke project iam export project1 -o file.txt

##### Flags 
```
--output value, -o value	Output file path
--folder value, -f value	Current working folder
```
#### 8.4.3 ```project iam import```
Import the project access policy from a file

##### Description

Import the project access policy from a file.

##### Example

    vke project iam import project1 -i file.txt

##### Flags 
```
--input value, -i value	Input file path
--folder value, -f value	Current working folder
```
#### 8.4.4 ```project iam remove```
Remove an identity from a role binding

##### Description

Remove an identity from a role binding on a project. Role can be 
   'project.edit', 'project.view' or use '*' to remove all existing roles.

##### Example

    vke project iam remove project1 -s user1@account.local -r project.edit 
      vke project iam remove project1 -s group1 -r project.view

##### Flags 
```
--subject value, -s value	Name of the identity to unbind (user or group)
--role value, -r value	Name of the role to unbind
--folder value, -f value	Current working folder
```
#### 8.4.5 ```project iam show```
Show the access policies for the project

##### Description

Show the direct and inherited access policies for the project.

##### Example

    vke project iam show project1

##### Flags 
```
--folder value, -f value	Current working folder
```
#### 8.5 ```project list```
List projects

##### Description

List the projects in the current working folder.

##### Example

    vke project list

##### Flags 
```
--folder value, -f value	Current working folder
```
#### 8.6 ```project set```
Set the current working project

##### Description

Set the current working project, used by vke commands that require a project context.

##### Example

    vke project set project1

##### Flags 
```
--folder value, -f value	Current working folder
```
#### 8.7 ```project show```
Show information about a project

##### Description

Show details of a particular project.

##### Example

    vke project show project1

##### Flags 
```
--folder value, -f value	Current working folder
```
#### 8.8 ```project unset```
Unset the current working project

##### Description

Remove the current working project, set in the vke configuration file.

##### Example

    vke project unset

Global Flags:
```
--log-file value, -l value	write logging information into a logfile at the specified path
--output value, -o value	the output format can be any of the following values ["json"]
--detail, -d	print the current target, user, Organization and project
--help, -h	show help
--version, -v	print the version
```

