---
title: Environments
---

![Environments Screen](/ui/environments.png)

Here you can manage the list of environments used by your organization. These represent your real-world deployment or release environments. Maintaining this list, along with the `record-deployment` and `record-release` commands, allows PactFlow to ensure you are safe to deploy using the `can-i-deploy` tool. You can read more [here.](https://docs.pact.io/pact_broker/recording_deployments_and_releases/)
![Environments Screen](/ui/environments-form.png)

&nbsp;

| Field | Description |
| ----- | ----------- |
| Name | A unique name, no spaces allowed. This name is used in the can-i-deploy and record-deployment CLI commands. For example, "payments-sit-1". This field cannot be edited |
| DisplayName | Verbose environment name. "Payments Team SIT 1". |
| Production | Whether or not this environment is a production environment. |
| Teams | Associating the environment with teams, used to determine which teams can view and edit the environment after it is created. See the permissions section below for details. |

&nbsp;

#### Recording deployments and releases

To successfully record a deployment or release to an environment, the user must be allowed to record a deployment/release for the application. Moreover, the user must be allowed to view the environment resource.

In terms of permissions and resource relationships, that means:

1. The user must have `deployment_and_release:record:team` and `environment:view:team` (or `environment:manage:team`) AND the environment, application and user must be assigned to the same team.

OR 
 
2. The user must have `deployment_and_release:record:*` and `environment:view:*` (or `environment:manage:*`).

#### Creating Production Environments

If all the Broker services are deployed to the same "public" internet, then there only needs to be one Production environment. If there are multiple segregated production environments (for example, when maintaining on-premises software for multiple customers) you should create a separate production Environment for each logical deployment environment.

#### Permissions

Environments can be associated with Teams via creating or editing an Environment, or in the Team settings when creating or editing a Team. The permissions that determine when a user can associate an environment with a team are as follows:

#### environment:manage:* 
Users with this permission can view, edit and delete all environments. When creating or editing an environment the user can add or remove any team, regardless of their 'teams' permissions. 

#### team:manage:*
When creating or editing any team the user can change the environments associated with the team.

#### team:manage:{uuid}
When creating or editing a team they have permission for, the user can change the environments associated with that team.

#### environment:read:*
The user can view a list of all environments but cannot edit or delete environments.

#### environment:read:team
The user can view a list of all the environments associated with their teams, but cannot edit or delete environments.

