Role Name
=========

This roles provision configuration to a new instance of vSRX.

Requirements
------------

The role assumes that vSRX is already up and running with basic configuration. The role supports only provisioning of vSRX configuration. Now other devices are supported. The role must be applied to the host which has routable access to vSRX instance. Usually it is run on localhost

Role Variables
--------------

Variable | Mandatory? | Description
-------- | ---------- | -----------
vsrx_ip  | YES | IP address of running instance of vSRX
vsrx_user | YES | Username which will be used to connected to vSRX instance
vsrx_password | YES | Password to connect
vsrx_sshkeyfile | NO | path to user ssh key (could be used instead of password) 
function | NO | vSRX function which has to be applied, default it to apply basic configuration file. in future it can be extended


Dependencies
------------

No dependencies

Example Playbook
----------------

This playbook shows how to apply role to a new device with default function
```
- hosts: localhost
  roles:
    - { role: vsrx_provisioning, vsrx_ip: 10.13.84.55, vsrx_user: srxadmin, vsrx_password: somestongpassword }
    - { role: vsrx_provisioning, vsrx_ip: 10.13.84.55, vsrx_user: srxadmin, vsrx_sshkeyfile: ~\.ssh\id_rsa.pub }
```
The following config applies a custom configuration of vSRX inatnce
```
- hosts: localhost
  roles:
    - { role: vsrx_provisioning, vsrx_ip: 10.13.84.55, vsrx_user: srxadmin, vsrx_password: somestongpassword, function: myfunction }
```
The role assumes that config files exist for any functions which are defined and that admin user accounts are preconfigured in the base configuration template

License
-------

BSD

