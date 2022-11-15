---
title: "User administration"
weight: 1
sidebarTitle: "Users"
description: |
  Learn how to add and delete users and assign user permissions per environment type.
---

Learn about user roles and environment types, how to add and delete users, and how to assign user permissions per environment type.

## User roles

Within a project, each user has a role that controls their access and permission levels.

* Project Admin: Users who can configure project settings, add and remove users, administer environment permissions, push code, and execute actions on all project environments.
* Project Viewer: Any user with access to [environment types](#environment-types) automatically gets this role.

These control who has access to projects.

To see all projects you have a role in, from the main Console page
click **All projects&nbsp;<span aria-label="and then">></span> All projects**.
See more on access control for [organizations](./organizations.md).

## Environment types

Each environment type groups one or more environments together so that you can manage access for all environments of a certain type.
This allows you to set permissions for multiple environments at once based on their purpose.

Platform.sh offers three environment types: Production, Staging, and Development.
You can assign user permissions for each environment type.
Any permissions you assign to an environment type apply to all environments of that type.

For example, if you assign User1 **Admin** permissions for Development environments,
User1 has **Admin** permissions for all environments of that type.

A few things to consider:

* Only one environment per project can be the Production type. It's set automatically as the default branch and can't be overridden separately.
* You can change an environment's type (if it's not Production).
* You can have multiple Staging and Development environments.

The following table shows the available roles for environment types.

| Role | View environment | Push code | Branch environment | SSH access | Change settings | Execute actions |
| ---- | ---------------- | --------- | ------------------ | ---------- | --------------- | --------------- |
| Viewer | Yes | No |  No |  No |  No |  No |
| Contributor | Yes | Yes | Yes | Yes | No | No |
| Admin| Yes | Yes | Yes | Yes | Yes | Yes |

To customize who can use SSH, [set the access key](../create-apps/app-reference.md#access) in your `platform.app.yaml` file.

## Manage users

To manage users, you need to be a [Project Admin](#user-roles), or an organization owner, or an organization user with the [Manage users](./organizations.md#manage-your-organization-users) permission.

### Audit users permissions on projects

{{< codetabs >}}

---
title=In the Console
file=none
highlight=false
---

- Navigate to you organization or the project in it.
- Open the user menu (your name or profile picture).
- Click **Users**.
- Click {{< icon more >}} **More** on the user you want to audit.
- Click **Edit user**.
- Audit the permissions across all projects, or manage the user’s permissions (see examples below).

<--->
---
title=Using the CLI
file=none
highlight=false
---

Say you want to audit `user1@example.com` permissions on projects **projectID1**, **projectID2**, **projectID3**:

```bash
platform multi --projects projectID1,projectID2,projectID3 'user:get user1@example.com'
```

{{< /codetabs >}}

### Add a user to a project

{{< codetabs >}}

---
title=In the Console
file=none
highlight=false
---

- Navigate to you organization or the project in it.
- Open the user menu (your name or profile picture).
- Click **Users**.
- Click {{< icon more >}} **More** on the user you want to add.
- Click **Edit user**.
- Click **+ add to project**.
- Select a project from the list and choose the user's permissions.
- Click **Save**.

<--->
---
title=Using the CLI
file=none
highlight=false
---

Say you want to add `user1@example.com` to the project with a Project Admin role:

```bash
platform user:add user1@example.com -r admin
```

{{< /codetabs >}}

The user has to create an account before they can contribute to the project.
Once you add a user to a project, they receive an email with instructions.
After you add a user to a project or an environment type, you don’t need to redeploy to propagate SSH access changes to each environment.

### Delete a user from a project

{{< codetabs >}}

---
title=In the Console
file=none
highlight=false
---

- Navigate to you organization or the project in it.
- Open the user menu (your name or profile picture).
- Click **Users**.
- Click {{< icon more >}} **More** on the user you want to delete.
- Click **Edit user**.
- Click {{< icon more >}} **More** on the project you want to delete the user from.
- Click **Remove**.
- Click **Yes** to confirm.

<--->

---
title=Using the CLI
file=none
highlight=false
---
To delete existing users:

```bash
platform user:delete user1@example.com
```
{{< /codetabs >}}

Once you delete a user, they can no longer access the project.
After clicking **Edit user**, you can also click **Remove user** to remove the user from all projects at once.
After you delete a user from a project or an environment type, you don’t need to redeploy to propagate SSH access changes to each environment.

### Change existing permissions for environment types

{{< codetabs >}}

---
title=In the Console
file=none
highlight=false
---

- Navigate to you organization or the project in it.
- Open the user menu (your name or profile picture).
- Click **Users**.
- Click {{< icon more >}} **More** on the user whose permissions you want to change.
- Click **Edit user**.
- Click {{< icon more >}} **More** on the project you want to change the user’s permissions from.
- Click **Edit**.
- Select the user’s permissions on environments.
- Click **Save**.

<--->

---
title=Using the CLI
file=none
highlight=false
---
Say you want `user1@example.com` to have the Viewer role for Production environments
and the Contributor role for Development environments:

```bash
platform user:update user1@example.com -r production:v,development:c
```

After you change a user's role for an environment type, you don’t need to redeploy to propagate SSH access changes to each environment.

{{< /codetabs >}}

## Troubleshooting

If you have setup an external integration to GitHub, GitLab, or Bitbucket and your users can't clone the project locally,
see how to [troubleshoot source integrations](../integrations/source/troubleshoot.md).
