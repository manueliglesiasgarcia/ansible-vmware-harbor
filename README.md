vmware-harbor
=========

An Ansible(tm) role Installs Harbor(tm) from VMware(tm) as the dependancies from Docker(tm).

* http://vmware.github.io/harbor/

Currently deploys from a local file, tested on RHEL 7

Requirements
------------

openssl needed for self signed certificates

Port 80 and 443 is required

docker-ce (>1.8) is　required

docker-compse is required

Network is required for install docker-ce and docker-compose
Only offline install is tested

harbor version > harbor-offline-installer-v1.9.1-rc1.tgz (newer version harbor use harbor.yml instead harbor.cfg )


Example Playbook
----------------
```
- hosts: registries
  become: true
  roles:
    - role: rzhang.vmware-harbor
      vars:
        #Storge harbor installation file
        - harbor_install_tmp: /root/harbor
        #Folder installed harbor
        - harbor_install_dir: /opt/harbor
        - harbor_install_upload_localcopy_of_installer: /somelocalpath/harbor-offline-installer-v1.9.1-rc1.tgz
        # The cert and key path is located in your ansible master, not the target hosts
        # The default path for certs is controlled by this role See roles/default/main.yml
        - harbor_ssl_cert: "yourlocalcertpath"
        - harbor_ssl_cert_key: "yourlocalcertificatekeypath"
        #ends up in  harbor.yml
        - harbor_hostname: registry.dev.example.com
        - harbor_db_password: root123
        - harbor_admin_password: Harbor12345
```


Dependencies
------------

None?

License
-------

BSD

Author Information
------------------
This is forked from for adapter new version of harbor and other features
https://github.com/mkgin/ansible-vmware-harbor
