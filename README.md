# Additional Information

See also https://github.com/scottd018/foreman_publish_content_view for the initial publish of a content view version.

# Variables

All default variables, which are not likely to change, are stored in **./defaults/main.yml** and should be reviewed before executing a playbook, with this role.

Required variables are as follows:

**foreman_organization:** the organization that the content view belongs to

**foreman_server:**       the IP Address or Hostname used to connect to the Foreman/Satellite API

**foreman_content_view:** the name of the content view

**foreman_user:**         the user which has appropriate access to publish the content view

**foreman_password:**     the password of the user which has appropriate access to publish the content view

**foreman_life_env_path** a list, in order, of lifecycle environment paths to promote to

# Inventory

**NOTE:** this role is largely dependent upon API calls and is not likely to depend on SSH connectivity, and therefore may likely depend on using `localhost` as the inventory.  One may delegate to another server if required, but `localhost` is sufficient assuming proper connectivity to the Foreman API.  Ansible maintains `localhost` in its inventory by default and thus, no further work to the inventory is needed if `localhost` is a desired execution target.

# Sample Playbook

```
- hosts:        localhost
  gather_facts: false
  connection:   local
  become:       false

  roles:

    - role: 'foreman_promote_content_view'
      vars:
        foreman_organization:  'Scott-Net'
        foreman_server:        '10.10.10.164'
        foreman_content_view:  'Test-View'
        foreman_user:          'admin'
        foreman_password:      'redactedpassword'
        foreman_life_env_path:
          - 'Dev'    # ORDERED: first lifecycle environment in path
          - 'Test'   # ORDERED: second lifecycle environment in path
          - 'Prod'   # ORDERED: third lifecycle environment in path
                     # etc, etc, etc

```

**NOTE:** vars can be passed in many different ways in Ansible.  This is just one example of how to accomplish this task.  Extra vars using the `-e "var1=value1 var1=value1"` is perfectly acceptable.

# Sample Usage

```
ansible-playbook -vv my_playbook.yml
```
