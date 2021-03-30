vmware-harbor
=========

An Ansible(tm) role Installs Harbor(tm) from VMware(tm) as the dependancies from Docker(tm).

* https://goharbor.io/

Currently deploys from a local file, tested on Ubuntu 18.04 and RHEL 7

Requirements
------------

openssl needed for self signed certificates

Port 80 and 443 is required

docker-ce (>1.8) is　required

docker-composer is required

Network is required for install docker-ce and docker-compose
Only offline install is tested


Example Playbook
----------------
```
- hosts: harbor
  become: true
  roles:
    - role: manueliglesiasgarcia.ansible_vmware_harbor
      vars:
        # Add a project
        harbor_projects:
          - project_name: myproject
            is_public: true
            content_trust: false
            prevent_vul: true
            severity: "high"
            auto_scan: true
        # Add a user
        harbor_users:
          - username: test
            email: test@test.com
            realname: test
            role_name: developer
            role_id: 2
            has_admin_role: true
        # Add a registry
        harbor_registry:
          - registry_name: "Test Harbor"
            registry_url: https://harbor.test/
        # Add an automatic registry pull
        harbor_replication_pull:
          - project_name: pullimagesfromtestharbor
            registry_name: "Test Pull Harbor"
            registry_url: harbor.test
        # Add an automatic registry push
        harbor_replication_push:
          - project_name: pushimagestotestharbor
            registry_name: "Test Push Harbor"
            registry_url: harbor.test
        # Add LDAP integration
        harbor_ldap:
          - auth_mode: ldap_auth
            ldap_url: ldap://test.com
            ldap_search_dn: CN=harbor,OU=ServiceAccounts,DC=test,DC=com
            ldap_search_password: password_service_account
            ldap_base_dn: DC=test,DC=com
            ldap_uid: sAMAccountName
        # Schedule an automatic scan of the docker images every hour
        harbor_schedule_scan:
          - cron: "0 0 * * * *"
        # Schedule an automatic garbage collection every hour
        harbor_schedule_gc:
          - cron: "0 0 * * * *"
        #Storge harbor installation file
        harbor_install_tmp: /root/harbor
        harbor_extras:
          -  clair
          -  chartmuseum
        #Folder installed harbor
        harbor_install_dir: /opt/harbor
        # The cert and key path is located in your ansible master, not the target hosts
        # The default path for certs is controlled by this role See roles/default/main.yml
        harbor_ssl_cert: "{{playbook_dir}}/certs/harbor_test.crt"
        harbor_ssl_cert_key: "{{playbook_dir}}/certs/harbor.test.key"

        #ends up in  harbor.yml
        harbor_hostname: harbor.test
        harbor_db_password: admin
        harbor_admin_password: password_harbor
```

License
-------

BSD

Author Information
------------------
This is forked from for adapter new version of harbor and other features
https://github.com/ryanzhang/ansible-vmware-harbor
