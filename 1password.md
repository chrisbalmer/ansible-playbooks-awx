# AWX 1Password Setup Playbook

This playbook downloads the 1Password CLI binary to AWX (localhost).

This only needs to be run once to setup 1Password for use in other playbooks.

You'll want to setup a credential type for 1Password and then add that credential to the templates using the 1Password lookup module.

## Credential Type Example

**Name:** `1Password Login`

**Input Configuration:**

```
fields:
  - id: op_email
    type: string
    label: 1Password Email
  - id: op_domain
    type: string
    label: 1Password Domain (i.e. my for non-business)
  - id: op_key
    type: string
    label: 1Password Key
    secret: true
  - id: op_password
    type: string
    label: 1Password Password
    secret: true
required:
  - op_email
  - op_domain
  - op_key
  - op_password
```

**Injector Configuration:**

```
extra_vars:
  op_domain: '{{ op_domain }}'
  op_email: '{{ op_email }}'
  op_key: '{{ op_key }}'
  op_password: '{{ op_password }}'
```

## Playbook Example Using This Lookup

Replace the `$values$` with information about the item you're looking up. *This example prints out the result so test with a non-password field from the item like `username`.*

```
---
- name: Test of 1Password Role
  hosts: localhost
  vars:
    test: "{{ lookup('onepassword', '$itemname$', section='$sectionname$', field='$fieldname$', vault='$vaultname$', domain=op_domain, secret_key=op_key, master_password=op_password) }}"

  tasks:
    - debug:
        var: test
```