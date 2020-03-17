# ar_os_registry_secret
Ansible Role to create Openshift secrets and associate with specific 
Service Accounts.

## Requirements
- oc command line is available
- Execution is made by user able to add users to groups (oc create secret) 


## Role Variables
The following details:
- the parameters that should be passed to the role (aka vars)
- the defaults that are held
- the secrets that should generally be sourced from an ansible vault.

### Parameters:
| Variable                        | Description                                    | Default        |
| --------                        | -----------                                    | -------        |
| ar_os_registry_secret_list      | List of secrets to create                      | []             |
| ar_os_registry_secret_namespace | Openshift project/namespace to add the secrets | null (invalid) |

For ar_os_registry_secret_list the structure is:
```
[
  {
    secret_name: '<Secret Name>'
    server: '<Associated Server>'
    username: '<User associated with the secret>'
    password: '<Password for the user>'
    email: '<Email for the user (optional)>'
    token: '<Token created for the user>'
    operation: '<Type of operation ['pull' | 'push']>'
  }, 
  ... 
]
```

### Defaults
| Variable                         | Description                              | Default |
| --------                         | -----------                              | ------- |
| ar_os_registry_secret_assertions | List of assertions made before execution | ar_os_registry_secret_namespace is provides, 'server' and 'username' are in each ar_os_registry_secret_list item,  |

### Secrets
The following variables should be provided through an encrypted source:
- ar_os_registry_secret_list[*].password
- ar_os_registry_secret_list[*].token

## Dependencies

- ar_os_secret_link


## Example Playbook

```
- hosts: localhost
  tasks:
    - name: Include registry secret
      include_role:
        name: ar_os_registry_secret
        tasks_from: registry-secret
      vars:
        ar_os_registry_secret_namespace: "sunshine-desserts"
        ar_os_registry_secret_list: [
          {
            secret_name: "t2-registry-credential",
            server: "docker-registry-default.t2.training.local",
            username: "reggieperrin",
            token: 'eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdW...',
            email: "reggieperrin@sunshine-desserts.com"
          },
          {
            secret_name: "t2-registry-push-credential",
            server: "docker-registry-default.t2.training.local",
            username: "charlesjefferson",
            token: 'eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdW...',
            email: "charlesjefferson@sunshine-desserts.com",
            operation: 'push'
          }          
        ]
```


## License

MIT / BSD

## Author Information

This role was created in 2020 by David Stewart (dstewart@redhat.com)
