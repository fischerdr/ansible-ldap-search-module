# Ansible LDAP Search Modules

Ansible provides no module to query a LDAP server in it's
[core](http://docs.ansible.com/ansible/modules_by_category.html).
As this is an useful functionality this module was written.

# Requirements

* python-ldap library
  (install this library to the local and remote machine if needed)

## Modules Installation

You can clone this repository anywhere on your local machine and either
set the `ANSIBLE_LIBRARY` env variable or the `library` directive in the
`defaults` section of your `ansible.cfg`.

### Usage

When no credentials are provided, the request will
be made via a SASL `EXTERNAL` binding (similar to `ldapadd ... -Y
EXTERNAL ...`).

To specify credentials for the task you can use the following keywords as
shown in the example:

```yaml
- hosts: hostnameXY
  tasks:
  - name: Query all users in ou=People
    ldap_search:
	  bind_dn: "cn=admin,dc=example,dc=com"
	  bind_pw: "password"
      dn: "ou=People,dc=example,dc=com"
      ou: People
      objectClass: organizationalUnit
      description: A bunch of Ansible-lovers.
```

This example performs a task for each LDAP user `account`:

```yaml
- name: 
  ldap_search:
    base: "ou=People,dc=example,dc=com"
	scope: "onelevel"
	filter: "(objectClass=account)"
  register: ldap_user_search

- debug:
    msg: ldap_user_search

- name: Iterate through the LDAP searchresult
  # ...
  loop: "{{ ldap_user_search.results }}"
```

