# Provision a Debian machine with Ansible to get a DHIS2 instance working.
The target machine requires (by default) a SSH root connection so this needs to be modified in the target machine (sshd_config). Please note this **IS NOT** secure by default and should only be used for testing purposes. This could be abstracted in a Vagrant environment to simplify things but at the moment some issues were encountered with Vagrant and libvirt

The Debian system will be provisioned with the required PostgreSQL, Postigs and Java packages as recommended in [Implementation Guide](https://docs.dhis2.org/master/en/implementer/html/dhis2_implementation_guide_full.html#installation) and not the latest from Debian. For this required offical repositories from previous Debian versions are used.

It is required to pass several parameters in the configuration such as (check the provisioning.yml to define those variables):
* DHIS2 version (accepts releases i.e. 2.32 and subreleases i.e. 2.32.1
* postgresql username and password
* allow external access to the DB
* JVM memory parameters to run Tomcat 
* TODO: DHIS2 Database (if none the default one will be downloaded and used)


## Preprovisioning
Ansible requires Python to work. In order to install Python packages if the machine doesn't have it a preprovision *Playbook* needs to be run. This is required with a Debian minimal installation.
```
ansible-playbook -i inventory.yml preprovisioning.yml
```

## Provisioning
Run the provision *Playbook* in order to perform all the tasks (they have been split in 4 subtasks for easier management and possible future split among different VM's.
```
ansible-playbook -i inventory.yml provisioning.yml
```

## Accessing and using the DHIS2 instance
Access the instance at the defined port (by default 8080) and log in with the default credentials *admin* / *district*

## Notes
1. Due to a limitation with Ansible and running processes in the background while provisioning the Tomcat service needs to be call with awkward parametres (see include/dhis.yml). This is done in this way to follow exactly the DHIS2 Documentation of running Tomcat manually and not as a service

## TODO
* Migrate the Tomcat service to a systemd service (although this would differ from the DHIS2 Documentation)
* Include a message task which display the information about the deployed server (Memory / JVM options / Tomcat version / IP...)
* Include a higher level of abstraction with Vagrant
