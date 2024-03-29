openvpn
==============

Ansible role which manage openvpn server

* Install and setup OpenVPN server
* Create/revoke client's configurations and certificates
* Setup authentication with PAM (System, passwd files)

#### Requirements

Only tested on ubuntu for now.

#### Variables

```yaml
openvpn_enabled: yes                                # The role is enabled

openvpn_etcdir: /etc/openvpn
openvpn_keydir: "{{openvpn_etcdir}}/keys"

# Default settings (See OpenVPN documentation)
openvpn_host: "{{inventory_hostname}}"              # The server address
openvpn_port: 1194
openvpn_proto: udp
openvpn_dev: tun
openvpn_server: 10.8.0.0 255.255.255.0
openvpn_max_clients: 100
openvpn_log: /var/log/openvpn.log                   # Log's directory
openvpn_keepalive: "10 120"
openvpn_ifconfig_pool_persist: ipp.txt
openvpn_comp_lzo: yes                               # Enable compression
openvpn_status: openvpn-status.log
openvpn_verb: 3
openvpn_user: nobody
openvpn_group: nogroup
openvpn_resolv_retry: infinite
openvpn_server_options: []                          # Additional server options
                                                    # openvpn_server_options:
                                                    # - dev-node MyTap
                                                    # - client-to-client
openvpn_client_options: []                          # Additional client options
                                                    # openvpn_client_options:
                                                    # - dev-node MyTap
                                                    # - client-to-client

openvpn_key_country: US
openvpn_key_province: CA
openvpn_key_city: SanFrancisco
openvpn_key_org: Fort-Funston
openvpn_key_email: me@myhost.mydomain
openvpn_key_size: 1024

openvpn_clients: [client]                         # Make clients certificate
openvpn_clients_revoke: []                        # Revoke clients certificates

# Use PAM authentication
openvpn_use_pam: yes
openvpn_use_pam_users: []                         # If empty use system users
                                                  # otherwise use users from the option
                                                  # openvpn_use_pam_users:
                                                  # - { name: user, password: password }
                                                  
# Use LDAP authentication (default is disabled)
openvpn_use_ldap: no
openvpn_ldap_tlsenable: 'no'
openvpn_ldap_follow_referrals: 'no'

# To use LDAP you must configure the following vars with real:
openvpn_ldap_server: ldap.mycompany.net
openvpn_ldap_bind_dn: 'user@mycompany.net'
openvpn_ldap_bind_password: password
openvpn_ldap_base_dn: ou=CorpAccounts,dc=mycompany,dc=net
openvpn_ldap_search_filter: '"sAMAccountName=%u"'
openvpn_ldap_group_search_filter: '"cn=OpenVPNUsers"'

```

#### Usage

Add `Stouts.openvpn` to your roles and set vars in your playbook file.

Example:

```yaml

- hosts: all

  roles:
  - Stouts.openvpn

  vars:
  openvpn_use_pam: yes
  openvpn_clients: [myvpn]
  openvpn_use_pam_users:
  - { name: user1, password: password1 }
  - { name: user2, password: password2 }

```

Install and copy client's configuration from `/etc/openvpn/keys/myvpn.tar.gz` file.

#### License

Licensed under the MIT License. See the LICENSE file for details.

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Stouts/Stouts.openvpn/issues)!
