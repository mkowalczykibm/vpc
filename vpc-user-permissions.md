---

copyright:
  years: 2019

lastupdated: "2019-08-27"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Granting user permissions for VPC resources
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.vpc_full}} uses role-based access control that enables account administrators to control their users' access to VPC resources. Access can be assigned to individual users or to groups of users by using {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM).
{:shortdesc}

This document shows examples of how the VPC administrator can use the {{site.data.keyword.cloud_notm}} console to grant the correct permissions for managing VPC infrastructure resources. It covers the following scenarios:

* **Full-access scenario:** Assign an access policy so a new user can create and use all VPC infrastructure resources (including VPCs).
* **Limited access scenario:** Assign an access policy so an existing user can create and use only virtual server instances.
* **Team access scenario:** Set up resource groups and access groups to allow two separate teams to create and use the VPC resources that are assigned to their team.

You can also manage permissions through the CLI or API. For more information, see [How do I use IBM Cloud IAM](/docs/iam?topic=iam-iamoverview#howto).
{: tip}

## Full-access scenario
{: #inviting-new-user-to-create-or-manage-vpc-resources}

This scenario shows how to invite a new IBM Cloud user to your account and give them access to VPC infrastructure so they can view, create, and update all VPC resources in the Default resource group.

To give a new user access to all VPC infrastructure resources:

1. Go to the [IAM Users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam/users){: new_window} page in the IBM Cloud console and click **Invite users**.
3. In the **Users** section of the Invite users page, enter the email addresses of the users that you want to invite in the **Email address** field.
4. In the **Access** section, expand the **Services** area and complete the following tasks:
  * From the **Assign access to** list, select **Resource group**.
  * From the **Resource group** list, select the Default resource group.
  * Make sure the **Assign access to a resource group** option is set to **Viewer**. 
  * From the **Services** list, select **VPC Infrastructure**.
  * From the **Resource type** list, select **All resource types**.
  * In the **Assign platform access roles** area, select **Editor**.
  * Scroll to the end of the page and click **Invite users**.

## Limited access scenario
{: #giving-existing-user-permission-to-create-vsis}

This scenario shows how to give an existing user permission to create and manage only virtual server instances in the Default resource group. Before the user can create an instance and associate a floating IP, the user also needs access to related resources, such as the VPC and subnet in which the instance will be created.

1. Go to the [IAM Users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam/users){: new_window} page in the IBM Cloud console and select the user whose access you want to configure.
3. On the **Access policies** tab, click **Assign access**.
4. Click **Assign access within a resource group** and select the Default resource group.
6. Make sure the **Assign access to a resource group** option is set to **Viewer**.
7. From the **Services** list, select **VPC Infrastructure**.
8. From the **Resource type** list, select **All resource types**.
9. In the **Assign platform access roles** area, select **Operator**.
10. Click **Assign**.
1. Repeat the previous steps to assign access to the following related resources.

| Resource type| Platform access role |
|--------------------------|------------------------|
|Block Storage for VPC     | Editor            | 
|Virtual Server for VPC | Editor|
|Floating IP for VPC| Editor|




## Team access scenario
{: #team-access-scenario}

This scenario shows how an account administrator can assign authorization so that different teams have access to separate VPC resources. The example uses _resource groups_ to set up separate resource access for two teams. For the purposes of this example, resources are not shared across teams.

The example takes you through the process of creating resource groups, creating access groups, and assigning the appropriate policies to provide your teams with access to separate VPC resources.

In this scenario, you're setting up two different project teams to use separate VPCs. You'll assign access so that each team has access to their team's VPC resources only.

* Your first team is a test team. You've decided to assign them access to VPCs in a resource group named `test_vpcs`.
* The second team is your production team. They'll be assigned access to VPCs in a resource group named `production_vpcs`.

This strategy can be used to assign separate VPC resources to any number of teams. However, all resources share the same VPC quotas for the account. For more information about quotas and limits, see [VPC quotas](/docs/vpc?topic=vpc-quotas#quotas).
{: tip}

### Step 1: Create resource groups
{: #step-1-create-resource-groups}

Create resource groups that contain each of your teams' VPC resources.

1. Create a resource group called `test_team`.  
2. Create a resource group called `production_team`.  

For more information about how to create resource groups, see [Managing resource groups](/docs/resources?topic=resources-rgs).

By default, account administrators can create new resource groups. Other users must be assigned the **Editor** role for **All Account Management Services**, which allows them to create resource groups.
{: tip}

### Step 2: Create access groups
{: #step-2-create-access-groups}

Resource access can be assigned to groups of users. Groups of users with the same access permissions are called _access groups_. In this scenario, the account administrator creates an access group to represent each grouping of team members who require a specific type of VPC access, a total of four unique access groups.

Create four access groups with the following names, and assign the appropriate users to each access group:
* `test_team_manage_vpcs`
* `test_team_view_vpcs`
* `production_team_manage_vpcs`
* `production_team_view_vpcs`

For more information about how to create access groups and assign users to the access groups, see [Create access groups](/docs/iam?topic=iam-groups#create_ag).

### Step 3: Add IAM policies to the access groups
{: #step-3-add-iam-policies-to-the-access-groups}

Add the necessary VPC access policies for each access group. For example, add a policy so members of the `test_team_manage_vpcs` access group can create, update, and delete all VPC resources in the `test_team` resource group.  

1. Go to the [IAM Group UI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam/groups){: new_window} in the IBM Cloud console.
2. Select an access group. Let's start with the `test_team_manage_vpcs` access group.
3. On the **Access policies** tab, click **Assign access**.
4. Select **Assign access within a resource group**.
5. Select the corresponding resource group. In this case, select the `test_team` resource group.
6. Make sure the **Assign access to a resource group** option remains set to **Viewer**.
7. Select the service **VPC Infrastructure**.
8. Make sure **Resource type** remains set to **All resource types**.
9. In the **Assign platform access roles** area, select **Editor**.
10. Select **Assign**.

Because the boot volume that's automatically attached to an instance and floating IP resources are created in the Default resource group, you must also add access policies for the Default resource group. 

| Access group | Resource group |  Resource type | Platform access role|    
|--------------------------|------------------------|-----------------------------------------------------------------|--------------------------|------------------------|-----------------------------------------------------------------|
|test_team_manage_vpcs|Default|Block Storage for VPC| Editor|
|test_team_manage_vpcs|Default|Floating IP for VPC| Editor|


Repeat the previous steps to add access policies for the remaining three access groups. 

| Access group | Resource group |  Resource type | Platform access role|    
|--------------------------|------------------------|-----------------------------------------------------------------|--------------------------|------------------------|-----------------------------------------------------------------|
|test_team_view_vpcs|test_team| All resource types| Viewer|
|test_team_view_vpcs|Default| Block Storage for VPC| Viewer|
|test_team_view_vpcs|Default| Floating IP for VPC| Viewer|
|production_team_manage_vpcs|production_team| All resource types| Editor|
|production_team_manage_vpcs|Default| Block Storage for VPC| Editor|
|production_team_manage_vpcs|Default| Floating IP for VPC| Editor|
|production_team_view_vpcs|production_team| All resource types| Viewer|
|production_team_view_vpcs|Default| Block Storage for VPC| Viewer|
|production_team_view_vpcs|Default| Floating IP for VPC| Viewer|
 
The teams are now set up to use VPCs. Members of the `test_team_manage_vpcs` and `production_team_manage_vpcs` access groups can now create VPCs in their assigned resource groups (that is, in the `test_team_manage_vpcs` and `production_team_manage_vpcs` resource groups).

When you create a VPC or other resources, make sure you specify the resource group in which to create the resource. If you don't specify a resource group, the resource is created in the Default resource group.
{: tip}

## Viewing user's permissions
{: #viewing-user-permissions}

Policies can be viewed in the user's **Access policies** tab.

You can use the following CLI commands to validate the resource group permissions that are assigned to your user, by policy or by access group:

**By policy**
```
ibmcloud iam user-policies <username>
```
**By access group**
```
ibmcloud iam access-groups -u <username>
```

Changes to IAM access policies for VPC can take up to 10 minutes to take effect.
{: note}

## Related links
{: #permissions-related-links}

* [Managing identity and access](/docs/iam?topic=iam-getstarted)
* [Managing users and access](/docs/iam?topic=iam-iamuserinv)
* [What is IAM](/docs/iam?topic=iam-iamoverview)

