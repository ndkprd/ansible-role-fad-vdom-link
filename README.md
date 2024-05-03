ndkprd.fad_vdom
===============

An Ansible role to manage Fortinet's FortiADC VDOM Link resources via HTTP REST API.

Requirements
------------

FortiADC:
- a REST API user with enough permission.

Role Variables
--------------

| Variable Name     | Type          | Default Value | Description                                    |
|-------------------|---------------|---------------|------------------------------------------------|
| fad_vdom_links    | list of dicts | []            | List of VDOM links.                            |
| fad_vdoms[x].name | str           | -             | VDOM link name/mkey.                           |
| fad_vdoms[x].type | str           | ethernet      | VDOM link type, other possible value is 'ppp'. |

Dependencies
------------

- [ndkprd.fad_vdom](https://github.com/ndkprd/ansible-role-fad-vdom)

Examples
----------------

Inventory:

```ini
[fortiadc]
fad-01 ansible_host=fad-01.infra.ndkprd.com ansible_connection=local fad_apitoken=mysupersecrettoken fad_vdom=root

[fortiadc:vars]
http_port=80
https_port=443
```

Playbook:

```yaml
---

- name: Update/create FortiADC resources.
  hosts: all
  become: false
  gather_facts: false
  connection: local
  vars:
    fad_vdoms:
    - name: development
      status: enable
    - name: staging
      status: enable
    - name: production
      status: enable
  fad_vdom_links:
    - name: to-prd
      type: ethernet
    - name: to-stg
      type: ethernet
    - name: to-dev
      type: ethernet

  roles:
    - ndkprd.fad_vdom_link
```

Limitation
----------

For some reason I can't use the same IP Address twice for VDOM Link (e.g. you deleted the VDOM Link once and want to recreate the VDOM Link again). Currently I just keep chaning them for every iteration.

License
-------

MIT
