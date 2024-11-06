
# OpenStack Project and User Management

## Projects

### List Projects
List all projects with their ID, name, and whether they are enabled or disabled:
```bash
$ openstack project list
```

### Create a Project
Create a project named `new-project`:
```bash
$ openstack project create --description 'my new project' new-project   --domain default
```

### Update a Project
Specify the project ID to update a project. You can update the name, description, and enabled status of a project.

- To temporarily disable a project:
```bash
$ openstack project set PROJECT_ID --disable
```

- To enable a disabled project:
```bash
$ openstack project set PROJECT_ID --enable
```

- To update the name of a project:
```bash
$ openstack project set PROJECT_ID --name project-new
```

- To verify your changes, show information for the updated project:
```bash
$ openstack project show PROJECT_ID
```

### Delete a Project
Specify the project ID to delete a project:
```bash
$ openstack project delete PROJECT_ID
```

## Users

### List Users
List all users:
```bash
$ openstack user list
```

### Create a User
Create the `new-user` user:
```bash
$ openstack user create --project new-project --password PASSWORD new-user
```

### Update a User
You can update the name, email address, and enabled status for a user.

- To temporarily disable a user account:
```bash
$ openstack user set USER_NAME --disable
```
If you disable a user account, the user cannot log in to the dashboard. However, data for the user account is maintained, so you can enable the user at any time.

- To enable a disabled user account:
```bash
$ openstack user set USER_NAME --enable
```

- To change the name and email address for a user account:
```bash
$ openstack user set USER_NAME --name user-new --email new-user@example.com
```
User has been updated.

### Delete a User
Delete a specified user account:
```bash
$ openstack user delete USER_NAME
```

## Roles and Role Assignments

### List Available Roles
List the available roles:
```bash
$ openstack role list
```

### Create a Role
Create the `new-role` role:
```bash
$ openstack role create new-role
```


# OpenStack Role Assignments

## Assign a Role
To assign a user to a project, you must assign the role to a user-project pair. To do this, you need the user, role, and project IDs.

1. List users and note the user ID you want to assign to the role:
```bash
$ openstack user list
```

2. List role IDs and note the role ID you want to assign:
```bash
$ openstack role list
```

3. List projects and note the project ID you want to assign to the role:
```bash
$ openstack project list
```

4. Assign a role to a user-project pair:
```bash
$ openstack role add --user USER_NAME --project TENANT_ID ROLE_NAME
```

For example, assign the `new-role` role to the `demo` and `test-project` pair:
```bash
$ openstack role add --user demo --project test-project new-role
```

5. Verify the role assignment:
```bash
$ openstack role assignment list --user USER_NAME   --project PROJECT_ID --names
```

## View Role Details
View details for a specified role:
```bash
$ openstack role show ROLE_NAME
```

## Remove a Role
Remove a role from a user-project pair:

1. Run the `openstack role remove` command:
```bash
$ openstack role remove --user USER_NAME --project TENANT_ID ROLE_NAME
```

2. Verify the role removal:
```bash
$ openstack role list --user USER_NAME --project TENANT_ID
```
If the role was removed, the command output omits the removed role.
