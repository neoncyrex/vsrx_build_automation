# vsrx_build_automation
## Build automation for vSRX instances

Vsrx_build_automation is a set of Ansible playbooks that are designed to build, deploy and publish a Juniper vSRX instance in the Contrail environment. The goal was to create a PoC for automating provisioning of a unified Juniper vSRX firewall instance in customer private cloud with customized base configuration. 


Vsrx_build_automation could also be a base platform for CI/CD of multiple Juniper vSRX instances in private/public customer clouds. The written ansible playbooks could be used as base steps in CI/CD tool to visualize the whole deployment process.


## Default Playbook Roles

build_vsrx.yml - based on pre-defined varaibles deploys Juniper vSRX instance in Contrail environment, configures default admin user acount with                  SSH key, stops vSRX instance 

deploy_vsrx.yml - deploys Juniper vSRX instance in Contrail environment which was previously created
publish_vsrx.yml - creates Juniper vSRX snapshot and saves it on local storage
