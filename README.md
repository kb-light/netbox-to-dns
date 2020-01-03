Netbox to DNS
=============

Ansible role to generate DNS zone files out of Netbox.

Requirements
------------

This role needs the Python package `netaddr` installed on the Ansible Controlnode.
Additional one need to define a handler called `restart nameserver` which will be executed when a zone file changes.

Role Variables
--------------

You need to set the folllowing variables. For more configuration see `default/main.yml`.

```yaml
# Default domain, used for autogenerate missing domain names
nbdns_default_domain: example.com

# List of nameservers for the DNS zones. Used to generate the 'NS' records
nbdns_nameserver:
- ns.example.com

# Email address for 'SOA' entry. The '@' will be replaced by '.' automatically.
nbdns_email: ns@example.com

# Folder for placing the zone files. Folder shoud be already present.
nbdns_zones_path: /etc/bind

# List of the DNS zones. Define 'subnet' if it is a reverse zone. 'entries' allows to specify additional records.
nbdns_zones:
- domain: example.com
  entries:
  - name: www.example.com
    type: CNAME
    value: host01.example.com.
- domain: 2.0.192.in-addr.arpa
  subnet: 192.0.2.0/24

# URL to the Netbox API
nbdns_netbox_api_url: https://netbox.example.com/api

# Token to authenticate against the Netbox API
nbdns_netbox_token: 0123456789abcdef0123456789abcdef01234567
```

Example Playbook
----------------

```yaml
- hosts: nameservers
  handlers:
  - name: restart nameserver
    service:
      name: ...
      state: restarted
    become: yes
  roles:
  - role: netbox-to-dns
```

License
-------

BSD-2-Clause
