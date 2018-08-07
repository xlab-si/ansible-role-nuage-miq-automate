Ansible Role: nuage_miq_automate
=========

This role accesses ManageIQ Automation Workspace for you to provide you with:

- Nuage credentials
- EMS event data

Requirements
------------

You need to have following Automate Instance Attributes assigned to your instance:

- nuage_username
- nuage_password (encrypted)
- nuage_enterprise
- nuage_url
- nuage_api_version

If you don't specify those, the role will still work, but `nuage_auth` output will
contain empty strings.

Role Variables
--------------

| Variable            | Default | Description |
|---------------------|---------|-------------|
| manageiq            | /       | Set by Automate.
| manageiq_connection | /       | Set by Automate.


```yaml
manageiq:
  X_MIQ_Group: Tenant My Company access
  api_token: 2d5d9031361a6ac570d810406f267cea
  api_url: 'http://localhost:4000'
  automate_workspace: automate_workspaces/82632486-7529-45c6-8fa7-fc53ebbb157d
  group: groups/1
  user: users/1
manageiq_connection:
  X_MIQ_Group: Tenant My Company access
  token: 2d5d9031361a6ac570d810406f267cea
  url: 'http://localhost:4000'
```

You shouldn't need to worry about role inputs because ManageIQ Automate will provide
them for you.

Outputs
-------
This role sets following variables for you:

| Stats name       | Description |
|------------------|-------------|
| nuage_auth       | Nuage authentication hash. Can be passed to nuage_vspk module directly.
| event            | EMS event data, see below.
| object           | Current Automate Instance absolute name.

```yaml
event:
  type:        CREATE
  ems_id:      4
  entity_type: subnet
  full_data:   { ... } # full event data as emitted by Nuage provider
  entity:      { ... } # event['entities'].first
```

Dependencies
------------

This role requires following roles that are also available on Ansible Galaxy:

- [syncrou.manageiq-automate](https://galaxy.ansible.com/syncrou/manageiq-automate)
- [syncrou.manageiq-vmdb](https://galaxy.ansible.com/syncrou/manageiq-vmdb)

Example Playbook
----------------

Example where we create Enterprise with name `DEMO`:

```yaml
- name: Nuage event callback
  hosts: localhost
  connection: local
  gather_facts: False
  roles:
  - xlab_si.nuage_miq_automate
  tasks:
  - debug: msg="I am a playbook running as event callback and I am able to access nuage credentials"
  - debug: var=nuage_auth
  - debug: msg="As well as details of the event that triggered me"
  - debug: var=event
  - debug: msg="So I'm now ready to do some work!"
```

License
-------

BSD
