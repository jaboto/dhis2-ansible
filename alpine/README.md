# Provision an Alpine with Ansible to get a DHIS2 instance working.

**STATUS**
Abandoned after 8 hours of work. Alpine and postgis compatibility requires to much work and might not be worth in a stable version. Edge packages could be considered in the future to overcome this issue.

It is required to pass several parameters in the configuration such as:
* Alpine repositories (if none default will be used)
* DHIS2 version
* DHIS2 Database (if none the default one will be downloaded and used)
* postgresql username and password

The target machine requires (by default) a SSH root connection so this needs to be modified in the target machine (sshd_config). Please note this **IS NOT** secure by default and should only be used for testing purposes.

## Preprovisioning
ansible-playbook -i inventory.yml playbook.yml --skip-tags provisioning

## Alpine specific hacks
As ansible fail to perfrom some tasks (i.e. https://github.com/ansible/ansible/pull/45507) some tasks are run by using direct commands instead of Ansible specific modules

