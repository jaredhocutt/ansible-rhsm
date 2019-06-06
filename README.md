# RHSM

This role handles subscribing a Red Hat Enterprise Linux (RHEL) host to Red Hat
Subscription Manager (RHSM) and then enabling the desired repositories.

In the case that the RHEL host is running in AWS, the role will disable the AWS
plugins for RHSM and disable the RHUI repositories.

## Requirements

The hosts you are targeting should have the following packages:

- `subscription-manager`

## Role Variables

| Variable                  | Required | Default | Description                                                                     |
| ------------------------- | -------- | ------- | ------------------------------------------------------------------------------- |
| `rhsm_username`           | &#9989;  |         | Username for access.redhat.com                                                  |
| `rhsm_password`           | &#9989;  |         | Password for access.redhat.com                                                  |
| `rhsm_repositories`       | &#9989;  | `[]`    | List of RHSM repositories to enable                                             |
| `rhsm_purge_repositories` |          | `no`    | Disable all currently enabled repositories not specified in `rhsm_repositories` |
| `rhsm_consumer_name`      |          |         | Name to represent system in RHSM (defaults to the hostname)                     |
| `rhsm_consumer_type`      |          |         | Type of unit to register in RHSM (defaults to system)                           |

There are multiple options for specifying information required to subscribe a host to RHSM. You must specify the variables for **one** of the options below:

| Option | Variable(s)                            | Description                                                                                                                                           |
| ------ | -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | `rhsm_pool`                            | Name of the subscription pool to consume (regular expressions allowed)<br><br>If possible, use `rhsm_pool_ids` instead, as it is much faster.         |
| 2      | `rhsm_pool_ids`                        | The ID(s) of the subscription pool(s) to consume.<br><br>A pool ID may be specified as a string (i.e. just the pool ID) or as a list of pool IDs      |
| 3      | `rhsm_org_id`<br>`rhsm_activation_key` | You must specify both `rhsm_org_id` and `rhsm_activation_key` when using this option.<br><br>The organization ID and activation key for registration. |

## Dependencies

None

## Example Playbook

See the table above for details on the available options.

### Option 1

```yaml
- hosts: servers
  roles:
    - role: jaredhocutt.rhsm
      vars:
        rhsm_username: username
        rhsm_password: password
        rhsm_pool: "^Red Hat Enterprise Linux Server 7 (RPMs)$"
        rhsm_repositories:
          - rhel-7-server-rpms
          - rhel-7-server-extras-rpms
```

### Option 2

```yaml
- hosts: servers
  roles:
    - role: jaredhocutt.rhsm
      vars:
        rhsm_username: username
        rhsm_password: password
        rhsm_pool_ids: ba4e7732f8abcdad545c7f62df736d1f
        rhsm_repositories:
          - rhel-7-server-rpms
          - rhel-7-server-extras-rpms
```

### Option 3

```yaml
- hosts: servers
  roles:
    - role: jaredhocutt.rhsm
      vars:
        rhsm_username: username
        rhsm_password: password
        rhsm_org_id: 1234567
        rhsm_activation_key: 2f75cbefcc7b72229c7324501be5668b
        rhsm_repositories:
          - rhel-7-server-rpms
          - rhel-7-server-extras-rpms
```

## License

MIT

## Author Information

Jared Hocutt (@jaredhocutt)
