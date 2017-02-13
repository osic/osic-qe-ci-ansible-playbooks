# OSIC-QE CI Ansible Playbooks
## Description
These are playbooks used to deploy basic CI Infrastructure used by OSIC-QE. Currently these allow for the creation of a
* CI Jenkins with default plugins
* CI Jenkins Build node that can be attached to an existing Jekins Master
* ElasicSearch/Kibana node using Kibana 4.5.3 and Elsaticsearch 2.3.4 (for compatibility with existing indexes and visualizations)

## Requirements
These have been tested with **Ubuntu 14.04** and **Ubuntu 16.04**. An assertion error will be raised if the target host is not either of these.

Addionally, **Ansible 2.2** or greater is needed on the deploy host due to module requirements. An assertion error will be raised if this is not deployed with 2.2 or greater. Ansible can be installed via pip.

**Note**, Jenkins Master/Build nodes do not have hard [hardware requirements](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins) for installation, but a minimum level of resources is recommended. ElasicSearch does **require** a minimum of 4GB of memory available, though this does with better with a greater amount and/or creating an additional swap space (not completed via playbooks).

## How To
* Clone Repo
* Create [Ansible Configuration](http://docs.ansible.com/ansible/intro_configuration.html) as required to access targets
* Create [Ansible Inventory](http://docs.ansible.com/ansible/intro_inventory.html) using groups as
  * `jenkins_master`
  * `jenkins_build`
  * `elastic`
* Run playbook of choice via `ansible-playbook -i hosts provision_all.yml`    
**Playbooks are not destructive and can be rerun if errors occur in initial deploy**

## Playbooks
* `provision_jenkins_master.yml` - provisions Jenkins build master installing default plugins and adding current Jenkins users from the [jenkins backup repo](https://github.com/osic/qa-zero-downtime-jenkins-configuration). **Due to backup plugin configuration, this does not configure backup on the newly build Jenkins to push backups to that repo, and that would require being done manually**
* `provision_jenkins_build.yml` - provisions server with requirements for using as an ssh slave for an existing Jenkins master. Would require manual ssh key creation/addition between nodes. By default via group_vars, build node installs f5fpc vpn client.
* `provision_jenkins.yml` - Creates jenkins master/build pair. Due to possible networking configuration differences, this does require manually adding build node to master via `Manage Jenkins >> Nodes` using the Jenkins SSH credential created on provision.
* `provision_elasticsearch.yml` - Creates ElasticSearch and Kibana node with OSIC-QE indexes and visualizations.
* `provision_all.yml` - Performs all functions above.
